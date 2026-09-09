# Showstreak 1.0.0

By **TraditionalDimension**. **First public release: 1.0.0.** Available on [Nexus Mods](https://www.nexusmods.com/balatro/mods/947). Automated checks and an isolated Windows game acceptance pass have completed. Remaining manual playtesting and support limits are listed in [known issues and validation status](KNOWN_ISSUES.md).

Keep a winning streak across fresh Balatro runs. Showman chooses a deck before each run; stars buy preparations between runs, and masks add lasting rules between acts.

[Русская инструкция](PLAYER_GUIDE_RU.md) · [Changes](../CHANGELOG.md) · [Credits](../CREDITS.md)

Use of Showstreak's files is governed by [LICENSE](../LICENSE).

## Install and start

Install [Lovely](https://github.com/ethangreen-dev/lovely-injector) and [Steamodded](https://docs.smods.dev/Installation/Installing%20Steamodded%20windows/) first. They are separate dependencies and are not included in the Showstreak archive.

With Balatro fully closed, extract the archive's `Showstreak` folder into Balatro's `Mods` folder. On Windows, the resulting file should be `%AppData%\Balatro\Mods\Showstreak\main.lua`. Avoid a second nested `Showstreak` folder. Start the game again and check that Showstreak appears in the Mods list.

Requires **Lovely 0.9+** and **Steamodded 26.829.0+**. The tested development baseline is **Windows, Balatro 1.0.1o-FULL, Lovely 0.9.0, Steamodded 26.829.0**. These minimum requirements do not certify every newer loader, operating system or combination of mods; see [validation status](KNOWN_ISSUES.md).

Open **To Show** on the Showstreak panel above Profile in the main menu. Drag its title area to move the whole panel; its position is saved across restarts and profiles, relative to the available screen space. **Config → Reset Menu Panel Position** restores the default position.

**New Series** shows the selected preset; **Start Series** starts it, while **Customize** opens an editable copy. **Continue** resumes an existing series. Returning to the menu or closing the game is not a loss.

## Update, remove and report a problem

Fully close Balatro before replacing the mod. Back up the campaign files and personal settings listed under **Campaign saves and compatibility** below. If you kept imported JSON inside `Showstreak/presets/import`, copy those files out too. Replace only the `Mods/Showstreak` folder with the new folder from the archive; keep the backup outside `Mods` so a second copy cannot load. Do not replace the entire Balatro save directory or your other mods.

To remove Showstreak, close Balatro and remove only `Mods/Showstreak`. Your campaign files, Dark Stars, settings and user presets stay in the save directory. Continuing a Showstreak series requires reinstalling the mod and its required content providers. Do not remove personal save files unless you intend to discard that progress.

Report problems in the discussion where you received the build or, after publication, alongside its download. Include the Showstreak, Balatro, Lovely and Steamodded versions; your operating system and other mods; reproduction steps; the error text or screenshot; and the relevant Lovely log from `Mods/lovely/log`. A campaign backup can help diagnose a save problem, but is not needed for every report.

## Presets and starting rules

| Preset | Blind score factor | Winning Ante | Dark Stars per completed act |
| --- | --- | --- | --- |
| Standard | ×1 | 8 | 2 |
| Easy | ×0.5 | 8 | 1 |
| Hard | ×1.25 | 10 | 3 |

These factors scale required blind scores. They are **not percentages of overall difficulty**: deck interactions, compounding conditions and Hard's longer run also matter.

Factory rules start with 3 stars, award 3 stars for a win, and use 3 wins per act. Stakes progress from 1 toward 8. The first run is introductory; the optional temporary rule starts with the second run.

The editor separates **starting values** from **bounds**. Factory starting values are:

| Parameter | Base value |
| --- | --- |
| Winning Ante | 8 |
| Money | $4 |
| Hands / discards | 4 / 3 |
| Hand size | 8 |
| Deck size | 52 |
| Joker / consumable slots | 5 / 2 |
| Shop item / booster slots | 2 / 2 |

Resource bases apply before deck and stake effects, only when a fresh run starts. For example, a base of 6 hands gives Black Deck 5; base money -$5 gives Yellow Deck $5. A deck-size base of 60 gives Abandoned Deck 48 cards, retaining its -12-card identity. Resizing uses the deck's generated cards, preserving such restrictions as Checkered Deck's suits. Resuming a run never reapplies its starting resources.

Winning Ante is explicit: target 10 with bounds 8–10 still means win at Ante 10. Negative starting money is supported; its configured bounds must permit it.

Bounds restrict **Showstreak's own changes**. They do not grant resources, refill spent hands or discards, or force normal cards and other mods to obey those limits. An effect that would cross a bound is unavailable rather than partially clipped. Deck effects can put the actual baseline outside a configured bound; this does not erase the deck's identity.

The editor has Basics, Bounds, Start and Decks pages. Blue **−** and red **+** controls change values without wrapping. Reset restores the draft defaults; Undo Reset is available until another manual change. Rules and the enabled deck list are fixed for a started series.

## Shop and held items

Gold Stars belong to the series shop and are separate from run dollars and the profile's Dark Stars. Hovering, focusing or selecting a card is free; confirm the displayed action to spend or use it.

| Action | Result |
| --- | --- |
| Buy / Take a prop or trick | Add it to your held inventory. |
| Use a prop | Prepare its effect for the next run. |
| Use a trick | Perform its stated immediate action. |
| Buy a voucher | Activate its effect until an accepted Intermission or the series ends. |
| Open a pack | Choose items to add to your held inventory. |
| Accept a bet | Prepare a next-run condition and its victory reward. |

**Buying is not using.** Advance Payment held in the inventory does not grant its $9. Use it before starting: a normal $4 baseline then becomes $13, subject to the series rules. Held items carry between runs; prepared props are consumed for the next run. A +1-hand prop applies each round of that run.

Reroll refreshes the two regular item offers, not vouchers or packs; its price rises in the current shop. Named deck tricks change the next deck while keeping its run seed. Their shared selection weight prevents fifteen deck choices from overwhelming the item pool.

The blue **Ready for next run** button opens preparations and active modifiers; **Run Info** also shows these details. **?** opens shop help. Discard asks for confirmation and gives no refund. Tooltips spell proportional increases as exact fractions: **one fifth more** or **seven twentieths more**. Values and gameplay balance are unchanged; these increases are not chance rolls.

## Acts, masks and results

Between acts, a mask initially reveals only its effect category. Inspecting or revealing a mask does not accept it. After acceptance, its exact effect must be acknowledged before the next run; this reveal is saved and returns after a reload. Accepted conditions last until an accepted Intermission or the series ends, unless you remove one with a suitable trick.

New series have **12 condition families**. A family already active in the series is not offered again. If no further condition fits the rules and resource bounds, no new mask is added. Older series keep their saved catalog, including the earlier six-family catalog where applicable.

An optional **Intermission** may be offered after a victory completes at least three acts in the current segment of a new series. You can decline and keep going with your accumulated effects. Accepting it:

- Keeps your total win streak, profile Dark Stars, records of settled wins and the series' fixed rules and content catalog.
- Clears held items, active vouchers, accumulated conditions and next-run preparations.
- Returns Gold Stars to the configured starting amount and restarts local Act and Stake progression from the series' starting values.

The offer is random, not guaranteed at the third act. A later segment must again complete three acts before another offer can appear. Old campaigns created without an Intermission policy retain their original progression; updating the mod does not add this policy to them.

A loss ends the series. Records are local, and a new series starts with fresh Gold Stars and inventory. Endless is not part of the streak flow.

## Dark Stars and Lore

The main-menu panel shows the current profile's Dark Star balance. New series using the official **Easy / Standard / Hard** presets earn **1 / 2 / 3** Dark Stars when a victory completes an act. Eligibility requires the original preset rules, starting values and full available deck selection at series creation; changing them makes the series ineligible. Props, vouchers and masks earned during an eligible series keep its eligibility.

Dark Stars persist through losses, new series and restarts, separately for each profile. An act reward is saved together with its victory; reopening results or reloading cannot claim it again. Existing saves start with a balance of zero, with no retrospective awards. Series already active before v0.6.0 can continue, but do not earn Dark Stars; start a new official series to earn them.

**To Lore** currently opens a development notice and your balance. Collect Dark Stars for future Lore content; this version has no purchases or unlockable story entries.

## Settings, languages and controller

Settings are available from the mode menu and **Mods → Showstreak**. Dialogue, voice, effect sounds, reduced motion and the default preset are shared across profiles. Showstreak also respects Balatro's reduced-motion setting. Showman's voice has its own setting; Jimbo's global voice is not replaced.

Showman's voice is prepared in memory from the voice sounds in your installed Balatro. No original or processed game voice files are included in the archive, and no audio download is needed. If preparation fails, the matching native voice is used.

Use Balatro's own language button in the lower-right main menu, including its confirmation screen. UI and Showman dialogue are included in **15 languages**: English, Russian, German, Spanish (Spain and Latin America), French, Indonesian, Italian, Japanese, Korean, Dutch, Polish, Portuguese (Brazil), Simplified Chinese and Traditional Chinese. Supported regional variants use their base language; otherwise the game's supported language or English is used. Translation coverage does not imply native-speaker review of every line. Do not edit `settings.jkr` to switch languages.

Navigation and actions have controller focus support. In value editors, up/down selects a row, left/right changes it, and holding accelerates repeated changes. Back first cancels the current selection or target, then returns to the previous screen. Physical-controller and final audiovisual acceptance remain pending for version 1.0.0.

## Save and share presets

The Rules settings page lists read-only built-in presets and user copies. **Edit a Copy** opens a draft; **Save Copy** creates a named preset. **Make Default** selects it for future series. These actions do not change an active series.

- User presets: `config/Showstreak/presets` inside Balatro's save directory.
- **Open Folder** opens that folder; **Export** writes to its `exports` subfolder.
- **Import Files** reads JSON from the mod's `presets/import` folder, validates it and copies it into the user folder. Source files remain intact; colliding IDs become copies.
- JSON placed directly in the user folder is discovered with **Refresh**.
- Examples are in `presets/examples`. Presets use schema 1 and rules version 2.

Importing does not start a series or select a default. A preset with missing required mods, unsupported or locked decks cannot be started. Requirements accept an exact version or `*`.

Shared mod preferences are saved by Steamodded in `config/Showstreak.jkr`. Older profile preferences are migrated once to that shared configuration. The `config.lua` inside the mod is the factory configuration, not your personal settings; it belongs in an installation archive. Your `config/Showstreak.jkr` does not.

## Campaign saves and compatibility

Each profile uses `showstreak-a.jkr` and `showstreak-b.jkr` for checksummed alternating campaign saves, plus `showstreak-run.jkr` for the active run. Balatro's ordinary `save.jkr` remains separate.

Back up **all three Showstreak files together while the game is closed**, retaining their profile folder. The A/B pair stores campaign progress and Dark Stars; the run file stores the current Balatro run. Also keep `config/Showstreak.jkr` and `config/Showstreak/presets` if you want to preserve personal settings and presets. The single run snapshot does not have the campaign's alternating-slot protection, so a campaign backup alone cannot restore every interrupted or damaged run.

The newest valid campaign slot is selected before migration. A migration writes and verifies an alternate slot; older versionless rules retain their existing meaning. Unknown newer save schemas and two invalid campaign slots stop loading with a diagnostic instead of replacing the campaign.

New series freeze gameplay definitions, ordering and rules. Cosmetic metadata does not automatically invalidate their gameplay signature. Saves record required content providers: a missing provider blocks continuation with its ID. This includes providers represented in the catalog, not just purchased items. External code and textures are not embedded in the save.

The Mods report distinguishes registered integrations from merely loaded mods. An integration badge is not a guarantee that every unrelated hook is compatible. Modded decks need explicit adapters; ordinary supported, unlocked decks remain available. Compatibility with Pantomime Paradox or another mod should be established with the actual versions used; simply having both installed does not certify a complete series. See [known issues](KNOWN_ISSUES.md) for the current validation scope.

## Installed files and credits

Keep the installed `Showstreak` folder intact. Its `scripts`, `assets`, `localization`, `translations` and `presets` folders contain the code and data used by the mod. The root `main.lua`, `config.lua`, `Showstreak.json` and `lovely.toml` are required installation files.

Artwork supplied by **TraditionalDimension** includes the gold and dark stars, logo, lamps and mod icon. See [credits and resource sources](../CREDITS.md).

## Presentation and the Showman

Currency balances, rewards and prices share white plates. Win Streak and Act use large numbers in native-style grey fields, with act progress and explanatory hover/controller tooltips. An act-boundary victory identifies the completed act; preparation shows the next act.

The Showman responds to confirmed item purchases and use, vouchers, packs, bets, masks, visited sections and series milestones. First explanations are remembered per profile, recent lines do not repeat, and hidden mask details stay hidden. Purchases in inventory and applied effects are separate events.

All Showman portraits use the eight existing frames through `scripts/showman/animation.lua`. Frame 0 is the resting pose, with a target of 65% of ordinary visible time. Short intervals can vary. Random idle gestures use the blink sequence `0 → 1 → 2 → 1 → 0` or a temporary look in frame 3 or 4. Event reactions use frames 5, 6 or 7 and return to idle after the reaction, even if the speech text remains visible. Every exit from frame 7 passes through frame 6.

Animation timing uses an independent cosmetic random generator, preserving gameplay RNG. Reduced motion keeps a quiet frame-0 idle and brief event reactions, with recurring blinks and looks disabled. Voice settings remain independent of the visual reactions.

The About page explains the series loop; the TraditionalDimension author link opens the developer website only when activated. Rules distinguishes the previewed preset, its default/built-in status and starting parameters. Preview does not change an active series. Mods and compatibility separates actual mods from the game and loader versions.
