# Hapax setup checklist

A build sheet for configuring the physical Hapax to match `README.md`'s diagram. Hapax's project file itself is a proprietary binary format saved only by the device — there's nothing to hand-author there, so this doc is what you actually go through on the hardware's track/settings menus.

Physical MIDI ports on Hapax are labeled **A/B/C/D** (plus USB), not numbered — the diagram's "out 1"/"in 1" labels are placeholders. Fill in the real letters below once confirmed, then we'll fix the diagram labels to match.

## Open items (need your input)

- [ ] Which physical Hapax MIDI port (A/B/C/D) goes to the Thru box? Assumed **A** below.
- [ ] Which physical Hapax MIDI port does Minilogue XD's MIDI OUT plug into? Assumed **B** below.
- [ ] Minilogue XD's own MIDI transmit channel (global channel setting on the Minilogue itself) — needed for track 16's input channel.
- [ ] Project tempo (BPM).
- [ ] Should Minilogue XD's own sequenced pattern (track 4) also respond to track 16 transpose, or stay fixed while you play live transposing lines on top of it? Defaulted to ON below — flip to OFF if you want it fixed.

## Global project settings

| Setting | Value |
|---|---|
| Tempo | *(TBD — your call)* |
| Time signature | 4/4 unless a track needs otherwise |
| Clock source | Internal (Hapax is the master clock for everything; Subharmonicon gets clock via its MIDI in on ch 2, then relays it to DFAM — nothing needs external sync) |

## Track-by-track

| Track | Name | Output port | Out ch | Input port | In ch | Track-16 transpose | Notes |
|---|---|---|---|---|---|---|---|
| 1 | Model D | A (→ Thru box) | 1 | — | — | ON | Monosynth — benefits from live key changes |
| 2 | Subharmonicon | A (→ Thru box) | 2 | — | — | ON | This *is* how you communicate key/chord changes to it — the outgoing note on ch 2 is what transposes its onboard sequencer |
| 3 | Shruthi-1 ⇄ Donner B1 | A (→ Thru box) | 3 | — | — | ON | |
| 4 | Minilogue XD | A (→ Thru box) | 4 | — | — | ON *(confirm — see open items)* | |
| 5 | DrumBrute | A (→ Thru box) | 5 | — | — | **OFF** | Critical: drum note numbers map to specific voices — transposing scrambles which drum plays |
| 16 | Transpose / keyboard control | none needed | — | B (← Minilogue XD) | *(Minilogue's TX channel)* | n/a (this is the leader) | Start with plain Transpose mode; try Match Chord mode later for full live reharmonization instead of simple pitch shift |
| 6–15 | free | — | — | — | — | — | Reserve one for DFAM's CV+Gate pitch track whenever that gets wired in (any of CV/Gate pairs 1-3) |

## DFAM (deferred, not wired yet)

When you're ready to add DFAM's pitch CV+Gate: pick any free track (6–15), set its type to CV/Gate, and assign it to CV/Gate pair 1, 2, or 3 (pair 4's CV is already spoken for by the spare LFO output). No MIDI channel needed since it's a CV/Gate track.
