# FM-FT8 - FT8 via FM

**My proposal for an experiment: standard FT8 through ordinary FM radios on 2 m.**

[Deutsch](README.de.md) · Article: <https://dl1tj.de/fmft8.html> · Skimmer: <https://fmft8.db0rbx.de>

This is a personal proposal by me, Tobias Johnson, DL1TJ. It is not a standard, not an
official project and not coordinated with anyone. I am writing it down in the open so that
others can try it, check it and tell me where it is wrong.

> **Status: an idea, not yet tested on the air.** I have made no contact this way yet and
> measured nothing. Every statement about range or performance here is what I expect, not
> what I know.

## The idea

FT8 is the most approachable digital mode we have: structured messages, automatic
sequencing, contacts that complete and log themselves. On 2 m it is run on SSB, and an
SSB-capable 2 m transceiver costs many times what an FM handheld does. So the people who
would enjoy FT8 most as a first step - newcomers, in Germany especially Class N and Class E
licensees - often cannot try it.

But FT8 is audio, and an FM radio can carry audio. My proposal: run **standard, unmodified
FT8** with **WSJT-X** through an **ordinary FM handheld**, and agree on a calling frequency
to find each other.

```
 FM radio  <-->  sound card + PTT  <-->  computer  <-->  WSJT-X
 (2 m,           (audio cable,           (USB,           (decodes, sends,
  narrow FM)      hardware PTT)           exact clock)    logs)
```

## What to expect

This is an entry point into digital operating, not a weak-signal mode.

- **No weak-signal advantage.** FT8 on SSB decodes far below the noise. Through an FM
  receiver most of that is lost. Expect roughly FM voice range.
- **One station at a time.** FM receivers lock onto the strongest signal, so two stations
  in the same time slot block each other.
- **Everything else is real FT8.** Same software, messages, timing and logging as on SSB
  and HF, so what you learn carries over unchanged.

Why this is so: [docs/TECHNICAL_BACKGROUND.md](docs/TECHNICAL_BACKGROUND.md).

## Proposed setup

| Parameter | Proposed |
|---|---|
| Frequency | 144.575 MHz, simplex |
| Modulation | Narrow FM, 12.5 kHz channel setting |
| Digital protocol | Standard FT8, unmodified |
| Software | WSJT-X |
| Timing | Standard FT8: 15-second slots, computer clock synchronised |
| PTT | Hardware-controlled; VOX only as a fallback |
| Audio | Wired interface |
| Transmit power | The lowest that gets through |

144.575 MHz is my suggestion for an experimental calling frequency. It is not allocated to
FT8, not coordinated and not exclusive. Before you transmit, check local activity, your
licence conditions and the current IARU Region 1 band plan, and listen first.
See [docs/FREQUENCY_PLAN.md](docs/FREQUENCY_PLAN.md).

## Where things stand

- The proposal and a step-by-step guide are on my website, in
  [English](https://dl1tj.de/fmft8.html) and [German](https://dl1tj.de/de/fmft8.html).
- A receive-only skimmer at my station DB0RBX in Regensburg (JN69ba) listens on
  144.575 MHz FM around the clock: <https://fmft8.db0rbx.de>
- Not done yet: the reference interface, any on-air contact, any measurement.

## What I plan

Three stages, which I keep apart:

- **A. Standard FT8 via FM (now).** Build a reference interface, make the first contacts,
  and measure how well it really works. The [test plan](testing/TEST_PLAN.md) lists what I
  want to find out.
- **B. A digital mode made for FM (an idea for later).** FT8 was designed for SSB. A
  protocol designed for the FM channel could send a message in a second or two instead of
  13, with forward error correction, within one narrow channel, and still work with cheap
  handhelds. Another route would be LoRa modules. Nothing of this exists.
- **C. Accessible digital amateur radio (the long view).** An interface anyone can rebuild,
  notes on which radios work, real reception results, documentation in German and English.

## In this repository

| | |
|---|---|
| [docs/SPECIFICATION.md](docs/SPECIFICATION.md) | The proposed configuration, with facts, proposals and assumptions kept apart |
| [docs/TECHNICAL_BACKGROUND.md](docs/TECHNICAL_BACKGROUND.md) | FT8, FM, capture effect, audio processing, PTT timing, limits compared with SSB |
| [docs/OPERATING_GUIDE.md](docs/OPERATING_GUIDE.md) | Setting up the radio and WSJT-X, making a contact |
| [docs/FREQUENCY_PLAN.md](docs/FREQUENCY_PLAN.md) | Why 144.575 MHz, and what to respect there |
| [hardware/REFERENCE_SETUP.md](hardware/REFERENCE_SETUP.md) | The interface I intend to build (draft, untested) |
| [testing/TEST_PLAN.md](testing/TEST_PLAN.md), [RESULTS.md](testing/RESULTS.md) | What I want to measure, and what has been measured (nothing yet) |

## Try it, and tell me

If you try this, I would like to hear how it went, whether it worked or not: open an issue
here or write to <dl1tj@darc.de>, in English or German. Corrections to the technical
reasoning are just as welcome.

## Credits and licence

FT8 and WSJT-X are the work of Joe Taylor (K1JT), Steve Franke (K9AN), Bill Somerville
(G4WJS) and the WSJT development group. This proposal is my own and has no connection to
them, the DARC or the IARU.

Text in this repository: [CC BY 4.0](LICENSE).

Tobias Johnson, DL1TJ - <https://dl1tj.de>
