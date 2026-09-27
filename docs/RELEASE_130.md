# Showstreak Toolkit — 1.3.0 notes

Use this release note and the [API appendix](API_130.md) alongside the [baseline reference](API.md). Public API contract **1**, revision **1.1.0**, remains in place. Capability discovery identifies additive features; the filename of an internal content module is not a public API version.

## Gameplay and saved contracts

The player changelog lists all new items, packs, conditions and Bets. A new catalog has three base Bet offers, with two extra from Grand Bill, while acceptance remains limited to two. Existing frozen offer lists are not regenerated merely to fill new columns. Missing external difficulty metadata produces Other, not an inferred category.

The required-Ante bounds of new official Easy, Standard and Hard variants are 2–10, 4–16 and 6–21. Existing series retain their saved rules. Ante preparation rechecks accepted Bets so an adjustment cannot silently erase the risk for which a reward was accepted. Boss programs also recheck target/minimum Ante and conflicts.

The release uses **store schema 7**. The update is explicit and previewable, with verified backups and stale-source checks. Catalog additions remain a separate opt-in between runs. Native runtime markers, pending pack choices, preparation grants and boss-objective state have their own validation. Do not directly rewrite these internals from an integration.

The schema update raises the five known native objective-Bet definitions to at least the current +5/+7-star rewards and replaces Fixed Blocking's old Blueprint illustration with Rough Gem. It preserves objective state, eligibility, saved offers, RNG and reward history; previously settled wins receive no retroactive payout. A matching native run marker adopts the revised definitions in memory on resume after campaign/run ID checks. Its file remains unchanged until an ordinary run checkpoint.

Old save data is not completed with guessed history or current metadata. In particular, a new provider declaration does not change an already frozen starting-addition contract. Required providers and their saved versions must remain compatible when resuming. A backward downgrade needs the matching backup.

## Author-facing additions

- Version-2 pack receipts can contain consumables, vouchers and Bets, with type-specific grants. Query `capabilities.pack_pools >= 2` before registering the new pools.
- `starting_joker_count` declares a conservative maximum occupancy for a custom starting category. Absence remains unknown, including in older frozen series.
- Action-policy sources can report restrictions and uncertainty relevant to new Bets, conditions and assigned bosses. They must not mutate gameplay or consume RNG while queried.
- External consumables require explicit safe-copy permission for Reprise. Bet difficulty is authored metadata, not inferred from price or reward.
- Shop catalog and Run Info previews are read-only. Hover, refresh and paging must not execute a custom effect or draw random rewards.

See [API_130](API_130.md) for fields, examples and boundaries. Compound native reward data such as `boss_program`, `bonus_effects`, `reward_by_stake`, `shop_credits` and starting-grant tokens is not a general supported API for arbitrary new effects. Use the supported custom-effect contract and declare related policies.

## Lore Workshop

The standalone editor now has an independent RU/EN interface and story-text selector, shared text validation, compound speech/captions, panel and page-image ordering, overlap, reparenting, multi-selection, resource replacement, document/text Undo/Redo, recovery and checkpoints. Save updates the manual project; autosave and closing write a separate recovery draft. Copy the whole project folder to preserve editor metadata and versions.

Exports remain **story schema 2**. Schema-1 imports can migrate; arbitrary Lua, nested runtime panels, native rotation/crop/filter fields and transform keyframes are not introduced. The crop action bakes a separate PNG instead of exporting unsupported runtime fields. Five shared files are shipped: `schema.lua`, `player.lua`, `renderer.lua`, `example.lua`, `text_layout.lua`.

LÖVE 11.5 must be installed separately. In `LoreEditor`, run `python build.py` to rebuild from local modules, codec, fonts and licenses, without Balatro or Steamodded. `dist/build-manifest.json` lists the archive and member hashes. Windows launchers do not install a runtime.

**Normal player Lore remains In development.** A development setup must explicitly enable `Showstreak.lore_development = true` before testing the game reader. Keep authoring and reader tests in an isolated profile. The editor never spends Dark Stars or changes a game profile.

## Retained material

The integration example and English/Russian guides remain the foundation for initialization, provider namespaces, save-safe effects and bans. The existing PDF guides describe the API 1.1.0 baseline. They are retained with this current appendix, not presented as rewritten 1.3.0 PDFs. Keep existing LICENSE and third-party notices with their files.

## По-русски

Контракт API 1 и ревизия 1.1.0 сохранены; новые возможности проверяются через capabilities и описаны в [приложении API](API_130.md). Формат хранилища — schema 7, обновление с подтверждением и копией. Каталог добавляется отдельно; правила, предложения и отсутствующие старые метаданные не подменяются текущими значениями.

Главные дополнения для авторов: новые пулы наборов с корректной выдачей ваучеров/Bets, объявление максимума начальных джокеров, политики ограничений, метаданные сложности и безопасного копирования. Старые PDF остаются базовыми руководствами API. Редактор имеет русский интерфейс, слои, отмену/повтор и версии; экспорт по-прежнему schema 2, лор игрока ещё не открыт.
