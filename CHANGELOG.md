# Changelog

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

### Кратко по-русски

**1.0.0 - первая публичная версия Showstreak.** Мод связывает отдельные забеги
Balatro в серию побед с назначаемыми колодами, ростом ставок, магазином между
забегами, подготовкой, купонами, паками и пари. Доступны три пресета, редактор
правил, двенадцать семейств условий, добровольный Антракт и тёмные звёзды,
сохраняемые отдельно для каждого профиля.

В выпуск входят анимированный Шоумен, интерфейс и реплики на 15 языках,
раздельные настройки звука и движения, независимые сохранения серии,
восстановление после ошибок записи и API v1 для интеграций. Руководства для
авторов модов поставляются отдельно от облегчённого архива игрока.

Покупок за тёмные звёзды и открываемых глав лора пока нет. Старые серии
сохраняют прежние правила и каталог; модовым колодам нужны адаптеры.
Проверенный набор - Windows, Balatro 1.0.1o-FULL, Lovely 0.9.0 и
Steamodded 26.829.0. Установка, резервные копии и ограничения описаны в
[русской инструкции](docs/PLAYER_GUIDE_RU.md) и [KNOWN_ISSUES.md](docs/KNOWN_ISSUES.md).
