# Technical background

How FT8 gets through an FM radio, what the radio does to it on the way, and why the result
is not the same as FT8 on SSB.

This document explains mechanisms. It follows from how FT8 and FM radios work, not from
measurements of my own. Where a number matters in practice, it is marked
**to be measured** and linked to the [test plan](../testing/TEST_PLAN.md).

## 1. What WSJT-X sends: the FT8 audio signal

FT8 is 8-GFSK: frequency-shift keying with eight tones, 6.25 Hz apart. One symbol lasts
0.16 s and selects one of the eight tones; a transmission is 79 symbols, 12.64 s long,
inside a 15-second time slot.

Three properties matter for FM:

- **It is one tone at a time.** The signal is a single sine wave whose frequency steps
  between eight values. Its amplitude is constant.
- **The steps are smooth.** The "G" in GFSK: the frequency glides from one tone to the next
  through a Gaussian filter, and the phase is continuous. There are no clicks.
- **It is narrow.** All eight tones fit in 50 Hz. In WSJT-X you choose where in the audio
  range that 50 Hz slice sits, for example at 1500 Hz.

On SSB, the transmitter shifts this audio signal up to radio frequency unchanged: an audio
tone at 1500 Hz becomes a radio signal 1500 Hz above the dial frequency, 50 Hz wide. That
is why dozens of FT8 stations fit side by side in one SSB channel.

## 2. What an FM radio does with it

An FM transmitter does not shift the audio up. It lets the audio voltage move the carrier
frequency back and forth. How far it moves is the **deviation**; narrow FM allows about
+/- 2.5 kHz, wide FM about +/- 5 kHz.

Feed in the FT8 tone at 1500 Hz and the carrier swings up and down 1500 times per second.
The louder the tone, the further it swings. On the air this is no longer a 50 Hz wide
signal: it occupies the whole FM channel, roughly 2 x (deviation + audio frequency), so
around 8 to 11 kHz in narrow FM.

The FM receiver reverses the process: it measures how the carrier frequency moves and
outputs that as audio. Out comes the 1500 Hz tone, stepping between its eight frequencies,
and WSJT-X decodes it as usual.

So FT8 survives FM. But two things are different in kind from SSB:

1. The signal on the air is wide, and the receiver must first receive that wide signal
   properly before any audio comes out.
2. One carrier, one conversation: an FM receiver demodulates one carrier at a time.

## 3. The FM threshold: why the weak-signal advantage is lost

FT8's reputation comes from decoding signals about 21 dB below the noise in a 2500 Hz
bandwidth. That works because on SSB the receiver is linear: signal and noise arrive at the
computer unchanged, and the decoder can concentrate on the 50 Hz where the signal is.

An FM demodulator is not linear. It needs the carrier to be clearly stronger than the noise
in the *whole* channel filter (roughly 8 to 12 kHz wide) before it works at all. Above that
point, the **FM threshold**, the audio is clean. Below it, the demodulator output breaks up
into clicks and noise within a few dB, and the tone is gone. Narrow post-detection
filtering cannot bring back what the demodulator never delivered.

Consequence: FM-FT8 works where an FM signal is receivable at all, and not much beyond.
The gap to FT8 on SSB is expected to be tens of dB. **To be measured** (test 6).

This is not a flaw to engineer around within Stage A. It is the price of using the FM radio
as it is.

## 4. Capture effect: the strongest signal wins

When two FM signals arrive on the same frequency, the demodulator follows the stronger one
and suppresses the weaker one almost completely, provided the difference exceeds the
receiver's **capture ratio** (a few dB; it depends on the receiver). If the two are about
equally strong, the output is a distorted mix of both.

For FT8 this means:

- Two stations transmitting in the same 15-second slot do not appear side by side on the
  waterfall as they would on SSB. At best you decode the stronger one. At similar strength
  you decode neither.
- It does not help that they use different audio tones. The collision happens at radio
  frequency, before the tones exist as audio again.
- Different receivers hear different winners, depending on who is stronger at each location.

Hence the operating convention: one station per slot, listen first, and no pile-ups.
On 2 m, where FM range is local and few stations share the frequency, this is a smaller
restriction than it would be on HF.

## 5. Audio processing in FM radios

An FM radio's audio path is built for speech. Between the microphone socket and the
modulator, and between the demodulator and the speaker socket, the FT8 tone passes:

**Band-pass filtering.** Roughly 300 Hz to 3 kHz. A signal placed at 1000 to 2000 Hz is
well inside.

