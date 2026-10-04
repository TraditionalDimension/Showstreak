# Showstreak

**A win-streak campaign mode for Balatro, by TraditionalDimension.**

The Showman has a challenge for you: keep winning across fresh Balatro runs. Prepare between runs, face changing decks and rising stakes, and see how long your streak can last.

**[Download the player ZIP](https://github.com/TraditionalDimension/Showstreak/releases/latest/download/Showstreak-BMM.zip)** · [Latest release](https://github.com/TraditionalDimension/Showstreak/releases/latest) · [Nexus Mods](https://www.nexusmods.com/balatro/mods/947)

This repository contains documentation and release notes. Install **Showstreak-BMM.zip** from Releases. GitHub's automatic **Source code** archives contain this documentation repository, not the playable mod.

## What the mode adds

- A campaign across separate Balatro runs, with assigned decks, rising stakes and conditions between acts.
- A between-run shop with preparations, tricks, vouchers, Bets and packs. Hold consumables until you are ready to use them.
- Easy, Standard and Hard presets, each with a **Casting** variant: choose Jokers to unlock for future appearances throughout the series.
- Custom rules, starting resources and deck selection, with preset import and export.
- Optional Card Sleeves, Partner and supported starting additions when their integrations are installed and enabled.
- Campaign records, profile Dark Stars and saves separate from the ordinary Balatro run.
- An animated Showman, dialogue in 15 languages, separate voice/effect settings and reduced motion.
- A public integration API and a separate Toolkit for authors.

**Lore remains in development.** Dark Stars are retained, but the normal player Lore screen does not yet offer a story catalog. The Toolkit's Lore Workshop is an authoring tool.

## Requirements and installation

| Component | Requirement |
| --- | --- |
| Balatro | Tested with 1.0.1o-FULL on Windows |
| Steamodded / SMODS | 26.829.0 or newer |
| Lovely | 0.9.0 or newer |

Install [Steamodded](https://docs.smods.dev/Installation/Installing%20Steamodded%20windows/) and [Lovely](https://github.com/ethangreen-dev/lovely-injector) separately. The [compatibility notes](docs/KNOWN_ISSUES.md) describe the tested scope.

1. Close Balatro completely.
2. Download **Showstreak-BMM.zip** from [Releases](https://github.com/TraditionalDimension/Showstreak/releases/latest).
3. Extract its **Showstreak** folder into `%AppData%\Balatro\Mods`.
4. Check that `%AppData%\Balatro\Mods\Showstreak\main.lua` exists, without a second nested Showstreak folder.
5. Restart Balatro and choose **To Show** on the Showstreak sign above Profile.

When updating, back up your profile's `showstreak-a.jkr`, `showstreak-b.jkr` and `showstreak-run.jkr` together while the game is closed. Preserve shared settings and personal presets too. Replace only the installed Showstreak folder, keeping backups outside Mods. The guides explain the full paths and the difference between updating a save and adding new catalog content.

## Guides and support

- **Players:** [English guide](docs/PLAYER_GUIDE_EN.md) · [Русская инструкция](docs/PLAYER_GUIDE_RU.md).
- **Authors:** [Toolkit overview](docs/TOOLKIT.md) · [Download Toolkit 1.3.0](https://github.com/TraditionalDimension/Showstreak/releases/download/1.3.0/Showstreak-Toolkit-1.3.0.zip).
- **Integrations:** [English modding guide](docs/MODDING_GUIDE_EN.md) · [Русское руководство](docs/MODDING_GUIDE_RU.md) · [API reference](docs/API.md) · [Tutorial integration](docs/examples/integration/README.md).
- **Project:** [Changelog](CHANGELOG.md) · [Known limitations](docs/KNOWN_ISSUES.md) · [Artwork and sound layout](docs/ASSET_LAYOUT.md) · [Credits](CREDITS.md).

The Toolkit contains the author documentation, integration example and standalone Lore Workshop. It is not required to play. The public API remains **contract 1, revision 1.1.0**; the mod and Toolkit release number is 1.3.0.

Report reproducible problems through [GitHub Issues](https://github.com/TraditionalDimension/Showstreak/issues). Include Showstreak, Balatro, Lovely and Steamodded versions, other installed mods, steps to reproduce, and the error text or relevant log.

## Permissions

See [LICENSE](LICENSE) for the author's terms. Use in Balatro is permitted. General redistribution, re-uploading and inclusion in other distributed packages are not permitted without separate author permission. Independently written integrations may be distributed separately without bundling Showstreak's files.

The author has granted a [limited exception for Balatro Mod Manager](BMM_PERMISSION.md) to distribute and install unmodified official releases.

Balatro and third-party components remain the works of their respective creators. See [CREDITS.md](CREDITS.md).
