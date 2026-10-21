# Changelog

## 1.4.1 — 2026-10-20

### Showman

- Added **800 reactions in all 15 supported languages**, bringing the built-in total to **1,341**. Existing situations now have at least 16 eligible repeatable lines, with separate banks for native-shop actions, selected/unselected Partners and Sleeves, and collection discoveries. One-time tutorials are counted separately.
- Added **16 profile-name asides each for 18 creators**: DrSpectred / Balatro University, Roffle, Northernlion, ZainoTV, GothicLordUK, Skoottie, Frost Prime, Aliensrock, Retromation, Olexa, Haelian, Rarran, RTGame, Dan Gheesling, MALF / michaelalfox, Sizaak, Bean and Focushock. Exact local aliases recognise stage names, not authenticated identities; no account or network lookup is performed. They appear in the Showstreak hub, including before a series exists. **Profile-name jokes** defaults on and follows the main dialogue setting.
- Added 16 asides for **Cryptid** and eight each for **Pantomime Paradox, All in Jest, Extra Credit, Paperback and CardSleeves**. These recognise enabled, loaded mods by their manifest IDs. The original Cryptid Spectral card does not trigger a mod aside. Mod comments share a cooldown, follow the initial greeting and are excluded from loss screens.
- Creator and mod asides share at most one displayed rare greeting per profile per application session. First-time teaching and important results retain priority. Renaming a profile or switching away and back does not reset that budget or the saved reaction history. Mod comments also retain their three-win cooldown between sessions.
- Repeatable reactions now use saved shuffled bags: an eligible bank is exhausted before another cycle begins. Specific reaction priorities and one-time introductions remain supported. Only displayed reactions consume their place; speech uses its own randomness.
- Added reactions to newly saved Showlection discoveries and original Legendary variants. Record and Act celebrations retain priority. A failed save, a repeat discovery or reopening the same result does not produce a new collection congratulations.

### Showlection and interface

- Reworked Run Info around a fixed overview of starting resources, one active-effect gallery and one page control. Removed repeated duration captions, bounded resource explanations and restored native deck-ability tooltips in preparation. Small resource labels use light ink while values retain their resource colours.
- Gave Intermission reset values a wider two-by-two layout and aligned Sleeve/Partner choice buttons beneath two-line name slots. Secondary navigation and collection filters use quieter, consistent colours; preparation panels have a darker surface.
- Shortened selected Lucky, starting-Joker, compound-effect and Boss Bet descriptions across all 15 locales without changing odds, penalties, rewards or duration. Blueprint and Brainstorm names follow the game's selected language. Catalogue technical details are optional; effects, costs and requirements remain available.
- Fixed Showstreak's Reduced Motion setting for Lucky effects, added a one-time Lucky Curtain verdict and completed quiet feedback for successful deck changes, start choices and Intermission decisions. Ordinary shop refreshes cap entrance staggering; failed actions and redraws do not replay success cues.
- Split config into five framed category pages with consistent category colours, a visible selected state and pagination for future groups and switches. Backups remains available beneath every category and through the existing Saves tab.
- Gave backup entries Manual / Automatic labels and distinct accents, with a gold outline around the selection. New action buttons use consistent colours for creation, inspection, details, navigation and rollback confirmation; labels remain explicit.
- Replaced the deck-change voucher icon with a short **Change** label in all 15 locales, kept its Star cost visible and removed the button tooltip.
- Trimmed the illustrated discovery guide to its three steps and Dark Star eligibility row; removed the example, modifier, Conditions and Archive footer rows with guide tooltips both on and off.
- Removed Lucky Curtain's redundant failed-roll line in every locale; its chance, Ante floor, alternative Star reward and duration are unchanged.
- Clarified reachable original-game variants: the five Legendary Jokers have their base form and four editions in ordinary runs. Eternal Legendary forms from Challenges are outside collection goals. The original totals remain **4,085 Joker forms and 8,340 playing-card forms**; winning-stake badges remain excluded.
- Added an optional `sstreak_collection_stickers` declaration for modded Joker centres. Providers can enumerate reachable original sticker combinations instead of relying on rarity alone. Declarations cannot enable incompatible stickers or Eternal + Perishable, or change the original Joker catalogue. Previously discovered forms outside the current catalogue remain browseable.
- Added softly shaded number fields for the current and best streak on the main-menu sign. Existing sizing, tooltips and dragging remain supported.
- Added brighter gold digits and a lightweight, glyph-masked gold sheen on the two menu counters. **Golden streak numbers** is enabled by default and can be switched off for plain gold. Reduced motion freezes the sheen; unsupported shaders fall back to plain colour.
- Moved the opening explanation above New Series into a compact `?` tooltip beside the hub title. The full translated help remains available on hover or controller focus.
- Added **Tooltips and ? icons**, enabled by default, across Showstreak menus, setup, shop, collection, results, records and config. Turning it off suppresses owned popups and hides help question marks. Unknown card/data markers keep their meaning; ordinary Balatro tooltips are unaffected.
- Switching tooltips off also closes an already focused Showstreak popup immediately, including UI elements held only by their native UIBox.
- Added a temporary **Guide tooltips** switch to Showlection's illustrated discovery guide. It starts **off on every entry**, covers the example cards as well as explanations and question marks, and cannot override the global preference. Redrawing the guide preserves the temporary choice and the same example cards and boss; leaving resets it.
- Shortened Showstreak-owned help across the hub, collection, preparation, shop guide and config in all 15 locales. Removed repeated Legendary/archive paragraphs and whole-panel hover targets from the discovery guide; enabled explanations use small, focused question marks. Exact values, reset conditions and native card descriptions remain available.

