# APR Emu Updater

A standalone PS5 payload that does one thing: keep **APR Emu**
(`fakelib/libSceAmpr.sprx`) up to date on installed titles. It serves a small
web interface on the console and puts a tile on the **home screen** that opens
that interface in the console browser.

## Running it

Send `apr_emu_updater.elf` to the console's ELF loader on port 9021.

## Where the versions come from

The build list is `manifest.json` under the Pegasus DL repository:

```text
https://raw.githubusercontent.com/tsuramatsu1/pegasus-dl/main/apr-emu/manifest.json
```
