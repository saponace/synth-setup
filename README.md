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
    HAPAX["Hapax"]
    SPLIT["Thru box"]
    SPARECV[" "]
    style SPARECV fill:none,stroke:none

    %% ===== Synths =====
    MODELD["Model D"]
    SUBH["Subharmonicon"]
    SWAP["Shruthi-1 ⇄ Donner B1"]
    XD["Minilogue XD"]
    DBI["DrumBrute"]
    DFAM["DFAM"]

    %% ===== Mixer / send  =====
    SUBMIX["2ch submixer"]:::mixer
    MIX["Mackie Mix8"]:::mixer
    MON["Monitors"]:::monitoring
    ZOOM["Zoom MS-70CDR+"]:::fx

    %% ----- MIDI links -----
    HAPAX midi0@-. "out 1" .-> SPLIT
    SPLIT midi1@-. "ch 1" .-> MODELD
    SPLIT midi5@-. "ch 2" .-> SUBH
    SPLIT midi3@-. "ch 3" .-> SWAP
    SPLIT midi4@-. "ch 4" .-> XD
    SPLIT midi2@-. "ch 5" .-> DBI
    XD midi6@-. "keyboard control" .-> HAPAX
    class midi0,midi1,midi2,midi3,midi4,midi5,midi6 midi

    %% ----- CV links (pitch/mod) -----
    HAPAX cv1@-. "LFO (CV 4)" .-> SPARECV
    class cv1 cv

    %% ----- Clock links -----
    SUBH clk1@-. "clock" .-> DFAM
    class clk1 clock

    %% ----- Audio links -----
    %% (the ~~~ invisible links anchor each direct-to-mixer synth to
    %% the submixer's rank/column, keeping all six synths aligned since
    %% DFAM/Subharmonicon's longer path via the submixer would otherwise
    %% pull Mackie Mix8 further out and split the column)
    MODELD ~~~ SUBMIX
    DFAM audio1@--> SUBMIX
    MODELD audio3@-- "ch 1" --> MIX
    SUBH audio2@--> SUBMIX
    SUBMIX audio4@-- "ch 2" --> MIX
    SWAP audio5@-- "ch 3" --> MIX
    XD audio6@-- "ch 4 (stereo)" --> MIX
    DBI audio7@-- "tape in" --> MIX
    SWAP ~~~ SUBMIX
    XD ~~~ SUBMIX
    DBI ~~~ SUBMIX
    class audio1,audio2,audio3,audio4,audio5,audio6,audio7 mixer

    MIX out@-- "main out" --> MON
    class out monitoring

    MIX fxSend@-- "aux send" --> ZOOM
    ZOOM fxReturn@-- "aux return (stereo)" --> MIX
    class fxSend,fxReturn fx
```

**Legend**

- MIDI (blue): dotted arrow
- Clock/sync (teal): dotted arrow
- CV (green): dotted arrow
- Audio/mixer (orange): solid arrow
- FX loop (purple): solid arrow
- Monitoring (red): solid arrow