### Compatibility and Toolkit

- Added manual checkpoints to the existing backup browser: choose a reason, add an optional 48-character Unicode note and save a verified copy of the Showstreak profile and its saved run. The reason is visible in the list; details include the saved mod versions. Creating a checkpoint does not advance the series or change its seed.
- Kept player-facing save migrations and their automatic pre-update backups. Restoring a manual or automatic copy still previews the rollback, creates a verified copy of the current state first and uses the existing interrupted-restore recovery. Create and restore require leaving the native run; mod files and external mod settings are not part of the checkpoint.
- Profile schema **8**, Showlection storage **1** and Showman memory **2** remain readable without a format migration. Missing cosmetic memory fields start empty; existing discoveries are retained.
- API contract **1**, revision **1.3.1**, adds `showman_cycles=1` and `collection_sticker_policy=1`. Updated the separate Toolkit, collection notes and LuaLS metadata. Lore Workshop, assets, licences and historical PDF manuals are preserved.


## 1.4.0 — 2026-10-18

### Showlection

- Added a profile collection of card variants retained when an eligible Showstreak run is won: **4,085 Joker forms and 8,340 playing-card forms**, for **12,425 original-game goals**. Owned Jokers and the whole remaining deck are checked at the final boss victory. Buying, using or playing a card alone is insufficient; sold and destroyed cards do not count. Debuffs and expired Perishable do not exclude a retained card.
- Variants use original editions, enhancements, seals and compatible Eternal / Perishable / Rental states. Stone Cards share one identity regardless of their hidden suit or rank. Winning-stake badges and results are not part of this collection.
- The optional **Mod collection** adds registered Joker identities, suits and ranks, while retaining the original modifier vocabulary. It is **off by default**. Custom editions, enhancements, seals, stickers and properties do not multiply the goals. Turning it off hides mod entries without deleting discoveries and skips the Original / Mods chooser.
- Added native card grids, original/modded categories, search with cancellable drafts, discovered/undiscovered views, recent discoveries, filters, page controls and direct page entry. Returning from details or help restores the browsing context. Short windows show fewer card rows while retaining readable text.
- Added an illustrated **?** guide with random original-game Joker and playing-card variants and one of the five original final bosses. Both sides of the example use the same retained variants. Details are available on hover or controller focus; examples do not award progress or advance gameplay randomness.
- Undiscovered cards explain Showlection's own victory requirement. Undiscovered playing cards use the original Blue Deck back; undiscovered Jokers keep the native hidden-Joker artwork. Previews do not reveal hidden identities or modifiers.
- Discoveries, the victory and Dark Stars share the verified A/B save transaction. Failed writes cannot award partial progress; retrying a result cannot grant the same discoveries twice. Optional discovery notifications open the new results safely.
- Existing observations remain readable. A separate display projection merges old extended variants without erasing their raw records, inventing past victories or awarding duplicate discoveries. Earned entries remain archived when their provider is absent.

