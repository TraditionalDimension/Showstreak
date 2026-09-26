# Changelog

## 1.3.0 — 2026-09-26

### Shop and presentation

- Added a **Shop catalog** tab to the mod configuration. Browse registered Showstreak shop items, including compatible additions from other mods, by type and inspect their effects, prices and source. Catalog inclusion does not guarantee availability in the current series.
- Reworked item descriptions around the effect a player receives. Duration, limits and secondary information use quieter text; technical stacking details no longer dominate the main description. Shop tooltips remain attached to their cards instead of stealing the hover when their windows overlap them.
- The next-run panel now separates the deck, stake, required Ante and readiness. The deck name and a separate deck-change button share one line. Changing the deck costs **2 stars, then 3, 4, and so on** during that preparation; Recast and the ordinary shop reroll remain separate actions.
- Bets use **Easy / Normal / Hard** columns. A new-series catalog offers three Bets, one per category when available; Grand Bill adds two more choices. Additional offers have their own column pages. The acceptance limit remains **two**. Existing frozen offers are preserved, and external Bets without a declared difficulty appear under **Other**. Empty columns, long names and page changes now keep a stable layout.
- Removed the duplicate Run conditions route. **Run Info** shows prepared effects, their sources, promised starting Jokers and boss-objective progress. Older saves without a complete source history are identified instead of inventing provenance.
- Replaced the shop help list with a one-page illustrated guide and educational tooltips. Item result screens show the actual outcome, consumption and duration, then return to the originating shop tab. Failed chance rolls no longer promise a bonus that was not awarded.
- Moved manual save-backup management into its own configuration tab. The normal menu prioritizes playing: Results uses the attention color and Main Menu is red. The automatic update notice remains, with a shorter explanation and optional technical details. Dark Star rewards have their own line.
- Rebuilt Showman's speech around full voice sounds and the original game's spaced speaking cadence. Delayed frames no longer compress the remaining sounds into a burst, menu speech does not speed up with game speed, and the positive speaking bounce is gentler. The sign lights reverse direction smoothly at varied intervals; reduced motion keeps them still.

### New preparation items

All prices below are in ordinary stars, not Dark Stars. These are new-series entries; older series use the separate, explicit catalog update.

| Item | Price | Effect |
| --- | ---: | --- |
| Reservation | 1 | Keep one unsold Prop or Trick through the next Showstreak shop reroll. |
| Backlight | 3 | Remove the next run's temporary condition. |
| Prop Master | 5 | Add one option to supported Showstreak packs, up to six, without increasing the number taken. |
| Short Script / Edited Cut | 8 each | Each independently lowers required Ante by 1. |
| Tight Rehearsal / Last Take | 5 each | Each lowers required Ante by 1 and discards by 1. |
| Grand Bill | 5 | +1 required Ante, +1 held-item slot and two additional offered Bets. |
| Lucky Curtain | 1 | A 1-in-8 chance to lower next-run required Ante by 1, down to the higher of Ante 6 and the series minimum. At or below that threshold, a successful roll awards 1–13 stars instead. |
| Double Applause | 4 | Gain stars equal to the balance when used, up to 10. |
| Box Office Return | 5 | Gain one star per owned Showstreak voucher in the current segment, up to 15. |
| Reprise | 3 | Create the last eligible consumed item, or a random eligible item if it is unavailable. Reprise and item-generating items are excluded. |
| Blue Skittles | 8 | Start the next run with Blueprint, lose one Joker slot and set that run to Blue Stake. |
| Red Brain | 7 | Start the next run with Brainstorm, lose one discard and one hand size. |

The five new Ante vouchers and Prop Master last until an accepted Intermission or the end of the series. Blue Skittles and Red Brain can each be used once per next run; grants respect Casting, bans, starting additions and capacity, and cannot be granted twice on resume. Blue Stake is set rather than permanently locked. Reprise remembers consumed chance items even when their roll failed, uses its own vacated slot when the inventory is full, and clears its history at Intermission. Star-return items cannot be wasted for a zero gain.

