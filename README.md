```mermaid
---
config:
  theme: neutral
---
flowchart LR
    %% ----- One color per connection group -----
    classDef midi stroke:#4c9aff,color:#4c9aff
    classDef clock stroke:#12b886,color:#12b886
    classDef cv stroke:#66a80f,color:#66a80f
    classDef mixer stroke:#f08c00,color:#f08c00
    classDef fx stroke:#be4bdb,color:#be4bdb
    classDef monitoring stroke:#fa5252,color:#fa5252

    %% ===== Sequencer brain =====
    HAPAX["Squarp Hapax"]
    SPLIT["Thru box"]

    %% ===== Synths =====
    MODELD["Behringer Model D"]
    SWAP["Shruthi-1 ⇄ Donner B1"]
    XD["Korg Minilogue XD"]
    DBI["DrumBrute Impact"]
    DFAM["Moog DFAM"]
    SUBH["Moog Subharmonicon"]
    SPARECV["spare CV out (unpatched)"]

    %% ===== Mixer / send  =====
    SUBMIX["2ch submixer (TBD model)"]:::mixer
    MIX["Mackie Mix8"]:::mixer
    ZOOM["Zoom MS-70CDR+"]:::fx
    MON["Monitors"]:::monitoring

    %% ----- MIDI links -----
    HAPAX midi0@-.-> SPLIT
    SPLIT midi1@-. "ch 1" .-> MODELD
    SPLIT midi2@-. "ch 2" .-> DBI
    SPLIT midi3@-. "ch 3" .-> SWAP
    SPLIT midi4@-. "ch 4" .-> XD
    XD midi5@-. "controller in" .-> HAPAX
    HAPAX midi6@-. "ch 5 (transpose + clock)" .-> SUBH
    class midi0,midi1,midi2,midi3,midi4,midi5,midi6 midi

    %% ----- CV links (pitch/mod) -----
    HAPAX cv1@-. "CV+Gate" .-> DFAM
    HAPAX cv3@-. "LFO" .-> SPARECV
    class cv1,cv3 cv

    %% ----- Clock links -----
    HAPAX clk1@-. "clock" .-> DFAM
    class clk1 clock

    %% ----- Audio links -----
    MODELD audio1@-- "ch 1" --> MIX
    SWAP audio2@-- "ch 3" --> MIX
    XD audio3@-- "ch 4 (stereo)" --> MIX
    DBI audio4@-- "tape in" --> MIX
    DFAM audio5@--> SUBMIX
    SUBH audio6@--> SUBMIX
    SUBMIX audio7@-- "ch 2" --> MIX
    class audio1,audio2,audio3,audio4,audio5,audio6,audio7 mixer

    MIX fxSend@-- "aux send" --> ZOOM
    ZOOM fxReturn@-- "aux return (stereo)" --> MIX
    class fxSend,fxReturn fx

    MIX out@-- "main out" --> MON
    class out monitoring
```

**Legend**

- MIDI (blue): dotted arrow
- Clock/sync (teal): dotted arrow
- CV (green): dotted arrow
- Audio/mixer (orange): solid arrow
- FX loop (purple): solid arrow
- Monitoring (red): solid arrow

## Hapax track map

Hapax has 16 tracks (any mix of MIDI or CV/Gate) and 4 physical CV/Gate pairs. Proposed assignment:

| Track | Type    | Destination                                  | Channel / CV pair          |
|-------|---------|-----------------------------------------------|-----------------------------|
| 1     | MIDI    | Behringer Model D                             | ch 1 (via Thru box)         |
| 2     | MIDI    | DrumBrute Impact                              | ch 2 (via Thru box)         |
| 3     | MIDI    | Shruthi-1 ⇄ Donner B1 (swap)                  | ch 3 (via Thru box)         |
| 4     | MIDI    | Korg Minilogue XD                             | ch 4 (via Thru box)         |
| 5     | CV/Gate | Moog DFAM                                     | pair 1                      |
| 6     | MIDI    | Moog Subharmonicon (transpose + clock)        | ch 5 (direct cable, not via Thru box) |
| 7     | Gate    | Clock → DFAM Clock In                         | pair 3 (gate only, CV spare)|
| 8     | CV      | Spare / LFO, unpatched                        | pair 4 (CV only, gate spare)|
| 9–16  | —       | free                                          | —                            |

Notes:
- Minilogue XD is bidirectional: it receives sequenced notes from Hapax on ch 4 (via the Thru box) and also sends its own keybed as a live controller directly into a Hapax MIDI in — this is an input routing setting, not a separate track.
- Subharmonicon has no CV/Gate connection at all: its front-panel patchbay MIDI in (3.5mm TRS Type A, same standard as Hapax) takes a direct cable from Hapax. Incoming MIDI only transposes Subharmonicon's own onboard 4-step/polyrhythmic sequencer and syncs its internal clock — it does not let Hapax dictate notes 1:1 the way CV+Gate would. This preserves Subharmonicon's native semi-autonomous character instead of reducing it to a plain monophonic voice.
- DFAM still gets a single CV+Gate pair (both VCOs driven together as one voice), not per-VCO CV.
- CV/Gate pair 2 (previously reserved for Subharmonicon) is now fully free for future use.
- Hapax uses 2 of its 4 MIDI outs (one to the Thru box, one direct to Subharmonicon) and 1 of its 2 MIDI ins (Minilogue controller); the rest are spare for future direct connections.

## Open questions

- Exact make/model of the 2ch submixer taking DFAM + Subharmonicon before Mix8 ch 2.
