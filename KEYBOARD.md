# Keyboard reference

Two input paths reach the BASIC editor:

* **BLE keyboard** (any HID keyboard paired via `CALL PAIR` / `F12` /
  the `BOOT` button). The ALT key on a modern PC keyboard plays the
  role of TI's **FCTN** modifier — most BLE keyboards reserve the
  hardware `Fn` key for profile switching, so it never reaches the
  HID layer.
* **USB serial console**. Anything typed in a terminal connected to
  the device's USB-UART (115200 baud) is interpreted exactly like the
  BLE keyboard — including the special control codes below.

---

## Editor controls

Most of these are the original TI-99/4A FCTN combos, plus PC-friendly
function-key aliases for convenience.

| Action | TI key | ALT-combo (BLE kbd) | Function key | Serial (Ctrl+) | TI char code |
|---|---|---|---|---|---|
| **DEL** — delete char under cursor | FCTN+1 | **ALT+1** | **F1** / Delete | Ctrl+G | 7 |
| **INS** — toggle insert mode | FCTN+2 | **ALT+2** | **F2** | Ctrl+D | 4 |
| **ERASE** — clear the current line | FCTN+3 | **ALT+3** | **F3** | Ctrl+B | 2 |
| **CLEAR / BREAK** — interrupt RUN | FCTN+4 | **ALT+4** | **F4** | Ctrl+L | 12 |
| **BEGIN** — return to top | FCTN+5 | **ALT+5** | **F5** | Ctrl+E | 5 |
| **AID** | FCTN+6 | **ALT+6** | **F6** | Ctrl+A | 1 |
| **PROCEED** | FCTN+7 | **ALT+7** | **F7** | Ctrl+F | 6 |
| **REDO** — reload last entered line | FCTN+8 | **ALT+8** | **F8** | Ctrl+N | 14 |
| **BACK** | FCTN+9 | **ALT+9** | **F9** | Ctrl+O | 15 |
| **ENTER** — submit line | ENTER | ENTER | ENTER | CR (Enter) | 13 |
| **BACKSPACE** — erase char before cursor | FCTN+S then DEL, or BACKSPACE | BACKSPACE | BACKSPACE | Ctrl+H / 0x7F | — |
| **PAIRING** mode (~30 s scan) | n/a | n/a | **F12** | — | — |

`F11` is reserved (TI's FCTN+= QUIT is not implemented; would warm-
reset the interpreter).

---

## Cursor / navigation

| Action | TI key | ALT-combo | Modern arrow | TI char code |
|---|---|---|---|---|
| **Left** | FCTN+S | **ALT+S** | ← | 8 |
| **Right** | FCTN+D | **ALT+D** | → | 9 |
| **Up** (or recall prev. line in EDIT mode) | FCTN+E | **ALT+E** | ↑ | 11 |
| **Down** (or recall next line) | FCTN+X | **ALT+X** | ↓ | 10 |

Long lines soft-wrap to the next screen row. Arrow keys + DEL +
BACKSPACE all follow the wrap automatically.

---

## ASCII / shifted characters

The standard PC layout is honored. SHIFT works as expected for symbol
overlays:

| Unshifted | SHIFT |
|---|---|
| `1 2 3 4 5 6 7 8 9 0` | `! @ # $ % ^ & * ( )` |
| `- =` | `_ +` |
| `[ ]` | `{ }` |
| `\` | `\|` |
| `; '` | `: "` |
| `` ` `` | `~` |
| `, . /` | `< > ?` |

---

## Hardware buttons

(Boards vary; not every button exists on every target.)

| Button | Action |
|---|---|
| **BOOT** | Enter BLE pairing mode (~30 s scan window). Same as `F12` or `CALL PAIR`. |
| **MUTE** | Toggles the audio codec power gate (on boards that have one). |
| **RST** | Hardware reset — equivalent to a power cycle. |

---

## BASIC-level commands related to BLE / WiFi

| Command | Effect |
|---|---|
| `CALL PAIR` | Enter BLE pairing mode for ~30 s. Same effect as `F12` / BOOT. |
| `CALL UNPAIR` | Forget all bonded BLE peers. |
| `CALL WIFI` | Print one-line status (`ONLINE <ip>` or `OFFLINE`). |
| `CALL WIFI("ssid","pass")` | Store WiFi credentials in NVS and connect. Persists across reboots. |
| `CALL WIFI("ssid","pass","name")` | Same plus a friendly hostname (shows in `/api/status` + UDP discovery). |
| `CALL WIFI("name","my-host")` | Set just the hostname; credentials unchanged. |
| `CALL WIFI("forget")` | Clear stored credentials (and hostname). |
| `CALL WIFI("on")` / `CALL WIFI("off")` | Toggle the radio without losing creds. |
| `BLE` *(serial-only diagnostic)* | Print the bonded-peer table on the serial console. |

See `EXTENSIONS.md` for the full CALL WIFI semantics + what the
hostname is used for.

---

## Tips

* **Pairing a new keyboard**: at the BASIC prompt, hit `F12` (or press
  BOOT, or type `CALL PAIR`). Put your keyboard into its own pairing
  mode at the same time. The "BleHidHost: pairing complete" line on
  serial confirms it bonded. Bonds persist across reboots — you only
  pair once per keyboard.

* **Insert mode visual indicator**: there isn't one yet. Type a
  character into the middle of a line — if the tail shifts right
  instead of being overwritten, insert mode is on.

* **Resetting the line in mid-edit**: `F3` (ERASE) clears whatever
  you've typed and returns the cursor to just after the `>` prompt.
