# Reference setup

> **Draft - not built, not validated.** This describes the reference setup I intend to
> build and test. No radio, cable or interface is confirmed to work until it appears in
> [testing/RESULTS.md](../testing/RESULTS.md). Do not treat anything here as a
> compatibility claim.

## Goal

One setup that a newcomer can copy part for part: inexpensive, built from things that can
be bought or soldered easily, and documented well enough that a second person gets the
same result.

## Block diagram

```
                       +----------------------------+
                       |        Interface           |
 +-----------+  spkr   |  +---------+               |  USB   +----------+
 |           |-------->|--| level   |--> ADC  ------|------->|          |
 | FM radio  |         |  +---------+   (USB sound  |        | Computer |
 | (handheld)|  mic    |  +---------+    card)      |        |  WSJT-X  |
 |           |<--------|--| atten-  |<-- DAC  ------|<-------|          |
 |           |         |  | uator   |               |        |          |
 |           |  PTT    |  +---------+               |        |          |
 |           |<--------|--[ switch ]<-- RTS/DTR ----|<-------|          |
 +-----------+         |              (USB serial)  |        +----------+
                       +----------------------------+
```

## Functions the interface must provide

### 1. USB sound card

Any USB audio device with a mono input and a mono output. WSJT-X uses 48 kHz, 16 bit.
A dedicated device for the radio keeps system sounds off the air.

### 2. Receive path: radio speaker output to sound card input

The speaker/earphone output of a handheld is far above microphone level. Feed it to a line
input, or reduce it with the radio's volume control and, if needed, a resistive divider.
Target: WSJT-X level meter around 30 dB on open-squelch noise, never in the red.

### 3. Transmit path: sound card output to radio microphone input

This is the critical part. A sound card output delivers up to about 1 V; a handheld's
microphone input expects a few millivolts. Without attenuation the radio's limiter clips
at the lowest slider settings.

- A resistive divider of roughly 100:1 as a starting point. **Value to be determined** for
  the reference radio (test 4).
- A trimmer potentiometer so the level can be set once and left alone.
- A series capacitor to block DC: many handhelds supply a bias voltage on the microphone
  line for an electret capsule.

### 4. Audio isolation

Radio and computer share a ground through the cable. With a handheld on battery and a
laptop this is often harmless; with mains-powered equipment and an external antenna it can
cause hum and RF feedback into the audio.

- Recommended for the reference: 1:1 audio isolation transformers (600 ohm) in both audio
  paths, and an optocoupler for PTT, so that no galvanic connection remains.
- Ferrite on the cable if RF gets into the audio.

Whether isolation is needed for a handheld on its own antenna is **to be determined**.

### 5. Hardware PTT

WSJT-X raises RTS or DTR on a serial port when it transmits. The interface turns that into
whatever the radio needs to key:

- A USB serial adapter (or a sound-card interface with a built-in serial port).
- A transistor or optocoupler that closes the radio's PTT contact.
- How the PTT contact is wired differs by radio family. Many handhelds key by connecting
  the microphone line to ground through a resistor rather than by a separate PTT pin. **The
  pinout for the reference radio will be documented here once it has been built and verified.**

VOX is the fallback, not the reference: see
[docs/TECHNICAL_BACKGROUND.md](../docs/TECHNICAL_BACKGROUND.md), section 6.

### 6. Transmit level adjustment

Procedure in [testing/TEST_PLAN.md](../testing/TEST_PLAN.md), test 4. In short: set the
level where the received audio of a second receiver stops getting louder as you raise the
drive, then back off.

## Ready-made interfaces

Several products and open designs combine sound card, serial PTT and radio cable in one
unit. They are candidates for the reference setup. **I have not tested any of them yet.**

## WSJT-X configuration

| Setting | Value |
|---|---|
| Settings > General | Callsign, grid locator |
| Settings > Radio > Rig | None |
| Settings > Radio > PTT method | RTS or DTR, port of the interface |
| Settings > Audio > Input / Output | The interface's sound device, mono |
| Settings > Frequencies | 144.575 MHz, FT8, region as applicable |
| Main window | Mode FT8, Tx 1000 to 2000 Hz |
| Operating system | Sound device level fixed; all enhancements and system sounds off for this device; time synchronisation on |

## Parts

Part types only. Models, values and prices follow once the setup is built and works.

| Item | Requirement |
|---|---|
| FM transceiver, 2 m | Analogue FM, narrow (12.5 kHz) setting, speaker/microphone connector, battery save can be switched off |
| USB sound card | Mono in, mono out, 48 kHz |
| USB serial adapter | RTS and DTR accessible |
| Audio transformers, 2x | 1:1, 600 ohm |
| Optocoupler or NPN transistor | Switches the radio's PTT |
| Trimmer potentiometer, resistors | Transmit attenuator and level; values to be determined |
| Capacitor | DC block in the microphone line; value to be determined |
| Radio plug or cable | Matches the radio's connector |

Alternatively a ready-made sound-card interface with hardware PTT and a cable for the radio.
The aim is that the interface costs clearly less than the radio.

## To be added after validation

- Wiring diagram with component values.
- Pinout of the reference radio's speaker/microphone connector.
- Photos of the built interface.
- Measured levels and the resulting deviation.
