# Showstreak

**A win-streak campaign mode for Balatro, by TraditionalDimension.**

The Showman has a challenge for you: keep winning across fresh Balatro runs. Prepare between runs, face new decks and rising stakes, and see how long your streak can last.

**[Download the player archive](https://github.com/TraditionalDimension/Showstreak/releases/latest/download/Showstreak-Players.zip)** · [Nexus Mods](https://www.nexusmods.com/balatro/mods/947) · [All releases](https://github.com/TraditionalDimension/Showstreak/releases)

This repository hosts the download page, documentation and release notes. Install the attached **Showstreak-Players.zip** from Releases. GitHub's automatically generated source archives contain this documentation repository and are not the playable mod.

## What the mode adds

- A campaign that continues across separate Balatro runs, with assigned decks and increasing stakes.
- A between-run shop with preparations, vouchers, contracts and packs. Buy an item, then use it when you are ready to prepare the next run.
- Masks from 12 condition families that change the rules between acts, plus an optional Intermission.
- Easy, Standard and Hard presets, custom starting resources and bounds, and preset import/export.
- Campaign records and profile Dark Stars, with campaign saves kept separate from your ordinary Balatro run.
- Animated Showman portraits and contextual dialogue, 15 languages, separate voice/effect settings and reduced motion.
- Public API v1 for authors who want to add compatible items, deck adapters or Showman reactions.

Dark Stars are recorded in this version, but Lore purchases and story unlocks are not yet available.

## Requirements

| Component | Requirement |
|---|---|
| Balatro | Tested with 1.0.1o-FULL on Windows |
| Steamodded / SMODS | 26.829.0 or newer |
| Lovely | 0.9.0 or newer |

Install [Steamodded](https://docs.smods.dev/Installation/Installing%20Steamodded%20windows/) and [Lovely](https://github.com/ethangreen-dev/lovely-injector) separately. Talisman is not required. The tested setup and remaining coverage limits are listed in [known issues](docs/KNOWN_ISSUES.md).

## Installation

1. Close Balatro completely.
2. Download **Showstreak-Players.zip** from [Releases](https://github.com/TraditionalDimension/Showstreak/releases/latest) or obtain the mod on [Nexus Mods](https://www.nexusmods.com/balatro/mods/947).
3. Extract its **Showstreak** folder into `%AppData%\Balatro\Mods`.
4. Check that the path is `%AppData%\Balatro\Mods\Showstreak\main.lua`, without a second nested Showstreak folder.
5. Restart Balatro. In the main menu, use **To Show** on the Showstreak sign above the profile button.

For an update, close the game and back up the three Showstreak profile files together: `showstreak-a.jkr`, `showstreak-b.jkr` and `showstreak-run.jkr`. Keep your shared settings and personal presets too. Replace only the installed Showstreak folder; keep backups outside Mods so they are not loaded as a second copy. Detailed paths and instructions are in the player guides.

## Guides and support

- Player guides: [English](docs/PLAYER_GUIDE_EN.md) · [Русский](docs/PLAYER_GUIDE_RU.md).
- Modding guides: [English](docs/MODDING_GUIDE_EN.md) · [Русский](docs/MODDING_GUIDE_RU.md).
- [API reference](docs/API.md) and [local tutorial integration](docs/examples/integration/README.md).
- [Changelog](CHANGELOG.md), [known issues](docs/KNOWN_ISSUES.md) and [credits](CREDITS.md).
- Report a problem through [GitHub Issues](https://github.com/TraditionalDimension/Showstreak/issues) or the [Nexus Mods page](https://www.nexusmods.com/balatro/mods/947). Include your game, loader and mod versions, other installed mods, steps to reproduce, and the error text or relevant log.

The English modding guide is also available as a separate PDF release asset. Modding guides, the API reference and tutorial integration files are kept out of the player ZIP; player instructions remain included.

## Permissions

See [LICENSE](LICENSE) for the author's terms. Use in Balatro is permitted. General redistribution, re-uploading and inclusion in other distributed packages are not permitted without separate author permission. Independently written integrations may be distributed separately without bundling Showstreak's files.

The author has granted a [limited exception for Balatro Mod Manager](BMM_PERMISSION.md) to distribute and install unmodified official releases. This also covers the official 1.0.0 archive whose bundled license predates that exception.

Balatro and third-party components remain the works of their respective creators. See [CREDITS.md](CREDITS.md).
