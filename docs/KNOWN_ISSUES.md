# Showstreak 1.3.0 — compatibility and limitations

[English player guide](PLAYER_GUIDE_EN.md) · [Русская инструкция](PLAYER_GUIDE_RU.md) · [Toolkit](TOOLKIT.md) · [Changelog](../CHANGELOG.md)

The tested game baseline is **Windows, Balatro 1.0.1o-FULL, Lovely 0.9.0 and Steamodded 26.829.0**. Focused checks and isolated Windows game scenarios cover saves, transactions, preparations, boss objectives, localized layouts and authoring workflows. Controlled scenarios, including forced outcomes, do not establish complete ordinary-campaign coverage or settled balance.

## Player-facing limitations

- **Lore remains in development.** The normal Lore screen retains the profile's Dark Stars but offers no player story catalog. The separate Lore Workshop is an authoring tool.
- **Back up the campaign and current run together.** Each profile uses `showstreak-a.jkr`, `showstreak-b.jkr` and `showstreak-run.jkr`. The campaign pair alone cannot reconstruct a missing current run. Settings and personal presets are separate.
- **Required save updates need confirmation.** Showstreak verifies a backup first. A failed backup or changed source blocks the operation. Deferring leaves the source untouched but prevents continuation until a required update is completed. Restore is in **Mods → Showstreak → Saves** and first creates a safety backup of the current state. Returning to an older mod requires its matching backup; reverse migration is not provided.
- **Adding new content is a separate choice.** Existing series retain rules, offers and definitions except for explicitly previewed compatibility corrections. **Rules → Add new content** can add compatible missing entries between runs. Installing an update alone does not give an older series the entire new catalog.
- **Keep required mods installed.** A missing provider can block continuation. An integration badge does not certify all of another mod's behavior. Undeclared boss schedules, action restrictions and starting grants may need an adapter.
- **Some starting combinations are unavailable.** A selected starting addition that lacks information about its starting Jokers cannot safely combine with Blue Skittles or Red Brain. Choosing no starting addition remains available. Old saves do not silently acquire missing information from a later registration.
- **Boss-objective Bets have specific restrictions.** Only one boss schedule can be accepted, including On a Needle. Assigned ordinary bosses appear one Ante before the target and must meet their own minimum Ante. External schedules or boss bans may make a Bet unavailable. Completing a task earns its bonus only when the whole run is also won.
- **Translations and balance still benefit from player feedback.** Fifteen locales have coverage and formatting checks, plus targeted layout checks. This does not mean every line has been edited by a native speaker. Long-term campaign statistics have not established the balance of every new price, chance and reward.
- **Platform testing is Windows-focused.** Native macOS, Linux and Steam Deck play, every newer loader release, all mod combinations, every small-window layout and physical-controller play have not been comprehensively validated.
- **Voice and motion are adjustable.** Voice, effect sounds and reduced motion have separate settings. Matching mouth animation exists for only part of Showman's expression set; other reactions retain their authored drawings while speaking.

