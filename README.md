# TI BASIC ESP32 — Shared Docs

Documentation shared between the two active TI BASIC simulator
projects:

- **[esp32-s3-box-basic-idf](https://github.com/JonVogel/esp32-s3-box-basic-idf)** — ESP-IDF port for the
  ESP32-S3-Box (V1 and V3) hardware
- **[ti-extended-basic-esp32](https://github.com/JonVogel/ti-extended-basic-esp32)** — ESP-IDF port for
  the Sunton 8048S043C (4.3" 800×480 RGB panel)

Both projects pull this repo in as a `docs/` submodule, so anything
here only has to be maintained in one place.

## Contents

| File | What |
|---|---|
| [KEYBOARD.md](KEYBOARD.md) | BLE keyboard / serial console / FCTN-key reference for the BASIC editor |
| [KEYWORDS.md](KEYWORDS.md) | Supported BASIC keywords and CALL subprograms |
| [EXTENSIONS.md](EXTENSIONS.md) | Non-TI extensions in this BASIC implementation (CALL WIFI, CALL PAIR, etc.) and known limitations |
| [PORT_NOTES.md](PORT_NOTES.md) | Hardware / IDF porting notes — pin maps, sdkconfig, gotchas |
| [GROM_NOTES.md](GROM_NOTES.md) | TI-99 GROM internals — relevant if you ever extend toward a true emulator |
| [transfer.md](transfer.md) | arduino-cli → ESP-IDF migration playbook used for both targets |
| [test_suite/](test_suite/) | 46 individual BASIC `.bas` test programs + `INDEX.txt` + the master `TEST_PROGRAMS.txt` spec with expected outputs |

## Updating

Edit a file here, commit + push this repo, then bump the submodule
pointer in whichever consumer project(s) you want to pick up the
change:

```bash
cd <consumer-project>/docs
git pull origin main
cd ..
git add docs && git commit -m "Bump docs: <one-line summary>"
git push
```
