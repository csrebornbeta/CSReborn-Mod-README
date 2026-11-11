# CS:REBORN

**CS:REBORN** is an unofficial Counter-Strike 2 offline mod focused on a locally
simulated inventory, a native-looking Armory, bot matches, and a branded launcher.
It is made by **Ginekolog**.

> CS:REBORN is a fan-made project. It is not affiliated with, endorsed by, or
> sponsored by Valve. Counter-Strike and Counter-Strike 2 are Valve trademarks.
> This mod does not grant real Steam items, change a Steam inventory, charge a
> Steam Wallet, or provide access to official matchmaking.

This repository is intended to publish this README only. It contains no game
files, launcher executable, mod package, or releases.

## What the mod is

CS:REBORN adapts an offline Counter-Strike 2 project around the game's own
interface. Instead of replacing CS2 with a separate game client, it adds local
gameplay and inventory systems to a separate, supported Windows copy. The native
Armory, Store, inventory and loadout screens are used where the supported client
allows them to be adapted.

The project has two parts:

1. A native C++ core and supporting tools that provide the local mod behavior.
2. The CSReborn Windows launcher, which signs in to Steam for game-file
   downloads, installs a specific CS2 build, and carries the mod package inside
   the launcher executable.

The launcher downloads Counter-Strike 2 from Steam. It does not need to download
the mod package from GitHub after it has been built into the launcher.

## Features

### Native Armory and local inventory

- Browse an Armory-style catalogue through CS2's native interface, using the
  stock-style **4/3/4 tile layout**.
- Earn and spend local Armory stars, buy catalogue offers, and sell eligible
  local items for stars.
- Keep local inventory, equipment/loadouts, progression and collection history
  in the separate mod installation.
- Open supported cases, capsules and souvenir containers, and use supported
  trade-up paths.
- Use local knife and glove finishes and the shipped bot cosmetic selections.
- Preserve generated finish identifiers and collection history across updates
  so previously saved items remain associated with their existing definitions.

The catalogue and economy are local simulations. They do not communicate item
grants, purchases, sales, equips, openings or trade-ups to Valve's live
inventory service. Some container, trade-up and visual paths still need broader
testing in the target game client.

### Local matches and bots

- Play local CS2 matches with bots and supported Bot Improver integrations.
- Use rating-based stock bot difficulty and the bundled Low, Medium and High
  bot presets.
- Use curated bot names, weapon cosmetics, knives, gloves and sticker crafts.
- Play the implemented match flow, including ready-up, a knife round, side
  selection, timeouts and supported 5v5 Wingman behavior.
- Choose Overpass or the Cobblestone Wingman Workshop map in local play. The
  Cobblestone item must be subscribed/installed through Steam Workshop.
- Use the MAX PERFORMANCE preset for reduced world detail, uncapped in-game
  FPS, and a 60 FPS menu cap. It is enabled by default on a fresh setup.
- Remove the old clutch-statistics HUD and its repeated statistics polling.
  Stutter causality has not been verified in a live client.
- Earn local match rewards: the implemented match flow awards 7 stars for a
  win and 3 for a loss.

Bot behavior, match-flow plugins and cosmetic selection depend on the exact
installed CS2 build and third-party plugin compatibility. These features are
intended for local play, not official matchmaking.

### Progression and interface changes

- Keep local level, XP, rating, match statistics and inventory data.
- Use supported care-package and reward flows connected to local progression.
- Use the adapted main-menu play flow, settings page, low-end option and classic
  buy-menu option where supported.
- Use supported weapon and knife drop behavior in local matches.

Missions are disabled in this beta, and planned or reference-only features are
not represented as working features.

### CSReborn launcher

- Sign in to Steam with mobile-app QR approval or account-name/password flow
  with Steam Guard when requested.
- Download the supported CS2 depots from Steam and merge them into one install
  folder.
- Recognize an existing `cs2.exe` to avoid downloading the game again.
- Install the mod package embedded inside the launcher, without a separate
  online mod download.