The 1.2.0 release corrected The Needle interaction reported in [issue #1](https://github.com/TraditionalDimension/Showstreak/issues/1), along with related boss-resource handling. If a similar problem occurs in 1.3.0, report the exact versions, accepted Bets and other mods instead of assuming the earlier cause.

## Authoring tools

The [Toolkit](TOOLKIT.md) is separate from the player installation. Lore Workshop uses **LÖVE 11.5** and supports independent Russian/English interface and story-text selection. Its portable `.love` build can run without Balatro.

Native Windows authoring and reader checks have been performed; other platforms have not received equivalent native acceptance. The editor does not support arbitrary Lua stories, nested runtime panels, native rotation/crop/filter fields, transform keyframes, a character library or marquee selection.

Export remains **story schema 2**. Document history and authoring groups remain in project metadata, so keep the entire project folder to retain checkpoints and compound-object links. In-game development reading requires the explicit `Showstreak.lore_development = true` flag; it is not a released player story catalog.

## Reporting a problem

Use [GitHub Issues](https://github.com/TraditionalDimension/Showstreak/issues). Include the Showstreak, Balatro, Lovely and Steamodded versions, operating system, installed mods, steps to reproduce and relevant error text or logs. Keep an unchanged copy of a reproducibly failing save. Installation and backup paths are in the player guides.

## По-русски

Основная проверенная среда — **Windows, Balatro 1.0.1o-FULL, Lovely 0.9.0 и Steamodded 26.829.0**. Изолированные игровые сценарии проверяют конкретные действия, но не заменяют обычные прохождения и длительную проверку баланса.

- **Лор пока в разработке.** Тёмные звёзды сохраняются, но обычный раздел ещё не предлагает каталог историй. Редактор предназначен для авторов.
- **Сохраняйте вместе три файла профиля:** `showstreak-a.jkr`, `showstreak-b.jkr` и `showstreak-run.jkr`. Пара файлов серии не восстановит отсутствующий текущий забег. Настройки и личные наборы правил находятся отдельно.
- **Перенос сохранения требует подтверждения и проверенной копии.** Отложенный перенос не меняет исходники, но нуждающуюся в нём серию пока нельзя продолжить. Восстановление находится во вкладке «Сохранения» конфига и сначала защищает текущее состояние копией. Обратного переноса формата нет; для старой версии нужна её старая копия.
- **Новый контент добавляется отдельно между забегами.** Правила, предложения и определения старой серии сохраняются, кроме явно показанных исправлений совместимости. Установка обновления сама по себе не расширяет её каталог.
- **Сохраняйте нужные серии моды.** Отметка об интеграции не гарантирует совместимость всех функций. Неподдержанные изменения боссов, запреты действий и стартовые предметы могут требовать адаптера.
- **Не все сочетания начальных предметов доступны.** Дополнение без сведений о начальных джокерах нельзя безопасно совместить с Blue Skittles или Red Brain. Можно выбрать вариант без начального дополнения. Отсутствующие сведения старой серии не подменяются молча новой регистрацией.
- **Одновременно принимается одна программа боссов**, включая On a Needle. Для бонуса нужно выполнить задание и выиграть весь забег. Конфликтующие расписания или запреты боссов могут сделать пари недоступным.
- **Переводы и баланс нуждаются в обратной связи.** Полнота 15 локалей не равна проверке каждой строки носителем языка. Автоматические сценарии не устанавливают баланс цен, шансов и наград.
- **Область проверки в основном ограничена Windows.** macOS, Linux, Steam Deck, все будущие загрузчики, сочетания модов, размеры окна и физический контроллер не прошли всеобъемлющую проверку.
- **Голос и анимация настраиваются отдельно.** Не у всех выражений Шоумена есть совпадающие кадры рта; такие рисунки сохраняются во время речи.

В 1.2.0 исправлена описанная в [issue #1](https://github.com/TraditionalDimension/Showstreak/issues/1) проблема The Needle и связанные взаимодействия ресурсов боссов. При похожем сбое в 1.3.0 укажите точные версии, принятые пари и остальные моды.

Редактор запускается отдельно через **LÖVE 11.5**. Языки интерфейса и истории выбираются независимо. Для сохранения версий и групп переносите целиком папку проекта. Экспорт использует story schema 2; история редактирования в `.sstory` не входит. Произвольные Lua-истории, вложенные игровые панели, поля поворота/обрезки/фильтров, ключевые кадры преобразований, библиотека персонажей и выделение рамкой не поддерживаются. Чтение историй в игре доступно в режиме разработки и не означает выпуск игрового каталога.

Сообщайте об ошибках через [GitHub Issues](https://github.com/TraditionalDimension/Showstreak/issues), приложив версии, шаги, список модов и текст ошибки. Установка и резервные копии описаны в [русской инструкции](PLAYER_GUIDE_RU.md).
