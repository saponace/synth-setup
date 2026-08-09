```mermaid
---
config:
  theme: neutral
  flowchart:
    curve: monotoneX
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
    SWAP["Shruthi-1 ⇄ Donner B1"]
    XD["Minilogue XD"]
    DFAM["DFAM"]
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
    %% (~~~ keeps the voices in one column despite the Subharmonicon
    %% and DFAM reaching the mixer through the submixer, and keeps the
    %% monitors out of the Zoom's column)
    MODELD ~~~ SUBMIX
    SWAP ~~~ SUBMIX
    XD ~~~ SUBMIX
    DBI ~~~ SUBMIX
    ZOOM ~~~ MON
    MIX -- "send" --> ZOOM
    ZOOM -- "return" --> MIX
    SUBH --> SUBMIX
    DFAM --> SUBMIX
    SUBMIX --> MIX
    MODELD --> MIX
    SWAP --> MIX
    XD --> MIX
    DBI --> MIX
    MIX --> MON

    class HAPAX,THRU seq
    class MODELD,SUBH,SWAP,XD,DFAM,DBI voice
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
