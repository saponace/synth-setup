```mermaid
---
config:
  theme: neutral
---
flowchart LR
    %% ----- One color per connection group -----
    classDef midi stroke:#4c9aff,color:#4c9aff
    classDef cv stroke:#66a80f,color:#66a80f
    classDef mixer stroke:#f08c00,color:#f08c00
    classDef fx stroke:#be4bdb,color:#be4bdb
    classDef monitoring stroke:#fa5252,color:#fa5252

    %% ===== Sequencer brain =====
    HAPAX["Hapax"]
    SPLIT["Thru box"]
    SPARECV["spare CV out (unpatched)"]

    %% ===== Synths =====
    MODELD["Model D"]
    SWAP["Shruthi-1 ⇄ Donner B1"]
    XD["Minilogue XD"]
    DBI["DrumBrute"]
    DFAM["DFAM"]
    SUBH["Subharmonicon"]

    %% ===== Mixer / send  =====
    SUBMIX["2ch submixer"]:::mixer
    MIX["Mackie Mix8"]:::mixer
    MON["Monitors"]:::monitoring
    ZOOM["Zoom MS-70CDR+"]:::fx

    %% invisible alignment aid: keeps the direct-to-mixer synths from
    %% drifting a column later than DFAM/Subharmonicon (whose longer
    %% path via the submixer otherwise pulls Mackie Mix8 further out)
    PAD[" "]
    style PAD fill:none,stroke:none

    %% ----- MIDI links -----
    HAPAX midi0@-. "out 1" .-> SPLIT
    SPLIT midi1@-. "ch 1" .-> MODELD
    SPLIT midi2@-. "ch 5" .-> DBI
    SPLIT midi3@-. "ch 3" .-> SWAP
    SPLIT midi4@-. "ch 4" .-> XD
    SPLIT midi5@-. "ch 2" .-> SUBH
    XD midi6@-. "in 1" .-> HAPAX
    class midi0,midi1,midi2,midi3,midi4,midi5,midi6 midi

    %% ----- CV links (pitch/mod) -----
    HAPAX cv1@-. "LFO (CV 4)" .-> SPARECV
    class cv1 cv

    %% ----- Audio links -----
    DFAM audio1@--> SUBMIX
    SUBH audio2@--> SUBMIX
    MODELD audio3@-- "ch 1" --> MIX
    SUBMIX audio4@-- "ch 2" --> MIX
    SWAP audio5@-- "ch 3" --> MIX
    XD audio6@-- "ch 4 (stereo)" --> MIX
    DBI audio7@-- "tape in" --> MIX
    class audio1,audio2,audio3,audio4,audio5,audio6,audio7 mixer

    MODELD ~~~ PAD
    SWAP ~~~ PAD
    XD ~~~ PAD
    DBI ~~~ PAD
    PAD ~~~ MIX

    MIX out@-- "main out" --> MON
    class out monitoring

    MIX fxSend@-- "aux send" --> ZOOM
    ZOOM fxReturn@-- "aux return (stereo)" --> MIX
    class fxSend,fxReturn fx
```

**Legend**

- MIDI (blue): dotted arrow
- CV (green): dotted arrow
- Audio/mixer (orange): solid arrow
- FX loop (purple): solid arrow
- Monitoring (red): solid arrow

## Hapax track map

Hapax has 16 tracks (any mix of MIDI or CV/Gate) and 4 physical CV/Gate pairs. Pair 4's CV (CV 4) is reserved for the spare LFO out; pairs 1-3 are fully free. Proposed assignment:

| Track | Type | Destination                            | Channel / CV pair    |
|-------|------|-----------------------------------------|------------------------|
| 1     | MIDI | Model D                                  | ch 1 (via Thru box)    |
| 2     | MIDI | Subharmonicon (transpose + clock)         | ch 2 (via Thru box)    |
| 3     | MIDI | Shruthi-1 ⇄ Donner B1 (swap)              | ch 3 (via Thru box)    |
| 4     | MIDI | Minilogue XD                             | ch 4 (via Thru box)    |
| 5     | MIDI | DrumBrute                                | ch 5 (via Thru box)    |
| 6–16  | —    | free                                     | —                       |

Notes:
- Subharmonicon has no CV/Gate connection — its MIDI in shares ch 2 with the other synths, but incoming MIDI only transposes its own onboard 4-step/polyrhythmic sequencer and syncs its internal clock, rather than dictating notes 1:1 like CV+Gate would.
- Subharmonicon relays clock to DFAM (not drawn above — a direct arrow between them would force the two into different columns): its SEQ CLK output tracks its internal clock, giving DFAM's Clock In a tempo-locked signal with no cable needed from Hapax.
- DFAM's pitch CV+Gate is deferred: it currently runs its own onboard 8-step sequencer, only clock-synced via Subharmonicon.
