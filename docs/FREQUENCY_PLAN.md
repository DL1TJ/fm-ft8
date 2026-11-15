# Frequency plan

## The proposal

| | |
|---|---|
| Frequency | **144.575 MHz** |
| Use | Proposed experimental calling frequency for FT8 via FM |
| Mode | Narrow FM, simplex, standard FT8 |

## What this is, and what it is not

- It is my personal **proposal**, put up for discussion.
- It is **not allocated** to FT8 or to FM-FT8 by anyone.
- It is **not coordinated** with the IARU, the DARC or any other band plan authority.
- It is **not exclusive**. Others may use the frequency, and they were there first.
- It **may change** with feedback, in particular if it turns out to conflict with
  established local use.

Before transmitting, check local activity, your licence conditions and national
regulations, and the current band plan for your region. This document describes IARU
Region 1; other regions have different band plans.

## Where 144.575 MHz sits

Source: IARU Region 1 VHF Handbook, version 10.03, 144-146 MHz band plan.

```
 144.000        144.400 144.500              144.794      144.975
    | CW, SSB, MGM  | beacons |      ALL MODE      | digital, APRS | FM repeaters ...
                              ^   ^
                        144.500   144.575  <- proposed
```

The all-mode segment, 144.500 to 144.794 MHz, maximum bandwidth 20 kHz:

| Frequency | Use listed in the band plan |
|---|---|
| 144.500 MHz | Image mode centre (SSTV, fax ...) |
| **144.575 MHz** | **nothing listed - proposed here** |
| 144.600 MHz | Data centre of activity (MGM, RTTY ...) |
| 144.750 MHz | ATV talk-back |
| 144.794 MHz | upper edge; digital communications and APRS (144.800 MHz) follow |

Why this frequency:

- The segment permits all modes with up to 20 kHz bandwidth, which covers FM.
- The band plan lists no specific use for 144.575 MHz.
- A 12 kHz wide narrow-FM channel centred on 144.575 MHz extends to 144.581 MHz and ends
  19 kHz below the data centre of activity on 144.600 MHz, which is left free for the
  narrow modes that meet there.

Constraints from the band plan:

- **No unmanned stations** in the all-mode segment (with exceptions that do not apply
  here). FM-FT8 operation must be attended. Receive-only monitoring is not affected.
- No repeater input or output channels between 144.000 and 144.794 MHz.

## Frequencies to stay away from

- **144.174 MHz** - FT8 on SSB. FM there would disturb the SSB users.
- **144.600 MHz** - data centre of activity.
- **144.800 MHz** - APRS.
- The beacon segment, 144.400 to 144.490 MHz.

## Open points

- Local use of 144.575 MHz differs by area, and I only know my own. If the frequency is busy where you live, I would like to hear about it.
- I have not asked the national VHF managers for their view yet.
- 70 cm: no frequency proposed yet.
- Regions 2 and 3: not looked at.

## Monitoring

A receive-only skimmer at DB0RBX in Regensburg (JN69ba) listens on 144.575 MHz FM around
the clock and publishes what it decodes, along with other signals it notices on the
frequency: <https://fmft8.db0rbx.de>