### Interface and records

- Compacted the floating menu into one row of **Streak / To Show / Best**, with labels above stable-width values. Pale gold-grey counter panels improve contrast; native hover behavior and exact-value tooltips remain. The Showstreak hub has separate collection, Lore and Dark Star controls. Dark Star displays use a consistent white plate.
- Refined preparation, records, series details, Intermission and content-update screens around native Balatro panels, larger counters, short captions and explanatory tooltips. Intermission shows the actual quantities retained or reset. Confirmation consequences remain visible at every information level.
- Added **Player information: Off / Only important / Detailed**, defaulting to **Only important**. It is independent of the existing optional Action previews, which remain off by default.
- Records separate the personal maximum from records under comparable conditions. New series retain their starting conditions, confirmed record prefix and significant events. Older histories show what was actually recorded rather than reconstructed conditions.
- Added keyboard arrows, Enter and Escape through native controller navigation on Showstreak panels, while preserving normal text-entry handling.
- Added stable translations for the new collection, preparation, record and decision screens in all 15 supported locales, including their help and refusal messages. Replaced punctuation missing from the native fonts.
- Refined shop audio using existing Balatro sounds: paid purchases, vouchers, pack wrappers, claims and item effects have appropriate cues. Failed or repeated actions do not play success cues. Free purchases omit payment sounds, and the effects preference also silences lucky-result animation sounds.

### Saves and reward eligibility

- Updated the profile format to **schema 8** with the existing explicit backup/update flow. Updating the format and adopting a new gameplay catalog are separate decisions. Existing earned rewards and frozen definitions are preserved; exact older conditions and card forms are not invented.
- Rule assessment compares the actual rules with the supported references rather than trusting an official-looking preset name or import label. New series freeze their rules, supported deck pool and gameplay environment. Supported harder score multipliers and lower starting Stars can retain admission.
- Adopting foreign gameplay content stops future Dark Star payouts, including the unfinished Act, while retaining earned Dark Stars and total wins. Changed or unavailable required providers cannot silently reinterpret a saved run.
- Added a separately audited **Bunco11** ladder and six corresponding presets for Bunco **5.4.8c~JumboFork** with its default supported gameplay options. Its reward policy uses 1 / 2 / 3 Dark Stars per completed Act. Bunco's ordinal 8 is not treated as the original Gold Stake. These gameplay rules are independent of Showlection's stake-free display.
- Saved custom effects resolve their exact provider/ID/version. Authors can retain a legacy handler alongside the current one. Missing versions block continuation with an explanation instead of replaying grants or switching to a newer effect.

### Compatibility fixes

- Checked Pantomime Paradox entities, Bunco's new Jokers and suits, UnStable's **75 Jokers and 52 playing-card fronts**, and CardSleeves **1.9.4**, individually and in the tested combinations with Pantomime. Fractional rank keys and declared card sizes are preserved. This is not certification of arbitrary mod mixtures.
- Neutral collection previews bypass gameplay initialization and modifier application. They preserve live deck IDs, `used_jokers`, debuffs and gameplay randomness. Bunco previews no longer update real Jokers; UnStable tooltips receive bounded visual defaults without running gameplay initializers.
- Original Joker previews now use their proper base X Mult, required initial display counters and native Half Joker / Photograph / Square Joker / Wee Joker proportions. To Do List tooltips no longer fail because a preview lacks its target hand. Standalone preview construction also preserves the used-Joker ledger.
- CardSleeves previews no longer let Checkered change the shown suit or Abandoned change rank and consume randomness. Live sleeve state is restored after nested previews and errors; real cards keep their effects.
- New sleeve-enabled series freeze the host and sleeve-provider versions. New starts and resumes check the chosen sleeve against the native save before construction. Disabled sleeve choices do not affect resources or inherit the ordinary menu's sleeve preference. Sleeve-enabled rules still require a separate Dark Star admission policy.
- Reduced the starting-addition selection screen's height without shrinking text or artwork. Offer metadata and readiness checks no longer allocate discarded tooltip objects. Unused description titles and info blocks are released, fixing names accumulating in the corner of the screen.

### API and Toolkit

