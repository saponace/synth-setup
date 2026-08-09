```mermaid
---
config:
  theme: neutral
  flowchart:
    curve: basis
---
flowchart LR
    %% box color = kind of gear, line style = kind of signal
    classDef seq fill:#e8f0fc,stroke:#2f6fd0,color:#1c1c20
    classDef voice fill:#e7f4ec,stroke:#1a8f5f,color:#1c1c20
    classDef mix fill:#fdf1de,stroke:#c77800,color:#1c1c20
    classDef fx fill:#f4ecfb,stroke:#7c4dbd,color:#1c1c20
    classDef mon fill:#fdecec,stroke:#c0392b,color:#1c1c20
    classDef pin fill:none,stroke:none
    classDef cv stroke-dasharray: 7 4

    %% legend, tied to Hapax by invisible links so it lands in the left columns
    LA[" "] -. "MIDI" .-> LB[" "]
    LC[" "] k1@-- "CV / gate" --> LD[" "]
    LE[" "] -- "audio" --> LF[" "]
    LB ~~~ HAPAX
    LD ~~~ HAPAX
    LF ~~~ HAPAX

    HAPAX["Hapax"]
    THRU["Thru box"]
    MODELD["Model D"]
    SUBH["Subharmonicon"]
    DFAM["DFAM"]
    SWAP["Shruthi-1 ⇄ Donner B1"]
    XD["Minilogue XD"]
    DBI["DrumBrute Impact"]
    ZOOM["Zoom MS-70CDR+"]
    SUBMIX["Moog submixer"]
    MIX["Mackie Mix8"]
    MON["Monitors"]

    %% ----- CV / gate -----
    HAPAX c1@--> DFAM

    %% ----- MIDI -----
    HAPAX -.-> THRU
    THRU -.-> MODELD
    THRU -.-> SUBH
    THRU -.-> SWAP
    THRU -.-> XD
    THRU -.-> DBI
    XD -.-> HAPAX

    %% ----- audio -----
    MODELD --> MIX
    SUBH --> SUBMIX
    DFAM --> SUBMIX
    SWAP --> MIX
    XD --> MIX
    DBI --> MIX
    SUBMIX --> MIX
    MIX -- "send" --> ZOOM
    ZOOM -- "return" --> MIX
    MIX --> MON

    %% ----- carry no signal, they only steer the layout -----
    %% RAIL is an invisible stand-in for the submixer: it holds the voices
    %% that bypass the submixer in one column with the rest, without the
    %% submixer itself being dragged down to the middle of all six of them
    RAIL[" "]:::pin
    MODELD ~~~ RAIL
    SWAP ~~~ RAIL
    XD ~~~ RAIL
    DBI ~~~ RAIL
    RAIL ~~~ MIX
    %% DFAM among the voices instead of pushed to an end of the column,
    %% monitors out of the Zoom's column
    THRU ~~~ DFAM
    ZOOM ~~~ MON

    class HAPAX,THRU seq
    class MODELD,SUBH,DFAM,SWAP,XD,DBI voice
    class SUBMIX,MIX mix
    class ZOOM fx
    class MON mon
    class LA,LB,LC,LD,LE,LF pin
    class c1,k1 cv
```

|             | Model D | Subharmonicon     | Shruthi-1 ⇄ Donner B1 | Minilogue XD | DFAM                | DrumBrute Impact |
| ----------- | ------- | ----------------- | --------------------- | ------------ | ------------------- | ---------------- |
| Hapax track | 1       | 2                 | 3                     | 4            | 5                   | 10               |
| Thru out    | 1       | 2                 | 3                     | 4            | —                   | 5                |
| MIDI ch     | 1       | 2                 | 3                     | 4            | —                   | 10               |
| CV / gate   | —       | —                 | —                     | —            | gate 1 → ADV/CLOCK  | —                |
| Mixer ch    | 1       | 2 (Moog submixer) | 3                     | 4 (stereo)   | 2 (Moog submixer)   | tape in          |