### New conditions and Bets

- **Commission:** Joker sales pay half their value, rounded down, or nothing. Recalculating a sale price does not repeatedly apply the penalty.
- **Short Showcase:** no Booster purchases in the first visited Balatro shop of each Ante. Reloading that shop preserves the restriction; later visits are counted separately.
- **Long Prologue, Late Bow and Third Bell** each add 1 required Ante; **Double Finale** adds 2. Built-in Easy and Easy Casting presets now use Ante bounds **2–10**, Standard variants **4–16**, and Hard variants **6–21**. Existing series retain their saved rules.
- **Full Programme:** no Blind skips for the next run; win for +2 stars. **No Substitutions:** no Balatro shop rerolls for the next run; win for +2 stars. Showstreak shop rerolls remain available.
- Added six next-run Bets: **Long Ovation** (+1 Ante, +3 stars on victory), **Long Monologue** (+1 Ante, +1 hand), **Second Rehearsal** (+1 Ante, +2 discards), **Wide Stage** (+2 Ante, +4 hand size), **High Stakes** (+2 Ante, victory stars equal to the starting stake level), and **On a Needle** (ordinary bosses become The Needle; victory gives three free purchases in the next Showstreak shop). Final boss selection is preserved by On a Needle; its free purchases exclude Bets, rerolls and internal pack selections.
- Added five objective Bets. Their bonus is earned only if the task is completed **and the whole run is won**, once per run:

| Bet | Assigned boss | Objective | Bonus |
| --- | --- | --- | ---: |
| Loose Ends | The Hook, one Ante before the target | No manual discards against this boss | +5 stars |
| Breakthrough | The Wall, one Ante before the target | Win the boss in at most two hands | +5 stars |
| Dry Run | The Water, one Ante before the target | No manual discards throughout that Ante | +5 stars |
| Three at a Time | The Serpent, one Ante before the target | Play at most three cards per hand against this boss | +5 stars |
| Fixed Blocking | Amber Acorn, at the target Ante | Do not manually reorder Jokers against this boss | +7 stars |

Only one boss schedule may be accepted at a time, including On a Needle. Compatible ordinary Bets can use the other slot. Failing an objective loses its bonus, not the entire run. Chicot and Luchador remain usable, but do not waive the objective. A fixed boss cannot consume a paid reroll or a Boss Tag; the tag waits for an eligible unfixed choice. Declared bans and conflicting external schedules are respected.

### New packs

| Pack | Price | Choice |
| --- | ---: | --- |
| Patron Pack | 4 | 1 of 2 Showstreak vouchers |
| Sponsor Pack | 5 | 1 of 3 Showstreak vouchers |
| Producer Pack | 7 | 1 of 5 Showstreak vouchers |
| Variety Pack | 3 | 1 of 4 mixed offers |
| Grand Variety Pack | 6 | 2 of 5 mixed offers |

Mixed offers include consumables, vouchers and Bets. Consumables enter the inventory; vouchers activate immediately; Bets are accepted immediately. Taking a card costs no additional stars. Each choice rechecks prerequisites, capacity and Bet conflicts; a blocked card does not consume a choice. Variety packs guarantee a paid-item option, and Grand Variety guarantees a compatible pair, rather than selling a selection consisting only of free Bets. Packs contain no duplicate IDs, nested packs or conditions. Mod-registered eligible items participate through the series' saved catalog.

### Saves, compatibility and author tools

