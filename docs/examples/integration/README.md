# Optional Showstreak integration example

This folder is inert inside Showstreak: its executable files have `.example` extensions. No example items are loaded by a normal installation.

To try it, create a separate `Mods/ShowstreakExample` folder and copy:

- `main.lua.example` as `main.lua`
- `ShowstreakExample.json.example` as `ShowstreakExample.json`

Restart Balatro with Showstreak 1.0.0 or newer, and use an isolated test profile. The Mods tab should mark **Showstreak Integration Example** as integrated. Existing series keep their catalog; start a new series to make these items eligible.

The example contains:

| Item | ID | Behavior |
| --- | --- | --- |
| Spare Hand | `showstreakexample_spare_hand` | Costs 2 Stars. Buying adds it to held items; manual Use grants +1 hand each round in the next run. |
| Stage Budget | `showstreakexample_stage_budget` | Costs 3 Stars. Buying grants +$2 starting money in subsequent runs until an accepted Intermission or the series ends. |

Both use native artwork and include English and Russian text. Buying Spare Hand also supplies a contextual Showman reaction. Offers are random, so an item need not appear in the first shop. The example does not alter the shop seed or force offers.

The provider is the manifest ID `ShowstreakExample`; item and reaction namespaces use `showstreakexample_`, independently of the SMODS prefix `sse`. Registration runs at priority 10000011, after Showstreak.

For a complete check: buy Spare Hand, verify that purchase alone does not change the next run, Use it, then begin a run and compare hands with the selected deck/stake baseline. Restart during the series and confirm that the item and modifiers persist. In a separate update test, retire Spare Hand from this file while leaving Stage Budget registered: an existing held Spare Hand should still display and use its frozen definition. Do not remove the entire required provider until that series is complete; a missing provider intentionally blocks continuation.

This example is a starting point, not a compatibility certification. See [the API contract](../../API.md) for supported effects, frozen catalogs, deck adapters, event data and limits. Shared rule presets are JSON files handled through the Rules importer, not item registrations.

## По-русски

Пример сам не загружается. Для локального учебного запуска скопируйте два файла в отдельную папку `Mods/ShowstreakExample`, убрав суффикс `.example`. Перезапустите игру с Showstreak 1.0.0 и создайте новую серию в тестовом профиле. «Запасная рука» сначала покупается в инвентарь и затем применяется кнопкой Use; «Бюджет сцены» действует после покупки до принятого Антракта или конца серии. Существующая серия сохраняет свой каталог, поэтому новые предметы появятся только в новой.

Подробное руководство: [MODDING_GUIDE_RU.md](../../MODDING_GUIDE_RU.md). Для публикации создавайте собственный мод-провайдер с зависимостью Showstreak и соблюдайте [LICENSE](../../../LICENSE). Не включайте файлы исполнения, графику, звук или сохранения Showstreak в свой архив.
