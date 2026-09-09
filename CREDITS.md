# Showstreak credits and resource sources

## Project

**TraditionalDimension** — Showstreak author, game-mode direction, writing and
art direction, supplied artwork and subsequent sprite edits. Development,
documentation, testing and some visual production were assisted by Codex.

[Author website](https://traditionaldimension.github.io/). This is the author
website, not a designated bug tracker; use the discussion accompanying the build
or its download to report problems.

## Artwork and audio

- `star.png`, `dark-star.png`, `sign.png`, `lamp-state-0/1/2.png` and `icon.png`
  include artwork supplied by TraditionalDimension at the mod's texture scales.
- The Showman artwork was developed from a Showman reference supplied by the
  author, with image-generation assistance and later author edits. The selected
  eight-frame `emotions.png` is the active portrait atlas. Historical generation
  notes do not describe every later edit to the final image.
- Showman is based on Balatro's existing character imagery. Balatro and its
  installed artwork and sounds are credited to their original creators; this
  document does not claim those game resources as original Showstreak work.
- Cards in the mode reuse artwork through installed Balatro/Steamodded center
  keys. Original game card atlases are not bundled with this mod.
- `purchase.wav` and `ready.wav` are short synthesized feedback cues created for
  Showstreak. They remain registered; current purchase and item reactions use
  native game sound keys.
- Showman's voice uses the installed game's `voice1.ogg`, `voice4.ogg`,
  `voice7.ogg` and `voice10.ogg`. The mod prepares its variants in memory at sound
  registration. Neither original nor processed Balatro voice files are included
  in the player archive. An unmodified native voice is the fallback if preparation
  fails. The earlier development workflow that wrote voice WAV files to the mod
  folder is no longer used for distribution.
- Legacy `host.png`, `showman-aura.png`, `host_face.fs` and `marquee.fs` remain in
  the asset tree but are not used by the current portrait/sign renderer.

## Runtime and language support

Showstreak uses **Balatro**, **Lovely** and **Steamodded**. They are separately
installed dependencies and are not included in the player archive. Their own
projects retain their respective authorship and conditions.

The UI and Showman dialogue include 15 locales. Translation coverage and
formatting checks are separate from native-speaker editorial review; no external
translator or reviewer is credited without an established contribution.

## Conditions of use

See [LICENSE](LICENSE) for the author's conditions governing Showstreak code and
author-owned resources. It permits use in the game and does not grant permission
to republish or include Showstreak files in another distribution. Authors may
write their own separate integrations using the public API without redistributing
Showstreak itself. Attribution here does not replace a permission grant for
third-party materials.

## По-русски

**TraditionalDimension** — автор Showstreak: концепция режима, тексты,
художественное направление, предоставленные рисунки и дальнейшая правка спрайтов.
Codex помогал с реализацией, документами, тестированием и частью подготовки графики.

Текущий Шоумен создан по предоставленному автором референсу с помощью генерации
изображений и последующей авторской доработки. Его образ опирается на персонажа
Balatro; игровые рисунки и звуки не объявляются оригинальными ресурсами Showstreak.
Карточки обращаются к установленным игровым атласам по ключам; сами атласы Balatro
в мод не входят. Звуки покупки и подготовки синтезированы для Showstreak.

Голос Шоумена использует `voice1.ogg`, `voice4.ogg`, `voice7.ogg` и `voice10.ogg`
из установленной Balatro. Обработка выполняется в памяти при регистрации звуков;
исходные и обработанные голосовые файлы в архив для игроков не входят. При ошибке
подготовки используется штатный голос. Прежний разработческий процесс с записью
обработанных WAV в папку мода больше не используется для распространения.

Условия использования кода и авторских ресурсов указаны в [LICENSE](LICENSE).
Они разрешают использование в игре, но не перепубликацию или включение файлов
Showstreak в чужой дистрибутив. Собственные отдельные интеграции через публичный
API создавать можно, не распространяя сам Showstreak. Авторство и условия
сторонних компонентов остаются у их создателей.