- Public API remains **contract 1**, now **revision 1.3.0**. Added structured definition validation, copied preparation/run/series views, action refusal reasons, exact effect-version queries, declarative preparation recipes, boss programs and pack pools. Capabilities must be checked before use.
- Added collection metadata through `get_collection_info()`: `collection=1`, `collection_axes=0`, `modifier_policy='vanilla_only'` and `winning_stakes=false`. Legacy `register_collection_axis` calls remain load-compatible but create no goals or filters and their callbacks are not used by the current collection.
- Updated the separate **Toolkit 1.4.0**, current API appendix, LuaLS schemas and annotations, examples and trusted-provider verifier. The verifier checks the actual host and both provider load orders; its model tests are distinct from native game acceptance.
- Lore Workshop source, portable `.love`, assets, fonts, licenses and the historical **1.3.1 EN/RU PDF manuals** are preserved. Current changes are documented in Markdown. Story format remains **2**; normal player Lore remains **In development**.

### Verification and scope

- Focused logic tests and isolated Windows game checks cover collection policy, rule admission, transactions, provider changes, previews, UI navigation, saved starts and resumes. The release polish adds localization, guide, unknown-card tooltip and layout checks, including repeated opening/cleanup and multiple window sizes.
- Personal saves and settings are protected by separate QA paths and before/after hashes. Native launch/resume checks use the real engine; completed-win observations used for payout and collection assertions are explicit test fixtures. Ordinary campaign balance, native-speaker review, physical controllers and other operating systems remain broader checks.
- Larger new mechanics and further content-mod expansion are deferred to **1.4.1**. Fully restart Balatro after installing this update.


## 1.3.3 — 2026-10-13

### Interface

- **Optional action previews:** the extra block below shop item descriptions is now controlled by **Action previews** in Showstreak's settings. It is **off by default**, including when upgrading a configuration without this setting. The selection is saved across restarts.
- **Clearer enabled previews:** compact action headings, coloured resource labels and values, native Stars and localised stake names replace the plain text block. Buying, using a held item and selecting a pack reward have their own headings. Refused resource changes retain the exact minimum or maximum. Generic lines that repeat the item's description are omitted.
- **Card presentation screens:** the new Act condition and Intermission offer have separate background panels for the heading, card and description.
- **Run Info:** cards are centred over their source and detail captions, fixing the offset between Vouchers and labels such as Series. The existing eight-card pages display their arrows and page count on a separate panel when multiple pages are needed.
- **End Series:** the button and its confirmation use a darker red, making them visually distinct from Main Menu.

### Compatibility

- Save schema **7**, API contract **1** and API revision **1.1.0** remain unchanged. Compatible existing series continue without a migration prompt; saves that require conversion retain the existing backup and confirmation flow.
- The public `preview_item_action` API remains available independently of the display preference. Previews continue to use disposable data and keep random outcomes and unpurchased pack contents hidden.
- New preference labels and preview headings are provided in all **15 supported languages**.


## 1.3.2 — 2026-10-12

### Fixed

- **Scheduled Boss Bets:** the assigned boss is now selected through Steamodded's current blind-selection path, restoring the intended encounters and objective tracking for Dry Run and Fixed Blocking. Objective bonuses still require both the task and the whole run to be won, and remain protected against repeat payment after reload. Fixes the scheduled-boss problem reported by **ARK-MAXIM** in [issue #4](https://github.com/TraditionalDimension/Showstreak/issues/4).
- **Blue Skittles:** using it immediately sets the current selected stake to Blue, whether the previous stake was higher or lower. Later preparation effects can change the stake again. Its Blueprint grant, Joker-slot cost and existing once-per-preparation rule retain their own behavior; descriptions clarify the stake assignment in all 15 languages.
- **Leaving a series run:** Main Menu correctly returns to the game's menu between runs and after a loss, preserving the preparation or completed-series state.
- **Preparation transactions:** native stake targets and resource combinations are validated before committing; refused actions preserve inventory, Stars and remaining pack choices.

### Interface

