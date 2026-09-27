# Author notes for Showstreak 1.2.0 / Авторам дополнений

> Historical notes for 1.2.0. For Showstreak 1.3.0 use [current release notes](RELEASE_130.md), [API additions](API_130.md) and [Toolkit](TOOLKIT.md). In 1.3.0 saves use schema 7 and Workshop has RU/EN UI; references to schema 5 and English-only UI below describe 1.2.0.
>
> Исторические примечания к 1.2.0. Текущая 1.3.0 использует схему сохранения 7 и редактор с RU/EN интерфейсом; схема 5 и английский интерфейс ниже относятся к 1.2.0.

## English

The public integration API remains **contract 1, revision 1.1.0**. Existing registrations and callback signatures are unchanged. The supplied example has been checked against 1.2.0 in both supported load orders. Install it only for deliberate testing; it adds gameplay content and influences Dark Star eligibility. Do not bundle Showstreak, Balatro, Lovely or Steamodded with an independently authored integration.

### Saves and catalog updates

Showstreak now uses save schema 5. A required format or compatibility-metadata update waits for a player preview and confirmation, with verified copies of existing A/B/run files. It can also be postponed. Providers must not force migration, write Showstreak save files or remove unknown saved definitions to bypass compatibility checks.

Existing series keep their frozen catalog. **Rules → Add new content** is an optional, backed-up, additive update between runs. It preserves existing IDs and definitions, owned items, preparations, current offers, rules, current RNG state/run seed and reward eligibility; adding eligible entries may change future random offers. Do not expect installing or upgrading a provider to silently refresh a series.

The new weighted built-in item families and exact-fraction metadata are internal content implementation details. The public API's documented integer weight fields and validation rules have not been widened to accept arbitrary built-in fields. Register through the API rather than modifying `Showstreak.content` or frozen definitions.

Legendary Joker and The Soul illustrations are reserved from Showstreak item artwork, including direct atlas aliases and saved definitions. Use another source; invalid reserved art falls back to Matador. This restriction concerns item illustrations, not actual Casting Jokers.

### Lore development

The normal 1.2.0 Lore route is a placeholder. The library and reader require an explicit development opt-in after Showstreak has loaded:

```lua
Showstreak.lore_development = true
```

Use an isolated test profile for in-game development because unlocking a chapter can spend that profile's Dark Stars. The editor itself never reads or writes game progress. Do not ship an integration that silently turns this development flag on for players.

The Toolkit editor and game contain byte-identical `schema.lua`, `player.lua`, `renderer.lua` and `example.lua`. Story schema 2 adds panels and transitions; schema 1 is still accepted. Package resources use relative paths. Exported `.sstory` files are ZIP archives with `manifest.json` at the root.

With development enabled, story providers can place packages in their active mod's `showstreak/lore/`. An optional `index.json` is an exhaustive list, not an additional list. Package order, before/after hints, required IDs and duplicate handling are documented in the editor guide. Missing or conflicting packages produce diagnostics; they are not silently merged.

The editor's `.love` runs with a separately installed LÖVE 11.5. Its source build works with Python 3.8+ without the developer's directory layout. macOS/Linux use the same portable archive but still need native testing. The interface is currently English; multilingual story data is separate from UI translation.

### Documentation

The Markdown API and these notes state the current compatibility behavior. The included English/Russian PDFs remain the checked 18-page **API revision 1.1.0** guides. The save-schema paragraph in those older guides is superseded by this document. They do not describe the new Lore editor or all 1.2.0 built-in items. Player-facing balance and behavior are listed in the supplied [CHANGELOG](../CHANGELOG.md).

## По-русски

Публичный API остаётся **контрактом 1, ревизией 1.1.0**. Регистрация и сигнатуры обработчиков не изменены; пример проверен на 1.2.0 при обоих порядках загрузки. Устанавливайте пример только для тестирования: он добавляет игровой контент и влияет на право новой серии получать тёмные звёзды.

Сохранения используют схему 5. Обязательное обновление формата или метаданных требует просмотра, подтверждения и проверенной копии A/B/run; его можно отложить. Дополнения не должны принудительно мигрировать серию, писать её файлы или удалять неизвестные определения ради обхода проверки совместимости.

Каталог старой серии остаётся прежним. **Rules → «Добавить новый контент»** между забегами добровольно добавляет отсутствующие совместимые записи с резервной копией. Прежние определения, покупки, подготовки, предложения, правила, состояние RNG/сид забега и право на награды сохраняются. Будущие случайные предложения могут измениться. Обновление установленного дополнения не означает автоматическое обновление каталога.

Дробные веса и дополнительные поля встроенных семейств относятся к внутреннему контенту. Публичные целочисленные поля весов и ограничения API не расширялись. Используйте регистрацию API, не меняйте напрямую `Showstreak.content` и сохранённые определения.

Для иллюстраций предметов нельзя использовать легендарных джокеров и Soul, в том числе через координаты атласа. Запасная иллюстрация — Матадор. Самих джокеров в Кастинге и игре это правило не запрещает.

Лор в обычной 1.2.0 пока показывает заглушку. Для разработки после загрузки Showstreak явно задайте `Showstreak.lore_development = true`. Не включайте этот флаг молча в дополнении для игроков. Проверяйте покупку и чтение глав в отдельном профиле: они могут менять его тёмные звёзды; сам редактор игрового прогресса не касается.

У редактора и игры одинаковые schema/player/renderer/example. Формат 2 добавляет панели и переходы, формат 1 остаётся совместимым. Ресурсы используют относительные пути. При включённой разработке истории обнаруживаются в `showstreak/lore/` активных модов, а необязательный `index.json` задаёт полный список. Порядок, зависимости и конфликты описаны в инструкции редактора.

Для запуска `.love` установите LÖVE 11.5, для пересборки исходников — Python 3.8+. Исходники Toolkit автономны, без Balatro и Steamodded. Интерфейс редактора английский; тексты историй могут иметь переводы. Для macOS/Linux используется тот же архив, но отдельная нативная приёмка ещё нужна.

Приложенные PDF по 18 страниц описывают **API ревизии 1.1.0**. Их прежние формулировки о схеме сохранения заменяются этим документом; редактор и новый контент 1.2.0 описаны отдельно. Баланс и поведение для игроков — в [CHANGELOG](../CHANGELOG.md).
