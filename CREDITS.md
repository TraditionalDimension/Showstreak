# Showstreak credits and resource sources

## Project

**TraditionalDimension** — Showstreak author, game-mode direction, writing, art direction, supplied artwork and subsequent sprite edits. Development, documentation, testing and some visual production were assisted by Codex.

[Author website](https://traditionaldimension.github.io/) · [GitHub Issues](https://github.com/TraditionalDimension/Showstreak/issues)

This reference reflects the 1.3.0 release. Asset paths below refer to the installed player distribution or the separate Toolkit.

## Artwork and audio

- `star.png`, `dark-star.png`, `sign.png`, `lamp-state-0/1/2.png` and `icon.png` include artwork supplied by TraditionalDimension at the mod's texture scales.
- The initial Showman artwork was developed from a reference supplied by the author, with image-generation assistance and later author edits. After an earlier expansion with code-based pixel edits, TraditionalDimension redrew and arranged the current **32 populated drawings** in `emotions.png`. The original eight frames are preserved. Runtime mappings were updated without modifying those author-supplied PNGs.
- Showman is based on Balatro's existing character imagery. Balatro and its installed artwork and sounds are credited to their original creators; this document does not claim those game resources as original Showstreak work.
- Cards reference artwork through installed Balatro/Steamodded center keys. Original game card atlases are not bundled with Showstreak.
- `purchase.wav` and `ready.wav` are short synthesized feedback cues created for Showstreak. They remain registered; current purchase and item reactions use native game sound keys.
- Showman's voice uses the installed game's `voice1.ogg`, `voice2.ogg`, `voice4.ogg`, `voice5.ogg`, `voice7.ogg`, `voice9.ogg` and `voice10.ogg`. The mod prepares its variants in memory during sound registration. Neither original nor processed Balatro voice files are included in the player archive. The native voice is the fallback if preparation fails.
- Legacy `host.png`, `showman-aura.png`, `host_face.fs` and `marquee.fs` remain in the asset tree but are not used by the current portrait/sign renderer.

The [artwork and sound layout](docs/ASSET_LAYOUT.md) describes the current atlas and runtime use. Historical generation notes do not describe every later author edit.

## Runtime and language support

Showstreak uses **Balatro**, **Lovely** and **Steamodded**. They are separately installed dependencies and are not included in the player archive. Their projects retain their respective authorship and conditions.

The UI and Showman dialogue include 15 locales. Translation coverage and formatting checks are separate from native-speaker editorial review; no external translator or reviewer is credited without an established contribution.

The separate Lore Workshop runs through **LÖVE 11.5**. See the [Toolkit overview](docs/TOOLKIT.md) for its documentation and dependencies.

## Comic typography

Comic rendering uses unmodified **DejaVu Sans**. Lore Workshop also uses **DejaVu Sans Bold** for its interface. Copyright and permission notices are distributed with the font: in the player mod, see `assets/lore/LICENSE-DejaVu.txt`; keep the corresponding notices with the Toolkit's fonts as well. The font is distributed under its own license.

## Conditions of use

See [LICENSE](LICENSE) for the author's conditions governing Showstreak code and author-owned resources. It permits use in the game and does not grant general permission to republish or include Showstreak files in another distribution. Authors may write their own separate integrations using the public API without redistributing Showstreak itself.

The [Balatro Mod Manager permission](BMM_PERMISSION.md) is a limited exception for unmodified official releases. Attribution does not replace a permission grant for third-party materials.

## По-русски

**TraditionalDimension** — автор Showstreak: концепция режима, тексты, художественное направление, предоставленные рисунки и дальнейшая правка спрайтов. Codex помогал с реализацией, документами, тестированием и частью подготовки графики.

Исходный Шоумен создан по предоставленному автором референсу с помощью генерации изображений и последующей авторской доработки. После расширения правками пикселей в коде TraditionalDimension перерисовал и расположил текущие **32 рисунка**. Привязки обновлены без изменения предоставленных автором PNG; исходные восемь кадров сохранены.

Образ опирается на персонажа Balatro; игровые рисунки и звуки не объявляются оригинальными ресурсами Showstreak. Карточки обращаются к установленным игровым атласам по ключам; сами атласы Balatro в мод не входят. Звуки покупки и подготовки синтезированы для Showstreak.

Голос Шоумена использует `voice1.ogg`, `voice2.ogg`, `voice4.ogg`, `voice5.ogg`, `voice7.ogg`, `voice9.ogg` и `voice10.ogg` из установленной Balatro. Обработка выполняется в памяти при регистрации звуков; исходные и обработанные голосовые файлы в архив для игроков не входят. При ошибке подготовки используется штатный голос.

Для комиксов используется неизменённый DejaVu Sans, а для интерфейса Lore Workshop — также DejaVu Sans Bold. Уведомления об авторстве и разрешениях поставляются вместе со шрифтами; в игровом моде это `assets/lore/LICENSE-DejaVu.txt`. У шрифта собственная лицензия.

Условия использования кода и авторских ресурсов указаны в [LICENSE](LICENSE). Они разрешают использование в игре, но не дают общего разрешения на перепубликацию или включение файлов Showstreak в чужой дистрибутив. Собственные отдельные интеграции через публичный API можно распространять без самого Showstreak. Для Balatro Mod Manager действует [ограниченное разрешение](BMM_PERMISSION.md). Авторство и условия сторонних компонентов остаются у их создателей.