- Card tooltips preview concrete preparation changes, stake names, action costs and useful refusal reasons. Chance effects show probabilities without exposing a sampled outcome; pack rewards appear after purchase.
- The content catalog adds localized-name/ID search and a source-mod filter, plus notices for entries included in or absent from the active series. Existing series retain their saved definitions; new content is added through Rules between runs.
- Boss objectives show their first recorded failure reason and relevant hand/card counters. Unavailable Boss Bets explain conflicts and the one-programme limit.
- The Mods tab adds **Copy problem report**, including versions, enabled integrations, preparation and objective status, and the last API diagnostic.

### API and Toolkit

- Added copied registration metadata and item eligibility queries, concrete `preview_item_action` results and a plain-text `get_diagnostic_report`.
- Added immediate `set_stake` assignment by numeric level or stable native `stake_key`; key-based Blue selection supports reordered modded stake pools.
- Added atomic `bonus_effects` for ordinary resource Props, Contracts and Vouchers, with existing lifetimes, bounds and save handling.
- Field diagnostics identify the invalid field, expected contract and actual type/value. API contract **1**, revision **1.1.0** and save schema **7** remain unchanged; use the additive capabilities.
- Updated the author documentation, added five minimal provider recipes and a general trusted-provider verifier for both load orders. The complete example, Lore Workshop and versioned 1.3.1 PDF manuals remain available.

### Rules clarification

- A run still accepts **one boss programme**, including programmes targeting different Antes. This explains the second question in [issue #4](https://github.com/TraditionalDimension/Showstreak/issues/4); an ordinary second Bet is still allowed.


## 1.3.1 — 2026-10-05

### Fixed

- **Items inside packs:** held Props and Tricks can be used while a Showstreak pack is open. Choosing a target, cancelling a choice or closing an item result returns to the remaining pack rewards without losing a pick. Unavailable actions do not consume the item or a pack choice. This fixes [issue #3](https://github.com/TraditionalDimension/Showstreak/issues/3).
- **Victory reward preview:** Run Info separates the base reward, earned bonuses and conditional bonuses. Boss objectives show their pending, completed or failed state; the extra stars from Dry Run are visible before they are earned. The bonus still requires both its objective and the whole run to be won, and cannot be paid twice after a reload. This addresses the reward-display part of [issue #2](https://github.com/TraditionalDimension/Showstreak/issues/2).
- **Leaving a lost run:** Main Menu returns to Balatro's actual main menu before another series can start. Opening and closing settings after a defeat no longer leaves the player in the completed run.
- **Automatic stake increases:** existing series no longer get stuck during preparation when a higher stake changes a resource baseline. Retained effects keep their saved values and follow the existing resource-bound rules.
- **Save failures:** an action rejected because its save could not be confirmed no longer appears as successful after reloading. A successful retry applies the action once.
- **Preparation and integrations:** invalid deck, stake or content changes are rejected before they can replace the current preparation. Errors in supported integration callbacks produce a short message and preserve the previous state.

### Interface

- Expanded the configuration's **Content catalog** with separate pages for all **100 shop items and 24 complications**, category filters, levels and Whole series / Tonight variants. Compatible additions retain their source labels.
- Improved text shadows, secondary-label contrast and separation between Showstreak panels while preserving the existing colors and artwork. Light tooltip panels keep their dark text.
- Completed localized **Custom** preset labels and the console-log setting across the supported languages.
- Moved Run Info's page controls into their section headers so dense lists fit with Japanese, Korean and Chinese fonts.
- Checked Run Info's source and metric page controls with large lists, including navigation in both directions and the first and last pages. Page changes preserve preparations, rewards and gameplay randomness.

### Logs and Toolkit

- Added **Mod Logs in Console**, off by default, for detailed troubleshooting. The Mods report includes the game and loader versions and supported integration features.
- Strengthened API registration, action checks and callback handling. The public API remains **contract 1, revision 1.1.0**, and save schema remains **7**.
- Updated the separate **Toolkit 1.3.1**, integration example, verification script and English/Russian guides, including the full PDF manuals.

The content lineup, authored item and complication effects, and prices are unchanged. Automated checks and isolated Windows game tests cover the fixes, saves and interface; ordinary campaign balance and native-speaker review remain broader player checks.

Thank you, [ARK-MAXIM](https://github.com/ARK-MAXIM), for reporting the pack-use problem and the reward-preview problem in [#3](https://github.com/TraditionalDimension/Showstreak/issues/3) and [#2](https://github.com/TraditionalDimension/Showstreak/issues/2).


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