**Pre-emphasis and de-emphasis.** The transmitter boosts higher audio frequencies by about
6 dB per octave; the receiver cuts them by the same amount. End to end the two cancel.
Within one transmission the effect is negligible: across FT8's 50 Hz at 1500 Hz the level
changes by less than 0.3 dB. What pre-emphasis does change is the deviation you get: for
the same audio level from the computer, a tone at 2000 Hz deviates about twice as far as a
tone at 1000 Hz. The level setting therefore belongs to a tone frequency.

**Limiting (clipping).** To keep loud voices inside the channel, a limiter cuts off audio
peaks before the modulator. A clipped sine wave is still a tone at the same frequency, plus
odd harmonics at 3x, 5x ... its frequency. With the tone above 1000 Hz, those harmonics
fall at or beyond the 3 kHz low-pass filter that follows the limiter. So moderate clipping
is expected to cost less than one might fear, but it wastes deviation, distorts the signal
and is avoidable. The clean way is to stay below the limiter. **To be measured** (tests 4, 5).

**Other processing.** Companders, noise reduction, "voice enhancement", automatic gain
control and battery-save receive cycling are designed around speech and should be switched
off. Their effect on FT8 is unknown and not worth finding out.

**Microphone input level.** A handheld's microphone input expects millivolts. A sound
card's line or headphone output delivers a volt. Without attenuation in the interface the
limiter is driven hard at the lowest slider settings. See
[hardware/REFERENCE_SETUP.md](../hardware/REFERENCE_SETUP.md).

## 6. PTT timing

FT8 transmissions start 0.5 s into the 15-second slot and last 12.64 s. Stations find each
other's signals by their synchronisation blocks at the start, middle and end, and WSJT-X
reports the time offset of each decoded signal as DT.

An FM handheld needs time between PTT and a clean modulated signal on the air: the
synthesiser settles, the power amplifier ramps up. On the receive side, the squelch (if
closed) and battery-save cycling add their own delay. Whatever is lost at the start is
missing from the first synchronisation block, and a late start shows up as DT.

- **Hardware PTT.** WSJT-X asserts PTT through a serial control line at the right moment,
  before the audio starts, and holds it until the end. Timing is repeatable.
- **VOX.** The radio or interface keys only once it detects audio, so the transmission
  always starts late, and a VOX with a short hang time can drop out. Acceptable as a
  fallback to get started, not as the reference.

How long common handhelds actually take is **to be measured** (test 3).

The computer clock matters as much as PTT: it should be synchronised by NTP or GPS to well
within a second.

## 7. Narrow or wide FM

| | Narrow (12.5 kHz) | Wide (25 kHz) |
|---|---|---|
| Peak deviation | about +/- 2.5 kHz | about +/- 5 kHz |
| Receiver filter | narrower, so less noise and a lower threshold in absolute terms | wider |
| Audio output for a given received deviation | scaled for 2.5 kHz | scaled for 5 kHz |
| Room taken on the band | less | more |

Narrow FM is the proposed reference because it occupies less spectrum next to the
neighbours in the all-mode segment, and because a narrower receive filter lets in less
noise. Wide FM gives more audio signal-to-noise above threshold, which FT8 does not need.

Mixing the two works but changes levels: a narrow signal sounds quieter on a wide receiver,
and a wide signal can exceed a narrow receiver's filter and distort. Both stations should
use the same setting.

Which setting decodes better at the margin is **to be measured** (test 7). Deviation
and audio levels must be tested per radio, not assumed.

## 8. Summary: FT8 on SSB and via FM

| | FT8 on SSB | FT8 via FM |
|---|---|---|
| Signal on the air | 50 Hz wide | fills the FM channel |
| Stations per slot on one frequency | many, side by side | one |
| Weak-signal performance | about -21 dB (2500 Hz reference) | none to speak of; limited by the FM threshold |
| Range | far beyond voice | about FM voice range (expected) |
| Radio needed | SSB transceiver | any FM transceiver |
| Software, messages, timing, logging | identical | identical |

FM-FT8 is an entry point into digital operating, not a weak-signal mode. For range and DX,
SSB remains the next step, and everything learned here carries over unchanged.

## References

- S. Franke (K9AN), B. Somerville (G4WJS), J. Taylor (K1JT): "The FT4 and FT8 Communication
  Protocols", QEX, July/August 2020.
- WSJT-X User Guide: <https://wsjt.sourceforge.io/wsjtx.html>
- IARU Region 1 VHF Handbook: <https://www.iaru-r1.org/>
