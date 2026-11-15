# FM-FT8 specification

Version 0.1 (draft), 2026-11-15. Stage A: standard FT8 via conventional FM.

This document writes down the configuration I propose for the experiment. It is a personal
proposal, not a standard. Every statement carries one of four labels, so that nobody
mistakes a proposal or a guess for a fact:

| Label | Meaning |
|---|---|
| **[FT8]** | Established property of the FT8 protocol or of WSJT-X. Not ours to change. |
| **[Proposed]** | An operating parameter I propose. A suggestion, not a rule. |
| **[Assumption]** | My engineering reasoning, not verified. |
| **[To validate]** | Needs a measurement. Linked to a test in [the test plan](../testing/TEST_PLAN.md). |

## 1. Scope

FM-FT8 sends the standard FT8 audio signal through the microphone input of a conventional
analogue FM transceiver and decodes the audio output of a conventional FM receiver.
Nothing in the FT8 protocol, the message format or WSJT-X is modified. **[Proposed]**

FM-FT8 is not a new mode. A contact is logged as an FT8 contact on 2 m.

## 2. The FT8 signal

These values come from the protocol description by Franke, Somerville and Taylor
("The FT4 and FT8 Communication Protocols", QEX, July/August 2020) and the WSJT-X User Guide.

| Property | Value | |
|---|---|---|
| Modulation | 8-GFSK: eight-tone frequency-shift keying, continuous phase, Gaussian-smoothed transitions | **[FT8]** |
| Tone spacing = symbol rate | 6.25 Hz = 6.25 baud | **[FT8]** |
| Occupied bandwidth | 50 Hz | **[FT8]** |
| Symbols per transmission | 79 (58 data, 21 synchronisation in three 7x7 Costas arrays) | **[FT8]** |
| Transmission length | 12.64 s | **[FT8]** |
| Time slots | 15 s, alternating "even/first" and "odd/second" | **[FT8]** |
| Nominal start | 0.5 s after the start of the slot | **[FT8]** |
| Payload | 77 bits + 14-bit CRC, LDPC (174,91) forward error correction | **[FT8]** |
| Envelope | Constant amplitude: one tone at any instant | **[FT8]** |
| Decoding threshold on a linear (SSB) channel | about -21 dB signal-to-noise in a 2500 Hz reference bandwidth | **[FT8]** |

The decoding threshold applies to a linear channel. It does **not** carry over to FM;
see section 6.

## 3. Radio channel

| Parameter | Value | |
|---|---|---|
| Band | 2 m | **[Proposed]** |
| Frequency | 144.575 MHz | **[Proposed]** |
| Operation | Simplex, no repeater shift, no CTCSS/DCS | **[Proposed]** |
| Modulation | Narrow FM (F3E-type analogue frequency modulation as produced by the radio's microphone path) | **[Proposed]** |
| Channel setting | 12.5 kHz ("narrow") | **[Proposed]** |
| Nominal peak deviation limit in this setting | +/- 2.5 kHz | **[Assumption]** - typical for amateur and PMR equipment; differs by radio |
| Antenna polarisation | Vertical | **[Proposed]** |
| Transmit power | The lowest that completes the contact | **[Proposed]** |
| Operation | Attended only | **[Proposed]** - the band plan allows no unmanned stations in this segment |

The status of 144.575 MHz and the band plan context are in
[FREQUENCY_PLAN.md](FREQUENCY_PLAN.md).

70 cm: the same configuration is expected to work. No frequency is proposed yet.

## 4. Audio

| Parameter | Value | |
|---|---|---|
| Audio connection | Wired, both directions | **[Proposed]** |
| Transmit audio frequency (WSJT-X "Tx" marker) | 1000 to 2000 Hz | **[Proposed]** |
| Transmit level | Set so the radio's limiter is not reached; method in test 4 | **[Proposed]**, procedure **[To validate]** |
| Target deviation | Not defined yet | **[To validate]** (test 4) |
| Receive level | WSJT-X level meter around 30 dB on open-squelch noise | **[Assumption]** - WSJT-X's general recommendation, not checked for FM noise |
| Squelch | Fully open | **[Proposed]** |
| Battery save, roger beep, voice compander, noise reduction | Off | **[Proposed]** |

Reasoning for 1000 to 2000 Hz **[Assumption]**: it keeps the signal away from the
edges of the speech filter (roughly 300 Hz to 3 kHz), and puts the third harmonic that
clipping would produce (3000 to 6000 Hz) at or beyond the upper edge of the audio filters.

## 5. Timing and PTT

| Parameter | Value | |
|---|---|---|
| Computer clock | Synchronised (NTP or GPS); error well below 1 s | **[FT8]** requirement in practice |
| PTT | Keyed by WSJT-X through a hardware line (serial RTS/DTR or equivalent) | **[Proposed]** |
| VOX | Fallback only | **[Proposed]** |
| Transmitter start-up delay (PTT to full RF with audio) | Unknown for typical handhelds | **[To validate]** (test 3) |
| Acceptable total start-up delay | Should stay well below 1 s; a late start shifts the measured time offset (DT) and cuts off the beginning of the first synchronisation block | **[Assumption]** |

## 6. Expected performance

Nothing in this section is measured.

| Statement | |
|---|---|
| FM-FT8 does **not** have the weak-signal performance of FT8 on SSB. An FM receiver needs a carrier-to-noise ratio above its threshold in the full channel bandwidth before it delivers usable audio; FT8's narrow bandwidth cannot recover what the FM demodulator has already lost. | **[Assumption]**, magnitude **[To validate]** (test 6) |
| Above the FM threshold the audio signal-to-noise ratio is high and decoding should be reliable. | **[Assumption]** (tests 2, 6) |
| Range is about that of FM voice with the same radios and antennas. | **[Assumption]** (test 8) |
| Two signals in the same slot on the same frequency: the stronger one is decoded if it exceeds the other by the receiver's capture ratio; otherwise neither. | **[Assumption]** (test 2, optional extension) |
| Pre-emphasis and de-emphasis have no significant effect on decoding, because FT8 spans only 50 Hz. | **[Assumption]** (test 5) |
| Narrow FM is at least as suitable as wide FM. | **[Assumption]** (test 7) |

## 7. Operating conventions

All **[Proposed]**:

1. Check local activity, your licence conditions and the current band plan before transmitting.
2. Listen first. The frequency is shared and not exclusive.
3. One at a time: do not transmit in a time slot another station is using, and let a
   running contact finish. No deliberate simultaneous transmissions.
4. Stay off 144.174 MHz, where FT8 is run on SSB.
5. Standard FT8 messages only; standard logging (mode FT8, band 2 m).

## 8. Open questions

- Target deviation and a simple way to set it without test equipment (test 4).
- Whether narrow or wide FM decodes better at the margin (test 7).
- Real start-up delays of common handhelds (test 3).
- Interoperability with SSB stations **[To validate]**: a tone that frequency-modulates a
  carrier at a low modulation index produces a sideband at carrier frequency + tone
  frequency. In principle an SSB receiver tuned to the FM carrier frequency could therefore
  see an FM-FT8 transmission as an ordinary FT8 signal, while an FM receiver cannot decode
  an FT8 transmission sent on SSB. This is untested and is not part of the proposal.
- A frequency for 70 cm.

## Change log

- 0.1 (2026-11-15): first draft.
