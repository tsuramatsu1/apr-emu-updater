<p align="center">
  <img src="./assets/banner.png" alt="APR Emu Updater" width="100%">
</p>

# APR Emu Updater v2.0.3

Some games on a jailbroken ps5 rely on a file called **APR Emu**
(`libSceAmpr.sprx`). Different games run best with different versions of it, and
swapping that file by hand means editing game images.

This payload does it for you. It opens a small web page on your ps5 where every
installed game is listed with the APR Emu version it is currently using, and you
pick the version each game should use.

> [!IMPORTANT]
> Despite its name, **APR Emu Updater does not update APR Emu itself**. It
> chooses which `libSceAmpr.sprx` a game reads, so the game runs the version you
> picked.

## Prerequisites

- A jailbroken ps5 with an ELF loader listening on port 9021.
- **NanoDNS** running, the first time — after that the payload keeps its own
  offline copy of the APR Emu builds and no longer needs the internet.
- **ShadowMountPlus** for games installed as image files. Both branches work:
  - **1.7alpha7 or newer** — the payload uses ShadowMountPlus's own emulator
    slot. Nothing is mounted over your games.
  - **1.6beta16 and older** — the payload mounts the override itself, as it
    always has.

## Upgrading from an earlier version

> [!WARNING]
> **Delete everything inside `/data/apr-emu-updater/` before using this
> release.** State from earlier versions is laid out differently and will
> conflict. This clears your staged versions and the offline copy; both are
> rebuilt from scratch.

For Shadowmount Alpha, update `update_emulators` to `1` in
`/data/shadowmount/config.ini`.  ShadowMountPlus will ignore the
override. You can edit that file from the payload's own **ShadowMountPlus
config** tab.

## How to use it

**Send `apr_emu_updater.elf` to your ps5**, port 9021, with whatever you normally
use to send payloads. The ps5 shows:

```text
APR Emu Updater listening on http://192.168.1.20:6971/
```

The first time it also installs an **APR Emu Updater** tile, so you can reach the
page later without typing an address. Open the page from the tile, or type that
address into a phone or computer on the same network.

### Setting a version for a game

ShadowMountPlus 1.7 mounts a game's image only while the game is running, so the
payload cannot see what APR Emu a game carries until it has run once.

1. **Launch the game once, then close it.** The payload reads and remembers the
   APR Emu it carries while the image is mounted.
2. **Open the page.** The game now shows its version and offers actions. Games
   that have never been launched are listed but offer nothing yet.
3. **Choose a version** — **Update** for the newest, or **Select version** for a
   specific one, including debug builds. The column shows the version you picked.
4. **Launch the game again.** The override is applied as the game starts, and a
   notification confirms which build is live.

Repeat step 1 for each game you want to manage.

### One version for every game

If you would rather not choose per game, switch **Override mode** to **Global**.
The game list is replaced by a single version picker, and ShadowMountPlus applies
that one build to every game that uses APR Emu.

## Good to know

- **Per game mode needs the payload loaded.** The override is set up as each game
  starts, so send the payload after every reboot, or let an autoloader do it. You
  do not need to open the tile — only to have the payload running.

- **Global mode does not.** That version is written to ShadowMountPlus's own
  folder and stays there, so it survives reboots with no payload loaded.

- **Games installed as folders are changed on disk** and stay changed either way.

- **It works offline.** The APR Emu builds are copied to your console the first
  time the payload runs with a connection. After that, versions and installs work
  with no internet at all. Use **Fetch latest builds** on the Offline builds tab
  when you want new ones.

- **Nothing is used unless it is verified.** Every build is checked against the
  size and SHA-256 the version list declares — when it is downloaded, and again
  every time it is read from the offline copy.

- **The page follows your language.** Spanish, Japanese, French, German and
  Arabic are translated, with English for everything else. Corrections from
  native speakers are welcome.

## If something looks wrong

| What you see | What it means |
| --- | --- |
| The tile opens an empty browser page | The payload is not loaded. Send it again, then tap the tile |
| A game is listed with no buttons | It has not been launched yet. Start it once, close it, and it will fill in |
| "did not take the APR Emu override" | The game started before the override was ready. Close it and start it again |
| A game says APR Emu is missing | That game has no `libSceAmpr.sprx` to replace, so there is nothing to change |
| A game is listed as unsupported | Its image is a raw `.ffpfs`, which cannot be overridden |
| No versions listed, first run | The ps5 could not reach the internet. Check NanoDNS, then use **Fetch latest builds** |
| Anything else | Press **Logs** on the page. It shows what the payload did, step by step |

## Where the versions come from

The APR Emu builds are in this repository, under `apr-emu/`, and the payload
reads the list from:

```text
https://raw.githubusercontent.com/tsuramatsu1/apr-emu-updater/main/apr-emu/builds.json
```

They are copied to `/data/apr-emu-updater/apr-emu/` on your console and read from
there afterwards. Nothing else is contacted, and nothing is downloaded from
anywhere else.