- Keep progress isolated by Steam account and switch profiles when signing in
  to another account.
- Back up local progress before using **RESET PROGRESS**.
- Verify game files, choose an install folder, set optional launch options,
  create a desktop shortcut, and open launcher logs.
- Use the supplied CS:REBORN icon and orange/pink launcher palette.

The launcher is not a Steam client replacement. Steam must be installed, and a
Steam account with access to Counter-Strike 2 is required to download the
supported depots. The launcher does not read the Steam desktop client's stored
session. Steam credentials are used for Steam authentication; the launcher
does not intentionally save the account password.

There is no online mod updater or launcher self-updater. A newer mod package
requires a newly built launcher. The project's build workflow does not publish
release files or upload the launcher or mod package as an artifact.

## Supported game build

The launcher targets Windows x64 and the following Counter-Strike 2 build:

| Steam app | Depot | Manifest |
|---|---:|---:|
| `730` | `2347771` | `8766757721818354077` |
| `730` | `2347770` | `2520908610273941189` |

These manifests identify the tested package target, not a promise of ongoing
compatibility. CS2 updates can change native interfaces, game resources, plugin
behavior or file layout. Do not mix files from another build into the supported
installation. Keep an unmodified Steam installation for official play.

## Local data and safety

- Mod inventory, progression, settings, generated identifiers, replays and logs
  are local files in the separate game copy.
- The launcher keeps account-specific progress under `CS2 GC Saves\Accounts`.
- Resetting progress moves the existing profile into a dated backup; it does not
  delete the backup.
- The launcher installs mod-owned files only. It does not alter the user's
  existing Steam game files or publish them.
- Do not share replay recordings, account tokens, personal logs or profile
  backups publicly.
- Do not use the modded installation for official matchmaking. Use a clean,
  unmodified game installation for normal Steam play.

## Testing and limitations

Automated native and Python tests validate offline logic, packaging rules,
manifests, launcher authentication flows and safe extraction. Passing those
tests does **not** prove that every feature works in a live CS2 client.

Live-client checks remain necessary for UI placement, menus, container paths,
bot behavior, match flow and visual finish quality. Runtime
integration is specific to the identified CS2 build. No client offsets,
protocol identifiers or UI behavior should be assumed to work on a newer
build without verification.

The following are not claims of this beta:

- Official matchmaking, real Steam inventory items, trading or wallet purchases.
- Validated public internet hosting or guaranteed peer-to-peer multiplayer.
- Compatibility with every CS2 release or every hardware configuration.
- Mission support or an unrestricted custom-case system.
- Fully verified in-game layout for every native UI modification.

## Building from source

The core uses C++20 and CMake. The launcher is a .NET Framework 4.8 WinForms
application built with the .NET SDK; Python 3.9 or later is used by the build
and support tools. On Windows, use an x64 Visual Studio developer environment.

```powershell
cmake -S . -B build -A x64 -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTING=ON
cmake --build build --config Release
ctest --test-dir build -C Release --output-on-failure
python tools/package_release.py
```

The packaging command validates the mod archive, embeds it as a resource in
`dist/CSReborn.exe`, and runs the launcher's offline self-test. The executable
is a local build output; the workflow is configured not to publish it or the
embedded mod package.

## Creator and acknowledgements

**Made by [Ginekolog](https://github.com/csrebornbeta?tab=repositories).**

CSReborn is Ginekolog's project. It builds on prior CSOffline work by
zenswxrld and includes third-party tools and libraries; those upstream
contributions and required licenses are acknowledged here and in the bundled
license notices.

- DepotDownloader by SteamRE is used to download Steam depots under its
  applicable license.
- The mod bundles and credits Metamod:Source, CounterStrikeSharp, Bot Improver,
  and their respective upstream components and licenses.
- Counter-Strike 2 game files are downloaded from Steam; Valve game files are
  not included in this README-only repository.
