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

Each entry names its SPRX relative to that same directory, so the current release
(`"id": "0.3.6.2-release"`, `"file": "libSceAmpr.sprx-0.3.6.2"`) is fetched from
`.../apr-emu/libSceAmpr.sprx-0.3.6.2` and checked against the size and SHA-256 the
manifest declares before anything is written.

To point somewhere else, write the base URL — the directory, not the
`manifest.json` — into `/data/apr-emu-updater/manifest-base-url` and relaunch.
