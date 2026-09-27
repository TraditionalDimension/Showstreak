# Showstreak 1.3.0 — player guide

A win-streak campaign for Balatro, by **TraditionalDimension**. Win fresh runs, prepare in a separate shop and face changing decks, stakes and conditions.

[Русская инструкция](PLAYER_GUIDE_RU.md) · [Download](https://github.com/TraditionalDimension/Showstreak/releases/latest) · [Changelog](../CHANGELOG.md) · [Known limitations](KNOWN_ISSUES.md) · [Credits](../CREDITS.md)

## Install and start

Install [Lovely](https://github.com/ethangreen-dev/lovely-injector) **0.9+** and [Steamodded](https://docs.smods.dev/Installation/Installing%20Steamodded%20windows/) **26.829.0+** separately. The tested Windows baseline uses Balatro **1.0.1o-FULL**. See [compatibility notes](KNOWN_ISSUES.md) for the scope of testing.

1. Close Balatro completely.
2. Download **Showstreak-BMM.zip** from [Releases](https://github.com/TraditionalDimension/Showstreak/releases/latest). The automatic GitHub source archives are not the playable mod; the separate Toolkit is for authors.
3. Extract the archive's `Showstreak` folder into Balatro's `Mods` folder. On Windows, check `%AppData%\Balatro\Mods\Showstreak\main.lua`, without a second nested Showstreak folder.
4. Restart Balatro and choose **To Show** on the Showstreak sign above Profile.

Drag the sign's title area to move the panel. Its position is remembered; **Config → Reset Menu Panel Position** restores the default. **New Series** opens preset selection; **Start Series** begins it and **Customize** opens an editable draft. **Continue** resumes a saved series. Returning to the main menu or closing the game is not a loss.

## Presets and starting rules

Easy, Standard and Hard each have an ordinary and a **Casting** variant.

| Preset family | Blind score factor | Starting winning Ante | Allowed winning Ante | Dark Stars per completed act, when eligible |
| --- | --- | --- | --- | --- |
| Easy | ×0.5 | 8 | 2–10 | 1 |
| Standard | ×1 | 8 | 4–16 | 2 |
| Hard | ×1.25 | 10 | 6–21 | 3 |

Blind score factors change required scores, not overall difficulty by a fixed percentage. Factory rules begin with 3 ordinary stars, use three wins per act and progress from the first stake toward the eighth. Existing series retain their saved rules, including earlier Ante bounds.

Customization separates **starting resources** from **bounds**. Starting values apply when a fresh run begins, before deck and stake effects. For example, a base of six hands gives Black Deck five; a base of −$5 gives Yellow Deck $5. Resuming a run does not grant the starting resources again.

Bounds limit Showstreak's own changes. They do not refill spent resources or force ordinary cards and unrelated mods to obey those limits. An effect that would cross a bound is unavailable. The required Ante is the actual victory target: starting at Ante 10 with bounds 8–10 still requires winning at Ante 10.

Rules and enabled decks are fixed once a series starts. Editing a copy or choosing a new default affects future series.

## Casting and optional starting additions

Casting limits which Jokers can appear. Before each run, choose three of seven offered Jokers to unlock for the series; fewer choices remain when the locked roster runs out. **Unlocking allows a Joker to appear; it does not give you its card.**

Joker, the four suit Jokers, Mr. Bones and Yorick are available from the start. Yorick keeps its normal legendary sources. The first Casting candidate is revealed by default; vouchers can reveal additional positions. Unlocks survive Intermission, while the reveal vouchers do not.

If installed and supported, **Card Sleeves, Partner and other starting categories** can be enabled before creating the series. Choose one of up to five options per enabled category, or continue without one. These additions are disabled in the six official presets. Enabling them makes a new series ineligible for Dark Stars. Keep the required integration mods installed to continue the series.

## Prepare the next run

The shop's next-run panel shows the **deck, stake, required Ante and readiness**. Complete any pending Mask, Casting or enabled starting choice. Then buy, use or keep items, inspect **Run Info**, and start when ready.

- **Ordinary stars** pay for Showstreak purchases.
- **Dollars** belong to the Balatro run.
- **Dark Stars** belong to the profile and are recorded separately.

The deck-change button costs **2 stars**, then 3, 4 and so on during that preparation. The Recast item and the ordinary Showstreak shop reroll are separate actions. Showstreak rerolls refresh regular item offers, not every shop slot.

| Action | Result |
| --- | --- |
| Buy or take a Prop/Trick | It enters held inventory. Use it to apply its effect. |
| Use a Prop | Prepare its effect for the next run. |
| Use a Trick | Perform its stated action. |
| Buy or take a voucher | Activate it immediately for the duration shown on the card. |
| Open a pack | Choose the displayed number of rewards. Consumables enter inventory; vouchers and Bets apply immediately. |
| Accept a Bet | Accept its terms for the next run. At most two can be active. |

**Buying is not using.** Advance Payment in your inventory does not grant its $9 until you use it. Held items carry between runs; next-run preparations apply to the run you are about to start. Read each card for its duration and restrictions. Discarding a held item gives no refund.

The **?** button opens the illustrated shop guide. **Run Info** lists preparations, sources, promised starting Jokers and boss-objective progress. Older saves may have incomplete source history. **Mods → Showstreak → Shop catalog** lets you browse registered items, prices and sources, including compatible additions from other mods; a listed item may still be unavailable under the current series' rules.

### Bets and packs

Bets are grouped into **Easy / Normal / Hard**, with additional offers on each column's own pages. External Bets without a difficulty appear under **Other**. The two-Bet acceptance limit applies regardless of the number offered.

Boss-objective Bets require both the task and a victory in the whole run. Only one boss schedule can be accepted, including **On a Needle**; a compatible ordinary Bet can use the other slot. Failing an objective loses its bonus, not the whole run. Inspect its progress in Run Info.

Version 1.3.0 adds voucher packs and mixed Variety packs. Taking a reward inside a purchased pack costs no additional stars. Each choice checks prerequisites, inventory space and Bet conflicts again; a blocked choice does not consume a pick. Vouchers and Bets take effect as soon as you select them.

Chance items are consumed whether they succeed or fail. Their result screen shows what actually happened. Reopening a result does not roll again. See the [changelog](../CHANGELOG.md) for the complete new-item list; use the in-game catalog to inspect current cards.

## Acts, Masks, Intermission and results

Wins advance the streak and complete acts. Masks add lasting conditions between acts; **Tonight** is a temporary condition for one run. A Mask's category is shown before acceptance. Inspecting it does not accept it; after acceptance, acknowledge its revealed effect before starting.

Conditions come from the series' saved catalog and must fit its rules. An active family is not offered again. Older series do not automatically gain every condition introduced by a later mod version.

An optional **Intermission** can be offered after completing at least three acts in the current segment. The offer is random. Accepting it:

- Preserves the total win streak, profile Dark Stars, settled records, Casting unlocks and series rules.
- Clears held items, vouchers, accumulated conditions and next-run preparations.
- Resets ordinary stars and local act/stake progression to the series' starting values.

You may decline and continue with the accumulated effects. Older series without an Intermission policy retain their original progression. Read the offered terms before accepting.

A loss ends the series. Starting again creates a new inventory and ordinary-star balance. Endless is not part of the streak flow.

## Dark Stars and Lore

Eligible official presets award **1 / 2 / 3 Dark Stars** for Easy / Standard / Hard when a victory completes an act, including their Casting variants. Custom rules or deck selection, enabled starting additions and foreign Showstreak gameplay content can make a new series ineligible. Check the eligibility line before starting.

Props, vouchers and conditions acquired during an eligible series do not by themselves remove its eligibility. Dark Stars persist through losses, new series and restarts, separately for each profile. Updating the mod does not retroactively award them to older progress.

**Lore is still in development.** The normal Lore screen preserves your balance but does not offer a player story catalog. The separate [Toolkit](TOOLKIT.md) includes the Lore Workshop for authors; it is not needed to play and does not unlock finished player Lore.

## Update and protect your saves

Close Balatro before replacing the mod. Back up these files together, keeping the profile directory they came from:

| Data | Location within Balatro's save directory |
| --- | --- |
| Campaign and profile progress | `<profile>/showstreak-a.jkr` and `<profile>/showstreak-b.jkr` |
| Current Showstreak run | `<profile>/showstreak-run.jkr` |
| Shared preferences | `config/Showstreak.jkr` |
| Personal presets | `config/Showstreak/presets` |

On Windows the save directory is `%AppData%\Balatro`; profile folders are numbered. Also preserve personal JSON files you placed in the old mod's `presets/import` folder. Keep backups outside `Mods`, then replace only `Mods/Showstreak`. Fully restart the game so Lovely applies the updated patches.

The alternating A/B campaign files and the current-run file serve different purposes. The campaign pair alone cannot reconstruct a missing current run. Ordinary Balatro's `save.jkr` remains separate.

### Updating a save and adding content are separate

If the save format requires an update, Showstreak explains the changes and waits for confirmation. It makes and verifies a backup before migrating. **Later** leaves the original files untouched, but a series needing migration cannot continue yet. A failed backup or changed source prevents the operation.

**Mods → Showstreak → Saves** contains manual backup verification and restore tools. Restore protects the current state with a safety backup first. Returning to an older mod version requires a matching older backup; reverse migration is not provided.

An existing series keeps its rules and catalog apart from explicitly previewed compatibility corrections. To include compatible new entries, use **Rules → Add new content** between runs. This is a separate previewed, backed-up choice. It does not restart the series or replace its existing rules. Future offers can change after the catalog expands.

Keep required content providers installed. Saves retain definitions, but do not bundle another mod's code or textures. A missing required provider can block continuation even if you have not purchased one of its items.

## Settings, languages and presets

The configuration includes preferences, rules, mod compatibility, the shop catalog and save tools. Dialogue, voice, effect sounds and reduced motion have separate settings shared across profiles. Showstreak also respects Balatro's reduced-motion setting.

Showman's voice is prepared in memory from your installed game's sounds. Original or processed game voice files are not included in the player archive; no audio download is needed. The native voice is used if preparation fails.

Use Balatro's own language selector. UI and dialogue cover **15 locales**: English, Russian, German, Spanish for Spain and Latin America, French, Indonesian, Italian, Japanese, Korean, Dutch, Polish, Brazilian Portuguese, Simplified Chinese and Traditional Chinese. See [known limitations](KNOWN_ISSUES.md) for translation and controller coverage.

The Rules page keeps built-in presets read-only. **Edit a Copy** creates a draft, **Save Copy** saves a named preset, and **Make Default** selects it for future series.

- **Open Folder** opens `config/Showstreak/presets`; **Export** writes to its `exports` subfolder.
- **Import Files** reads JSON from the mod's `presets/import` folder and copies validated presets to your personal folder. The source remains intact.
- Use **Refresh** after placing JSON directly in the personal folder.
- Examples are included in the installed mod's `presets/examples`.

Importing neither starts a series nor changes its default. Missing required mods or unavailable decks can prevent starting a preset. The mod's root `config.lua` contains factory values; your personal settings are in `config/Showstreak.jkr`.

## Compatibility, removal and support

The Mods report distinguishes a registered integration from a merely loaded mod. An integration badge does not guarantee every feature of another mod is compatible; custom decks need support from an adapter. Check the [compatibility notes](KNOWN_ISSUES.md) when combining mods.

To uninstall, close Balatro and remove only `Mods/Showstreak`. Progress and personal settings remain in the save directory. Continuing a saved series requires reinstalling Showstreak and its required providers.

Report reproducible problems through [GitHub Issues](https://github.com/TraditionalDimension/Showstreak/issues), with the mod/game/loader versions, operating system, other mods, steps to reproduce and relevant error text. Lovely logs are in `Mods/lovely/log`. Keep an unchanged copy of a failing save when reporting a save issue.

Use of Showstreak's files is governed by [LICENSE](../LICENSE). Keep the installed mod folder intact; attribution is listed in [CREDITS](../CREDITS.md).
