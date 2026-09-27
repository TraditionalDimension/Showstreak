# Showstreak Toolkit 1.3.0 and Lore Workshop

[Download Toolkit 1.3.0](https://github.com/TraditionalDimension/Showstreak/releases/download/1.3.0/Showstreak-Toolkit-1.3.0.zip) · [Release 1.3.0](https://github.com/TraditionalDimension/Showstreak/releases/tag/1.3.0) · [По-русски](#по-русски)

Toolkit contains author documentation, an integration example, and the standalone
Lore Workshop comic editor with source and a portable `.love` build. It is
optional for players. Install the separate [player download](https://github.com/TraditionalDimension/Showstreak/releases/latest/download/Showstreak-BMM.zip)
to play Showstreak; do not copy the entire Toolkit into `Mods`.

**Normal player Lore remains In development in 1.3.0.** Creating or exporting
a story does not unlock a released in-game story catalog. Development reading
requires the explicit flag described below.

## Choose your starting point

| Goal | Start here |
| --- | --- |
| Register items, conditions, decks or custom effects | [English modding guide](MODDING_GUIDE_EN.md) / [Russian guide](MODDING_GUIDE_RU.md) |
| Look up fields and callback contracts | [API reference](API.md) and [1.3.0 additions](API_130.md) |
| Test a complete baseline integration | [Inert repository example](examples/integration/README.md) |
| Understand saves and compatibility | [1.3.0 release notes](RELEASE_130.md) |
| Create a comic without Balatro | Lore Workshop instructions below |

Showstreak is **1.3.0**; its public API is **contract 1, revision 1.1.0** with
additive capabilities. Game saves use **schema 7**, and exported stories use
**schema 2**. These are independent version numbers. The Toolkit PDFs retain the
API 1.1.0 baseline; use the current Markdown and appendix for newer behavior.

## Run Lore Workshop

1. Extract Toolkit outside `Mods`.
2. Install [LÖVE 11.5](https://love2d.org/) separately. It is not bundled.
3. Open `LoreEditor/dist/ShowstreakLoreEditor.love` with LÖVE. On Windows you can
   use `LoreEditor/Start-LoreEditor.cmd`; its launcher checks PATH, standard
   locations and `SHOWSTREAK_LOVE`, or accepts `-LovePath` through the PowerShell launcher.
4. Choose **UI: RU / EN-US** for the interface and **Text: ru / en-us** for the
   authored language. These controls are independent and preserve other translations.

Balatro and Steamodded are not required to edit or preview a story. Python 3.8+
is needed only for rebuilding from the included source with `python build.py`
inside `LoreEditor`. The packaged editor shares schema, player, renderer, example
and text-layout modules with the mod. Native acceptance is Windows-focused;
the portable archive does not establish tested macOS/Linux support.

## Create, preview and export

Create a project, then acts, chapters and pages. Add panels and image/text/shape
layers, or **Speech / Caption** objects. Panels are containers; layers can overlap,
change order or move between containers. Multi-selection and compound objects
help move related content together. Use the timeline for manual/automatic steps,
sounds and fade, slide, wipe, scale or shake transitions. Preview uses the shared
player; the story-language selector also selects which translation is checked.

**Check** reports errors and warnings. A draft may be saved with errors, but export
requires validation. The Balatro quality profile checks text fit and required
resources; the preview does not silently shrink overflowing text in that profile.
Optional missing artwork uses a placeholder and optional missing sound becomes
silence. Required missing resources block export. Cropping bakes a separate PNG;
it does not introduce a runtime crop field.

Export creates `.sstory`, a ZIP containing a root `manifest.json` and relative
resource paths, plus installation guidance and an optional index template.
Exports remain schema 2 and can import/migrate schema-1 stories. Limits are
4 MiB for the manifest and 128 MiB for the package. Arbitrary Lua, nested runtime
panels, rotation, transform keyframes and a character library are not supported.

## Save and recover a project

- **Save / Ctrl+S** updates the manual `manifest.json` / `editor.json` pair.
  `editor.json` preserves compound-object links and workspace metadata.
- Autosave, focus loss, switching projects, export and closing write a separate
  recovery draft. They do not replace the manual save. Recovery offers the
  draft or manual version before further editing.
- Document Undo/Redo holds up to 60 session commands; the active text field has
  a separate history of up to 80 input states. **Versions** stores persistent
  named checkpoints independently of those limits. Restoring one is undoable.
- Copy the **whole project folder**, including resources, metadata and checkpoints,
  to keep it editable on another computer. `.sstory` carries runtime data, not
  editor history or compound links. Save a copy retains published IDs; give it a
  new package ID before publishing it as a different story.

**Open workshop folder** opens the actual project location. In a normal Windows
LÖVE launch it is `%APPDATA%/LOVE/ShowstreakLoreEditor`. The editor does not spend
Dark Stars or modify game profiles. Full controls are in `LoreEditor/README.md`
and `LoreEditor/README_RU.md` inside the downloaded Toolkit.

## Development reading in Showstreak

Use an isolated test profile and explicitly set `Showstreak.lore_development = true`
after Showstreak loads. Do not silently enable this flag in a player integration.
Testing chapter purchases can spend that test profile's Dark Stars.

Place a `.sstory` under an active mod's `showstreak/lore/`, or use the game's
development story folder. A direct subfolder with a manifest and resources is
also supported. An optional `index.json` provides the exhaustive package list:

```json
{"schema":1,"packages":["mymod.story.sstory","another-story-folder"]}
```

Paths are relative to `showstreak/lore/`. When an index exists, nearby unlisted
drafts are not loaded automatically. Merge with an existing index instead of
replacing its entries. Preserve package/chapter/page IDs when updating an existing
story, replace its file and avoid parallel copies with the same ID. Required
package IDs and ordering hints are configured in the editor; missing dependencies
and conflicting duplicates produce loader diagnostics. The `showstreak` namespace
and its dotted/underscore/hyphen prefixes are reserved.

## По-русски

[Скачать Toolkit 1.3.0](https://github.com/TraditionalDimension/Showstreak/releases/download/1.3.0/Showstreak-Toolkit-1.3.0.zip)
содержит документацию для авторов, пример интеграции и самостоятельный редактор
комиксов. Для игры нужен отдельный [архив игрока](https://github.com/TraditionalDimension/Showstreak/releases/latest/download/Showstreak-BMM.zip).
Не копируйте Toolkit целиком в `Mods`.

**Лор игрока в 1.3.0 всё ещё в разработке.** Экспорт истории не открывает обычному
игроку каталог. Версия мода 1.3.0, контракт API 1 / ревизия 1.1.0, схема игрового
сохранения 7 и формат истории 2 обозначают разные вещи. PDF остаются базовыми
руководствами API 1.1.0. Актуальные источники: [руководство](MODDING_GUIDE_RU.md),
[справочник](API.md), [дополнения API](API_130.md), [совместимость 1.3.0](RELEASE_130.md)
и [учебный пример](examples/integration/README.md).

Для редактора распакуйте Toolkit отдельно, установите [LÖVE 11.5](https://love2d.org/)
и откройте `LoreEditor/dist/ShowstreakLoreEditor.love`. В Windows также доступен
`LoreEditor/Start-LoreEditor.cmd`. Balatro для создания и предпросмотра не нужна.
Python 3.8+ требуется только для пересборки исходников. Интерфейс **RU / EN-US**
и язык текста истории **ru / en-us** переключаются независимо.

Создайте проект, акты, главы и страницы, добавьте панели и слои либо составные
реплики. Панель — контейнер: её содержимое можно перекрывать, упорядочивать и
переносить между контейнерами. Есть мультивыбор, ручные и автоматические шаги,
звук, переходы и общий с игрой предпросмотр. **Проверить** показывает ошибки и
замечания. Черновик сохраняется с ошибками; экспорт требует успешной проверки.
Обязательный пропавший ресурс блокирует экспорт. Кадрирование создаёт PNG-копию.

**Сохранить / Ctrl+S** обновляет ручную пару `manifest.json` и `editor.json`.
Автосохранение, потеря фокуса, переход к другому проекту, экспорт и закрытие пишут
отдельный черновик восстановления. История документа содержит до 60 команд сеанса,
текстового поля — до 80 состояний. **Версии** хранит именованные контрольные точки
постоянно; их восстановление можно отменить. Для переноса копируйте **всю папку
проекта**, включая ресурсы, `editor.json` и checkpoints. `.sstory` не переносит
историю редактирования и связи составных объектов. Копия проекта сохраняет ID:
для самостоятельной новой истории назначьте новый ID пакета.

**Открыть папку мастерской** показывает фактический каталог. При обычном запуске
Windows это `%APPDATA%/LOVE/ShowstreakLoreEditor`. Редактор не меняет профили игры
и не тратит тёмные звёзды. Полные инструкции вложены в `LoreEditor/README_RU.md`.

`.sstory` — ZIP с `manifest.json` и относительными путями ресурсов, формат 2.
Истории формата 1 импортируются с миграцией. Ограничения: manifest до 4 MiB,
архив до 128 MiB. Произвольный Lua, вложенные игровые панели, вращение, ключевые
кадры transform и библиотека персонажей не поддерживаются. Нативные проверки
проводились в Windows; отдельная приёмка macOS/Linux не заявляется.

Для проверки чтения в игре включите `Showstreak.lore_development = true` после
загрузки Showstreak в отдельном тестовом профиле. Покупка глав может тратить
звёзды этого профиля. Архив размещается в `showstreak/lore/` активного мода.
Необязательный `index.json` из примера выше задаёт полный список пакетов; объединяйте
его с существующим списком. При обновлении сохраняйте ID пакета/глав/страниц и
заменяйте файл, не оставляя дубликаты версий. Не включайте флаг разработки молча
в дополнении для игроков.

[Back to Showstreak](../README.md) · [Known limitations](KNOWN_ISSUES.md) · [License](../LICENSE)
