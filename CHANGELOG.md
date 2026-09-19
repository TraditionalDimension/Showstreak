# Changelog

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
