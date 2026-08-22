<p align="center">
  <img src="./assets/banner.png" alt="APR Emu Updater" width="100%">
</p>

# APR Emu Updater

A standalone PS5 payload that keeps **APR Emu**
(`fakelib/libSceAmpr.sprx`) up to date on installed titles. It serves a small
web interface on the console and adds a tile to the **home screen** that opens
that interface in the console browser.

> [!IMPORTANT]
> **APR Emu Updater must always be loaded.** Add it to your payload autoloader,
> or launch it manually before starting a game, so the APR Emu override remains
> mounted.
>
> Despite its name, **APR Emu Updater does not update APR Emu itself**. Instead,
> it overrides the `libSceAmpr.sprx` that games use at runtime, allowing them to
> use the selected version while the payload is loaded.

## Running it

Send `apr_emu_updater.elf` to the console's ELF loader on port 9021.

## Where the versions come from

The builds ship in this repository, under `apr-emu/`, and the payload reads the
list from:

```text
https://raw.githubusercontent.com/tsuramatsu1/apr-emu-updater/main/apr-emu/builds.json
```

Builds are grouped under the version they belong to, newest version first, with
each version's release build ahead of its debug build:

```json
{
  "builds": "apr-emu",
  "buildsVersion": 2,
  "latest": { "release": "0.3.6.2", "debug": "0.3.6.2" },
  "versions": [
    {
      "version": "0.3.6.2",
      "builds": [
        {
          "build": "release",
          "file": "libSceAmpr.sprx-0.3.6.2",
          "bytes": 225094,
          "sha256": "aa574cbe7624f611d5858f8a62771726d5613aa58787095f36c98f852406f4e1"
        }
      ]
    }
  ]
}
```

`file` is relative to the same directory as `builds.json`, and the download is
checked against `bytes` and `sha256` before anything is written. The `builds`
name and `buildsVersion` are both required, so a differently shaped document is
refused rather than half-read.

To point somewhere else, write the base URL — the directory, not the
`builds.json` — into `/data/apr-emu-updater/builds-base-url` and relaunch.
