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
    DBI["DrumBrute Impact"]
    DFAM["DFAM"]
    MODELD["Model D"]
    SWAP["Shruthi-1 ⇄ Donner B1"]
    SUBH["Subharmonicon"]
    XD["Minilogue XD"]
    L6["L6max"]
    FX["MS-70CDR+"]
    MON["Monitors"]

    %% ----- CV / gate -----
    HAPAX c1@--> DFAM

    %% ----- MIDI -----
    HAPAX -.-> THRU
    THRU -.-> DBI
    THRU -.-> MODELD
    THRU -.-> SWAP
    THRU -.-> SUBH
    THRU -.-> XD
    XD -.-> HAPAX
    HAPAX -.-> L6

    %% ----- audio -----
    DBI -- "mix" --> L6
    DBI -- "kick" --> L6
    DFAM --> L6
    MODELD --> L6
    SWAP --> L6
    SUBH --> L6
    XD --> L6
    L6 -- "send" --> FX
    FX -- "return" --> L6
    L6 --> MON

    %% ----- carry no signal, they only steer the layout -----
    %% DFAM among the voices instead of pushed to an end of the column,
    %% monitors out of the FX pedal's column
    THRU ~~~ DFAM
    FX ~~~ MON

    class HAPAX,THRU seq
    class DBI,DFAM,MODELD,SWAP,SUBH,XD voice
    class L6 mix
    class FX fx
    class MON mon
    class LA,LB,LC,LD,LE,LF pin
    class c1,k1 cv
```

|             | DrumBrute Impact | DFAM               | Model D | Shruthi-1 ⇄ Donner B1 | Subharmonicon | Minilogue XD | DrumBrute kick | MS-70CDR+ |
| ----------- | ---------------- | ------------------ | ------- | --------------------- | ------------- | ------------ | -------------- | --------- |
| Hapax track | 1                | 2                  | 3       | 4                     | 5             | 6            | 1              |           |
| Thru out    | 5                |                    | 1       | 2                     | 3             | 4            | 5              |           |
| MIDI ch     | 1                |                    | 3       | 4                     | 5             | 6            | 1              |           |
| CV / gate   |                  | gate 1 → ADV/CLOCK |         |                       |               |              |                |           |
| L6max strip | 1 (mono mix)     | 2                  | 3       | 4                     | 5             | 6 (stereo)   | 7              | 8 ← aux 1 |
