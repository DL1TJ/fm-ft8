# Test plan

The eight tests I plan to run, ordered from "no radio needed" to "on the air". Each stands
on its own. If you try one yourself, I would be glad to hear the outcome
(<dl1tj@darc.de>); what is worth noting down is listed at the [end](#what-to-note-down).
Results go into [RESULTS.md](RESULTS.md).

Ground rules:

- Report what you measured, with how you measured it. No estimates presented as results.
- A negative or inconclusive result is a result. Report it.
- Note every setting that could matter: radio model and firmware, narrow/wide, power,
  interface, WSJT-X version, tone frequency, slider position.
- Keep on-air tests short, at low power, on a clear frequency, and identify with your
  callsign as your licence requires. Tests into a dummy load are preferred where they do
  the job.

Recording decodes: WSJT-X writes every decode to `ALL.TXT` (File > Open log directory).
Attach the relevant lines. For audio, File > Save > Save all keeps the `.wav` of each slot.

---

## Test 1 - Audio loopback

**Question:** Does the computer side work, independent of any radio?

**Equipment:** Computer, WSJT-X, the interface (or just a cable from sound card output to input).

**Procedure:**
1. Connect the sound card output to its input (through the interface's attenuator if it has one).
2. Run a second WSJT-X instance, or a second computer, as the receiver; or record and replay.
3. Transmit a CQ at 1500 Hz. Repeat at 500, 1000, 2000 and 2500 Hz.

**Record:** Decoded yes/no per frequency, reported SNR and DT, slider position, input level.

**Expected:** Decodes at every frequency with DT near 0.

---

## Test 2 - Two-station RF link

**Question:** Does standard FT8 get from one FM radio to another at all?

**Equipment:** Two FM radios with interfaces, two computers. Dummy loads or attenuators for
a bench test; antennas for an on-air test.

**Procedure:**
1. Both radios: same frequency, narrow FM, squelch open, low power.
2. Station A calls CQ at 1500 Hz for ten slots. Station B receives.
3. Swap roles.
4. Complete a full contact.

**Record:** Decodes out of slots sent, in each direction; SNR and DT as reported by WSJT-X;
whether the full contact completed without manual help.

**Optional extension - capture:** A third transmitter in the same slot, with a step
attenuator. At what level difference does the receiver decode the stronger station? The weaker? Neither?

---

## Test 3 - PTT start-up delay

**Question:** How long from PTT until a clean modulated signal is on the air, for hardware PTT and for VOX?

**Equipment:** Radio under test, interface, a second receiver (FM radio or SDR) whose audio is recorded.

**Procedure:**
1. Record the second receiver's audio and, on the other stereo channel, the transmit audio
   sent to the radio (or WSJT-X's own audio output).
2. Send ten transmissions with hardware PTT. Measure the time between the start of the
   audio on the reference channel and the start of clean tone in the received audio.
3. Repeat with VOX.
4. Also note the DT that a receiving WSJT-X reports, compared with test 1.

**Record:** Delay per transmission (ms), minimum/median/maximum; radio model; PTT method;
any drop-outs during the 12.64 s.

---

## Test 4 - FM deviation

**Question:** What deviation does a given audio level produce, where does the limiter set
in, and how can an operator without test equipment set the level?

**Equipment:** Radio under test and interface; a deviation meter, or an SDR with software
that displays the spectrum of the FM signal.

**Procedure:**
1. Transmit a steady tone (WSJT-X "Tune") at 1500 Hz into a dummy load.
2. Raise the slider in steps. At each step record the deviation. With an SDR: read the
   deviation from the demodulated amplitude, or use the carrier-null method (the carrier
   disappears at modulation index 2.405, i.e. at 3.6 kHz deviation for a 1500 Hz tone, which the
   limiter should prevent in narrow FM, and at 2.4 kHz for a 1000 Hz tone).
3. Find the level where deviation stops rising (limiter).
4. Repeat at 1000 and 2000 Hz.
5. "No test equipment" method: listen on a second receiver while raising the drive; note
   the slider position where the received tone stops getting louder. Compare with step 3.

**Record:** Table of slider position / audio level versus deviation per tone frequency;
limiter onset; whether the listening method finds the same point.

---

## Test 5 - Audio frequency response

**Question:** How does the complete chain (transmit audio path, FM, receive audio path)
treat tones across the audio range?

**Equipment:** As test 2, bench set-up.

**Procedure:**
1. At a fixed drive level below the limiter, transmit FT8 at 300, 500, 1000, 1500, 2000,
   2500 and 3000 Hz.
2. On the receiving side record the audio level of the tone and the decode result.
3. Repeat with drive above the limiter and look at the waterfall for harmonics.

**Record:** Received level and SNR per frequency; decode yes/no; harmonics seen and at
which frequencies.

---

## Test 6 - Weak-signal decoding

**Question:** How weak can the FM signal be before decoding fails, and how does that
compare with voice intelligibility on the same link?

**Equipment:** As test 2 on the bench, with a calibrated step attenuator between the radios
(and adequate shielding - handhelds leak).

**Procedure:**
1. Start with a strong signal. Send ten slots; count decodes.
2. Add attenuation in 2 dB steps (1 dB near the edge). Ten slots per step.
3. Note at each step what the FM audio sounds like (clean / noisy / breaking up) and, if
   possible, whether speech would still be readable.
4. If the receiver is an SDR, record the carrier-to-noise ratio.

**Record:** Decode rate versus attenuation; the step at which decoding drops below 50 %;
the corresponding audio impression; RF level if known.

This test produces the number that replaces "roughly FM voice range" in the documentation.

---

## Test 7 - Narrow versus wide FM

**Question:** Which setting decodes better at the margin, and what happens when the two
sides differ?

**Equipment:** As test 6.

**Procedure:** Repeat test 6 for the four combinations: narrow to narrow, wide to wide,
narrow to wide, wide to narrow. Re-do the level setting (test 4) for each transmit setting.

**Record:** The 50 % decode point for each combination.

---

## Test 8 - Real-world range

**Question:** How far does it work with ordinary stations?

**Equipment:** Two stations with antennas, or one station and a monitoring receiver such as
the skimmer at <https://fmft8.db0rbx.de>.

**Procedure:**
1. Fixed station at one end. The other end moves or is at a known distance.
2. At each location: ten slots of FT8, then an FM voice test on the same radios, power and antennas.
3. Note terrain and line of sight.

**Record:** Locators of both ends, distance, antennas and heights, power, radios; decode
rate; voice readability at the same spot; SNR reported.

---

## What to note down

For any test or on-air attempt, so that someone else can repeat it:

- Callsign, grid locator, date and time (UTC).
- Radio make and model, narrow or wide FM, transmit power.
- Antenna type and height.
- Interface (self-built or which product), PTT method (hardware or VOX).
- WSJT-X version, transmit tone frequency, power slider position.
- Which test, bench or on the air, the other station, distance and path.
- Slots sent and slots decoded, SNR and DT as reported, whether a full contact completed.
- How FM voice fared on the same link.
- Measurements with the instrument and method used.
- The relevant `ALL.TXT` lines, and `.wav` files or screenshots if you have them.

Write "not measured" rather than guessing.
