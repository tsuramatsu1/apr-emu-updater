<p align="center">
  <img src="./assets/banner.png" alt="APR Emu Updater" width="100%">
</p>

# APR Emu Updater

Some games on a jailbroken ps5 rely on a file called **APR Emu**
(`libSceAmpr.sprx`). Different games run best with different versions of it, and
swapping that file by hand means editing game images.

This payload does it for you. It opens a small web page on your ps5 where every
installed game is listed with the APR Emu version it is currently using, and you
pick the version each game should use.

> [!IMPORTANT]
> **APR Emu Updater must always be loaded.** Add it to your payload autoloader,
> or launch it manually before starting a game, so the APR Emu override remains
> mounted.
>
> Despite its name, **APR Emu Updater does not update APR Emu itself**. Instead,
> it overrides the `libSceAmpr.sprx` that games use at runtime, allowing them to
> use the selected version while the payload is loaded.

## Prerequisites

- A jailbroken ps5 with an ELF loader listening on port 9021.
- **NanoDNS** must be running.
- **ShadowMountPlus** for games installed as image files.
- **APR Emu Updater** must remain loaded at all times. Add it to your payload
  autoloader, or send it manually before starting a game. No need to open the app
  tile, the updated apr-emu will be remounted when the payload is active.

## How to use it

1. **Send `apr_emu_updater.elf` to your ps5**, port 9021, with whatever you
   normally use to send payloads. Do this every time the console has been turned
   off, or let your autoloader do it for you.

2. **Watch for the notification.** The ps5 shows:

   ```text
   APR Emu Updater listening on http://192.168.1.20:6971/
   ```

   The first time, it also says `Installing APR Emu tile` and then
   `APR Emu tile installed` — that adds an **APR Emu Updater** tile so you can
   reach the page later without typing an address.

3. **Open the page.** Either tap the tile on your ps5, or type that address into
   a phone, tablet or computer on the same network. A phone is easier to type on
   and works exactly the same.

4. **Pick a game.** Each row shows the APR Emu version that game has now, and
   whether a newer one is available. Press **Update** for the newest version, or
   **Reinstall** to choose a specific one, including debug builds.

5. **Wait for it to finish**, then start the game. Leave the payload loaded.

## Good to know

- **Keep the payload loaded.** For games installed as image files the new APR Emu
  is layered on top of the game while the payload is active. Turn the ps5 off and
  the layer is gone, so send the payload again before playing. Games installed as
  folders are changed on disk and stay changed.

- **You do not have to open the tile.** It is only a shortcut to the page. What
  matters is that the payload is loaded.

- **The version list needs internet.** Versions are fetched once and remembered
  for ten minutes. If the ps5 cannot reach the internet, your games are still
  listed but their available versions show as unknown.

- **Nothing is replaced unless it is verified.** Every download is checked
  against the size and the SHA-256 the version list declares before it is written,
  and re-checked afterwards.

## If something looks wrong

| What you see | What it means |
| --- | --- |
| The tile opens an empty browser page | The payload is not loaded. Send it again, then tap the tile |
| "Load failed" or no versions listed | The ps5 could not reach the internet. Check that NanoDNS is running |
| A game says APR Emu is missing | That game has no `libSceAmpr.sprx` to replace, so there is nothing to update |
| A game is listed as unsupported | Its image is a raw `.ffpfs`, which cannot be overridden |
| Anything else | Press **Logs** on the page. It shows what the payload did, step by step |

## Where the versions come from

The APR Emu builds are in this repository, under `apr-emu/`, and the payload
reads the list from:

```text
https://raw.githubusercontent.com/tsuramatsu1/apr-emu-updater/main/apr-emu/builds.json
```

Nothing else is contacted, and nothing is downloaded from anywhere else.