- Added a previewable, backed-up update to **save schema 7**. Rules, frozen offers, progression and gameplay RNG are retained; missing older preparation history is not guessed. Compatible new catalog entries are still a separate opt-in between runs. Returning to an older mod version requires a matching backup; there is no reverse migration.
- Existing saved definitions of the five objective Bets are raised to the current +5/+7-star minimum rewards, and Fixed Blocking uses Rough Gem artwork. Objective progress, available offers and already settled payouts are preserved; there is no retroactive reward payment. A matching current-run snapshot adopts the updated definitions on resume without replaying grants.
- Added backup verification and restore controls, including a safety backup before restore, protection against stale source files, and recovery handling for interrupted operations. Ordinary loading does not create a new backup on every launch.
- Added action-policy declarations for integrations, pack-pool capability 2 (`vouchers` and `mixed`), optional Bet difficulty metadata, explicit safe-copy metadata for Reprise, and declared starting-Joker occupancy for supported starting additions. The public API contract remains **1**, revision **1.1.0**; the Toolkit's 1.3.0 appendix documents the additive capabilities and compatibility limits.
- Expanded the standalone **Lore Workshop** with independent RU/EN interface and story-text controls, Balatro colors, overlapping panel/image layers, reordering and reparenting, multi-selection, compound speech/caption objects, document/text Undo and Redo, persistent checkpoints, recovery drafts, resource replacement and shared preview/text validation. Exports remain story schema 2. Five runtime modules are shared with the mod.
- **Player Lore remains In development.** Reading imported stories in-game still requires the explicit development flag; the authoring tools do not unlock a released story catalog. LÖVE 11.5 is a separate editor dependency.
- New descriptions and interface messages cover all **15 supported game locales**. Focused Lua and isolated Windows game checks cover transactions, saves, hover, localized layouts, boss objectives and editor workflows. These checks do not replace ordinary campaign balance playtesting, native-speaker review or macOS/Linux/Steam Deck acceptance.



## 1.2.0 — 2026-09-19

### Fixed

