# Monoprice 6-Zone Amplifier RS-232 Protocol

Transcribed from the MPR-6ZHMAUTO installation manual (model 10761).

## Connection

- **Interface:** DB-9 serial (Tx, Rx, GND)
- **Baud rate:** 9600, 8, N, 1 (default; configurable — see below)
- **Terminator:** CR = `0x0D` (carriage return)
- **Case:** insensitive

---

## Zone Codes (`xx`)

| Code | Meaning |
|------|---------|
| `10` | All zones — Master unit |
| `20` | All zones — Slave 1 |
| `30` | All zones — Slave 2 |
| `11`–`16` | Zones 1–6 of Master unit |
| `21`–`26` | Zones 1–6 of Slave 1 |
| `31`–`36` | Zones 1–6 of Slave 2 |

---

## Control Commands

Format: `<xx><PP><uu>\r`

| Code | Parameter | Values (`uu`) |
|------|-----------|---------------|
| `PA` | Public Announcement | `00` = off, `01` = on |
| `PR` | Power | `00` = off, `01` = on |
| `MU` | Mute | `00` = off, `01` = on |
| `DT` | Do Not Disturb | `00` = off, `01` = on |
| `VO` | Volume | `00`–`38` |
| `TR` | Treble | `00`–`14` |
| `BS` | Bass | `00`–`14` |
| `BL` | Balance | `00`–`20` |
| `CH` | Source channel | `01`–`06` |

**Examples:**

```
>1101\r       # Power on zone 11
>1100\r       # Power off zone 11
>1138\r       # Set zone 11 volume to max (38)
>1101\r       # Set zone 11 to source 1  ← wrong: use CH
>11CH03\r    # Set zone 11 to source 3
>10PR00\r    # Power off all zones on master unit
```

---

## Inquiry Commands

### Form 1 — Full zone status

Request: `?<xx>\r`

Reply: `><xx><aa><bb><cc><dd><ee><ff><gg><hh><ii><jj>\r`

| Field | Parameter | Notes |
|-------|-----------|-------|
| `xx` | Zone code | As above |
| `aa` | PA status | `00`/`01` |
| `bb` | Power status | `00`/`01`; bit 5 = backup zone power (zone-only) |
| `cc` | Mute status | `00`/`01` |
| `dd` | Do Not Disturb status | `00`/`01` |
| `ee` | Volume | `00`–`38` |
| `ff` | Treble | `00`–`14` |
| `gg` | Bass | `00`–`14` |
| `hh` | Balance | `00`–`20` |
| `ii` | Source channel | `01`–`06` |
| `jj` | Keypad connected | `00` = disconnected, `01` = connected |

When querying all zones (`10`/`20`/`30`), the controller replies with one line per zone.

### Form 2 — Single parameter

Request: `?<xx><PP>\r`

Reply: `><xx><PP><uu>\r`

`PP` can be any of the control codes above, plus `LS` for keypad connect status.

---

## Source Name Configuration

```
1<name>\r    # Set Source 1 display name (8 ASCII chars, padded with spaces)
2<name>\r    # Set Source 2 display name
...
6<name>\r    # Set Source 6 display name
```

`<name>` must be exactly 8 valid ASCII characters.

```
N<name>\r    # Set the unit name shown on keypad at power-on (4 ASCII chars)
```

---

## Baud Rate Configuration

Send the desired rate followed by CR. Rate resets to 9600 on AC power cycle.

```
9600\r
19200\r
38400\r
57600\r
115200\r
230400\r
```
