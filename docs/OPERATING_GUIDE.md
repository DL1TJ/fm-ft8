# Operating guide

From nothing to a first FT8 contact via FM. The same guide is on the
website in [English](https://dl1tj.de/fmft8.html) and [German](https://dl1tj.de/de/fmft8.html).

This guide has not been walked through on the air yet. If a step does not work for you,
I would like to hear about it: <dl1tj@darc.de>.

## Before you transmit

- Check local activity on the frequency, your licence conditions and national rules, and
  the current IARU Region 1 band plan.
- Listen first. 144.575 MHz is a shared frequency, not a reserved or exclusive one. It may
  be in use or unsuitable where you live.
- One at a time. Do not transmit in a time slot another station is using, and let a running
  contact finish. Two FM signals at once block each other.
- Attended operation only.
- Stay off 144.174 MHz, where FT8 is run on SSB.

## 1. What you need

- An FM radio for 2 m. A handheld is enough.
- A computer with [WSJT-X](https://wsjt.sourceforge.io/wsjtx.html) (free; Windows, macOS, Linux).
- An audio interface with PTT between the two. See
  [hardware/REFERENCE_SETUP.md](../hardware/REFERENCE_SETUP.md).

```
 FM radio  <-->  sound card + PTT  <-->  computer  <-->  WSJT-X
```

## 2. Set up the radio

- 144.575 MHz, FM, simplex (no repeater shift). Narrow FM (12.5 kHz) if the radio offers it.
- Squelch fully open. A closed squelch hides weak stations from the decoder and cuts off
  the start of transmissions.
- Battery save, CTCSS/DCS, roger beep, compander and noise reduction: off.
- Low power. FT8 transmits for about 13 seconds at a time, every other slot, and handhelds
  get warm at full power.
- A vertical antenna, as on any FM simplex channel.

## 3. Set the computer clock

FT8 works in fixed 15-second slots. Switch on automatic internet time in the operating
system and check that the clock is right to well within a second.

## 4. Set up WSJT-X

- **Settings > General:** your callsign and grid locator.
- **Settings > Radio:** Rig "None". PTT method RTS or DTR on the interface's serial port,
  so that WSJT-X keys the radio itself. VOX only as a fallback if the interface has no PTT
  line: it starts late and can drop out.
- **Settings > Audio:** the interface's sound device for input and output.
- **Settings > Frequencies:** add 144.575 MHz for FT8 and select it in the main window, so
  the log shows the right frequency.
- **Main window:** mode FT8; transmit tone between 1000 and 2000 Hz.

## 5. Check that you receive

With the squelch open you should see noise across the waterfall. Adjust the radio's volume
until the WSJT-X level meter reads roughly 30 dB. If a station is on, its messages appear
in the Band Activity list at the end of each slot.

## 6. Set your transmit level

Start with the WSJT-X power slider low. Raise it only until another station, or a second
receiver of your own, decodes you reliably. Too much drive makes the radio clip the tone,
which distorts your signal. A method for setting the level properly is part of the
[test plan](../testing/TEST_PLAN.md) (test 4).

Within FM range of Regensburg, the skimmer at <https://fmft8.db0rbx.de> shows whether you
are decoded.

## 7. Make a contact

- **Answer a CQ:** double-click it in the Band Activity list. WSJT-X sends your call and
  locator, then the reports and 73, on its own.
- **Call CQ:** select the CQ message, then Enable Tx. When someone answers, the exchange
  runs automatically. Call in a free slot.
- **Log it:** confirm the log window when the contact is complete. It is an FT8 contact on
  2 m like any other.

## 8. Tell me how it went

Whether it worked or not, a short note to <dl1tj@darc.de> helps. The
[test plan](../testing/TEST_PLAN.md) lists what is worth writing down.

## Troubleshooting

| Symptom | Things to check |
|---|---|
| Waterfall is empty | Squelch open? Right sound device? Volume? |
| Signals visible but no decodes | Computer clock; receive level too high (meter in the red) or too low; battery save still on |
| Others do not decode you | Transmit level (too high is as bad as too low); PTT actually keyed for the whole slot; same narrow/wide setting on both sides |
| DT values far from zero | Clock on one side; VOX or slow PTT |
| Radio gets hot | Reduce power; pause between transmissions |