- **The Needle softlock:** accepting Short Programme (Burglar) or Short Rehearsal could leave The Needle with zero playable hands and no way to finish the round. Showstreak now applies permanent hand and discard changes before the boss applies its own rules. This addresses [issue #1](https://github.com/TraditionalDimension/Showstreak/issues/1). Thank you, [ARK-MAXIM](https://github.com/ARK-MAXIM), for reporting the problem and both affected bets.
- **Other boss resource interactions:** The Water removes and restores the correct discard allocation. Temporary hand bonuses, Burglar and boss-disable effects retain their intended behavior. Focused resource regression checks cover all 28 standard bosses.
- **Existing stuck runs:** a narrowly identified older Needle save, before its first action, can recover one playable hand and the correct boss undo amount. This does not refill ordinary rounds after their hands have been spent.
- **Run results:** restored Balatro's victory/defeat cues and the music slowdown on defeat. Reopening the same settled result does not replay its cue; leaving results restores normal soundtrack speed.
- **Item illustrations:** Showstreak items no longer use Legendary Joker or The Soul artwork, including saved item definitions and aliases pointing at the reserved atlas positions. Matador is the safe fallback. Actual Jokers available through Casting keep their normal artwork and sources.

### New shop content

Added **17 items across seven families**. All prices below use Gold Stars. These entries are available in new series; existing series can add them explicitly as described under save compatibility.

| Family | Prices, levels 1 / 2 / 3 | Effect |
| --- | --- | --- |
| Quiet Finale — prop | 1 / 2 / 3 | Bosses require 1/10, 1/8 or 1/4 fewer chips, for the next run. |
| Finale License — voucher | 5 / 7 / 10 | The same boss reductions until an accepted Intermission or the end of the series. |
| Opening Contract — voucher | 5 / 7 / 9 | Small Blinds require 1/10, 1/4 or 1/2 more chips and pay an extra $2 / $4 / $6 when defeated and cashed out. |
| Lucky Seat — consumable | 1 / 1 / 1 | A 1/8, 1/6 or 1/4 chance to gain one Joker slot for the next run. |
| Lucky Glove — consumable | 1 / 1 / 1 | A 1/5, 1/4 or 1/3 chance to gain one hand size for the next run. |
| Fresh Voucher — trick | 1 | Replace the unsold voucher without changing other offers or the regular reroll price. |
| New Audition — trick | 1 | Replace one Casting candidate before confirming the selection. Hidden candidates stay hidden. |

- Boss reductions **multiply**. The discounted result is rounded down to a whole chip, but cannot fall below **1/25** of the requirement before these reductions; that minimum is rounded up. Props and vouchers can combine.
- Opening Contract requires the preceding level; the highest purchased level **replaces** the previous effect. Its extra dollars work at all stakes, including a Small Blind with no ordinary reward. Skipping or losing earns no bonus, and reloading does not duplicate payment.
- Lucky consumables are spent on success or failure. **Every successful use adds another +1**, including repeated uses of the same item or mixed levels. Joker slots and hand size accumulate independently, only for the next run. Failure keeps bonuses already earned.
- Lucky results are saved together with consumption before the result animation. Reopening cannot roll again. Items that would cross the series' resource bounds cannot be used.
- Items are selected by **family weight, then eligible level**, rather than by a new rarity system. Relative to an ordinary item at weight 1, boss-reduction families have weight 1, Opening Contract and Lucky Seat have weight 1/2, and Lucky Glove has weight 3/4. Level weights are **65:25:10**, equivalent to 13/20, 1/4 and 1/10 when all three levels qualify. Selection weights and success chances are separate.

### New conditions

- **Interest Holiday:** disables end-of-round interest.
- **Reroll Surcharge:** adds $1 or $2 to paid rerolls in Balatro's run shop. Free rerolls remain free.
- New series now have **18 condition families: 14 ordinary families and four Casting families**. The schedule for choosing permanent conditions at act boundaries is unchanged.

### Shop and Showman

- Hover the next run's **Stake** cell to read its description. Click it to see all inherited stake effects, including supported branching modded chains. The preview uses the upcoming run's stake.
- Hover **Win Ante** to see the next target and the minimum/maximum allowed by the saved series rules, or its fixed value.
- Added the native stake icon, darker Stake/Win Ante fields, a larger Gold Star balance on a white plate, and a clearer next-run panel. Act progress explains when the next permanent condition is chosen.
- Pack price labels are **15% smaller and centered**. This is a visual change; pack costs are unchanged.
- The upcoming deck has a native shadow, respecting the game's shadow setting. Its sprite uses Steamodded's creation path for supported custom and animated backs.
- Showman's speech cadence, pauses and mouth timing follow the selected game speed from the next line, without changing vocal pitch or truncating samples.
- Improved voice variety and phrasing: recent fragments are remembered, punctuation affects pauses, ordinary reactions leave room after the preceding line, and voice/mouth timing recover together after slow frames. Voice randomness does not advance gameplay randomness. Audio is prepared from the installed game; game audio files are not distributed.

### Saves and updating

- Introduced save **schema 5** with an explicit update preview. When an older format or required compatibility data needs updating, choose **Back up and update** or **Later — return to menu**. An already compatible save does not need migration on every patch.
- Before updating, Showstreak copies and verifies the existing campaign A/B files and linked run file in the profile's `showstreak-backups` folder. It checks for pending writes and source changes, then verifies the new alternate save slot.
- The preview preserves progress, Stars, rules, the current run and the saved content catalog. Older versions may not read the updated save; keep the backup for a possible return to an older version. There is no automatic reverse migration.
- **Rules → Add new content** is a separate optional step between runs. It previews and backs up the addition of missing compatible item/condition IDs. Existing definitions, prices, held items, preparations, current offers, rules, run seed and reward eligibility stay intact; future random offers can change.
- Installing 1.2.0 does not silently expand an older series' catalog or retroactively grant Dark Stars.
- **Fully restart Balatro after updating** so Lovely can apply the changed patches.

### Lore and Toolkit

- The player Lore screen now uses the shop palette and clearly says **In development**, while preserving the Dark Star balance, existing chapter unlocks and reading progress.
- The separate **Showstreak Toolkit 1.2.0** includes API reference material, English/Russian guides, an integration example and the standalone **Lore Workshop** editor with source code and a portable `.love` build.
- Workshop supports acts, chapters and pages; overlapping panels/layers; manual or automatic steps; timed sounds; and configurable fade, slide, wipe, scale and shake transitions. Preview uses the same schema/player/renderer code as Showstreak. Missing resources have safe fallbacks and diagnostics.
- Development story loading supports active-mod discovery, ordering hints, dependencies and deterministic conflict handling. **In-game story reading remains behind `Showstreak.lore_development = true` in 1.2.0**; these tools do not constitute a released story catalog. Workshop UI is currently English; story text can be multilingual.
- API contract **1**, revision **1.1.0**, remains unchanged. Toolkit's 1.2.0 compatibility notes cover save migration, catalog updates and the Lore development gate. The Toolkit is not needed to play.

### Localization and verification

- Completed the new item/condition descriptions, lucky-result messages, migration screens and Lore controls in all **15 supported locales**: English, Russian, German, Spanish (Spain/Latin America), French, Indonesian, Italian, Japanese, Korean, Dutch, Polish, Brazilian Portuguese and Simplified/Traditional Chinese.
- Replaced punctuation that the game's fonts cannot display in affected languages, and clarified the distinction between a run and a round in migration messages.
- Checked localization keys, substitutions and formatting, alongside focused content, migration, boss-resource and UI tests. Isolated Windows game checks cover the new behaviors without changing player saves. Full ordinary-campaign playtesting and native-speaker review remain broader checks; see [validation and limitations](docs/KNOWN_ISSUES.md).


## 1.1.0 — 2026-09-13

### Added

- **Casting mode**, with separate Easy, Standard and Hard presets. Before each run, choose three of seven offered Jokers to unlock for the series. Fewer choices remain when the locked roster runs out. Unlocking a Joker allows it to appear; it does not grant the card.
- Joker, the four suit Jokers, Mr. Bones and Yorick are available from the start. Yorick retains its legendary sources.
- The first Casting card is revealed by default. Four new vouchers reveal positions 2, 4, 6 and 7. Intermission removes these vouchers while preserving unlocked Jokers.
- New Casting conditions can restrict future appearances of Jokers, consumables, vouchers and packs.
- **Banner integration:** restrictions from different sources combine. Removing one ban does not override another. Cards already obtained are not removed.
- Optional **Card Sleeves, Partner and supported starting additions**. Before each run, choose one of up to five options in each enabled category, or continue without one. Offers and selections survive reloads.
- Starting additions are disabled in all six built-in presets. Enabling them disables Dark Star rewards.
- **Expanded integration API v1, revision 1.1.0:** register items, conditions, custom effects, ban sources, starting categories and Showman reactions.
- A separate **ModdingKit** with a working example, API reference and English/Russian PDF guides.

### Improved

- The main-menu panel now highlights the current win streak and personal best. Large values use compact formatting, with full counts available in tooltips.
- Refined the next-run shop panel, action buttons and remaining-wins indicator. Price tags scale with enlarged packs.
- Added a preset selector before starting a series and a clear Dark Star eligibility line. Editing a preset switches the draft to Custom; returning from customization preserves the draft.
- Saved series display their rules profile above Continue, using **Custom** for custom rules.
- Expanded Showman to **541 reactions in all 15 supported languages**, with contextual expressions, varied delivery, remembered recent lines and short variants for tighter layouts.
- Updated Showman's presentation to use the author's **32 drawings**, with matching mouth animation where available and reduced-motion support.
- Completed translations for the new interface, descriptions and integration messages.
- New series containing Showstreak items or conditions supplied by another mod are ineligible for Dark Stars. Existing series retain their saved content and reward policy.

### Fixed

- Fixed a crash when hovering Casting, Sleeves or Partner in **Extras**, including the saved-preset editor.
- Corrected tooltips for starting-category tabs, “without an addition” buttons and the category carousel. Long descriptions now wrap correctly.
- Empty starting categories no longer ask players to choose “1 of 0” or describe an empty selection as a selected item.
- Starting additions display their own descriptions instead of the ability of a card used only as an illustration.
- Corrected temporary-condition eligibility checks when a mod defines tiers that do not increase in strength.
- Added safeguards against repeated custom-effect and starting-addition bonuses when continuing a saved run. Missing or incompatible required integrations produce an explanation.

Lore is unchanged in this update.


## 1.0.0 - Initial release

The first public release of **Showstreak**, by **TraditionalDimension**.
Available on [Nexus Mods](https://www.nexusmods.com/balatro/mods/947).

Showstreak connects fresh Balatro runs into an ongoing win-streak challenge.
The Showman chooses your next deck; victories fund preparations, while new
conditions change what it takes to keep the streak alive.

### Gameplay

- Win-streak campaigns with assigned decks, rising stakes, act progression,
  and local records.
- A shop between runs with props, tricks, vouchers, packs, and bets. Props
  and tricks enter your held inventory when bought; Use prepares a prop for
  the next run or performs a trick's immediate action.
- Easy, Standard, and Hard presets, plus custom starting resources, resource
  bounds, and deck selection. Save, import, and export your own rule presets.
- Twelve condition families for new campaigns. Masks reveal their exact
  effects when accepted and must be acknowledged before the next run.
- Optional Intermission offers after enough acts: clear accumulated effects
  and restart local progression while keeping your total wins, profile Dark
  Stars, and the campaign's fixed rules and catalog.
- Persistent Dark Stars for completing acts in campaigns using eligible
  official presets, with separate balances for each profile.

### Interface and Showman

- Contextual Showman dialogue and interface text in 15 languages.
- Animated idle blinks, glances, and brief event reactions that return to a
  resting pose.
- Separate dialogue, voice, effect-sound, and reduced-motion preferences.
  Showman voice playback uses sounds from the installed game; original or
  processed Balatro voice files are not bundled.
- A movable main-menu panel with a saved position and a reset control.
- Controller focus/navigation, in-game help, item tooltips, and a view of
  active preparations and modifiers.

### Saves and integrations

- Separate campaign and active-run saves that leave the ordinary Balatro
  run save independent. Campaign progress uses two alternating, checksummed
  save slots.
- Recovery screens for save errors and an explicit retry for failed loss writes,
  preserving the original result without duplicate history.
- Saved rules and catalogs remain fixed for an existing campaign. Missing
  required providers or decks are reported before continuation.
- Public integration API v1 for items, deck adapters, and contextual Showman
  reactions, with an API reference, local tutorial examples, and English and
  Russian modding guides in the separate developer materials.

### Requirements and limitations

Requires **Lovely 0.9+** and **Steamodded 26.829.0+**, installed separately.
The tested baseline is **Windows, Balatro 1.0.1o-FULL, Lovely 0.9.0, and
Steamodded 26.829.0**.

- Dark Stars can be earned and saved, but this release has no Lore purchases,
  unlockable story entries, or chapters.
- New content and Intermission policies do not silently enter older saves.
  Modded decks need explicit adapters; an integration badge is not a guarantee
  of compatibility with every other hook or mod combination.
- Campaign A/B saves do not replace a backup of the active-run save. Back up
  all campaign and run files together.
- Physical-controller use, smaller windows, sound levels, balance, and
  translation quality still need broader player evaluation. Linux, macOS,
  Steam Deck, and future loader versions have not been verified by the
  Windows acceptance checks.

See [KNOWN_ISSUES.md](docs/KNOWN_ISSUES.md) for validation scope and remaining checks,
[README.md](README.md) for installation and updates, and [LICENSE](LICENSE)
for terms of use.
