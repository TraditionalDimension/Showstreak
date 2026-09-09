# Showstreak 1.0.0 - limits and validation / ограничения и проверка

**Showstreak 1.0.0: first public release on [Nexus Mods](https://www.nexusmods.com/balatro/mods/947).** The checks below describe the tested 1.0.0 build; publication does not expand their scope.

## Completed checks

The player candidate was installed from its ZIP into a separate Mods directory
with **Windows, Balatro 1.0.1o-FULL, Lovely 0.9.0 and Steamodded 26.829.0**.
A separate save identity kept the native acceptance run away from personal data.
The test driver used actual game objects, UI callbacks and save workers. Some
wins, losses, mask and Intermission states were prepared programmatically; this
was integration acceptance, not a claim of completing ordinary player campaigns.

- 27 Lua specification suites pass, including 179 recovery and 32 runtime-audio
  checks. All seven manifest patches compile against the installed game; the
  save-worker probe and queued save routing are covered separately.
- The native game loaded the unpacked candidate and acknowledged critical save
  patches. The shop, purchase/Use, preparation, start and a full process restart
  passed. The saved run resumed without applying its resources twice.
- Native win/loss/abandon, mask acknowledgement and Intermission accept/decline
  passed. A simulated campaign-write failure showed one recovery window; the
  native Retry button saved the original loss without duplicating history.
- Both campaign slots were deliberately corrupted only in the isolated profile.
  The game showed a save-error window without overwriting them, then loaded the
  restored files through Retry.
- The ordinary run save remained byte-identical during Showstreak operations and
  was subsequently resumed. In-memory voice registration, separate voice/effect
  preferences, the frame 7 -> 6 return and reduced-motion idle were exercised.
- Both 1x and 2x portrait assets were actually reloaded and checked. A separate
  smoke test with Pantomime Paradox 1.16.0 passed shop/start/save/resume and
  ordinary-save isolation in its default configuration; extra PP modes were not
  enabled.
- Native EN/RU/zh_CN UI was checked at 1280x720. All 15 locales have automated
  coverage; older native checks covered the full locale set.
- A separate LÖVE check confirmed that all four voices generated in memory are
  byte-identical to the previous processed WAV files. No voice WAV is shipped.
- The API guide and starter passed 12 example scenarios using the real API/core
  under LuaJIT. PDF pages were rendered and visually checked.

## Intentional limits and remaining manual checks

- **Lore is future content.** Dark Stars accumulate per profile; this version has
  no Lore purchases, unlockable entries or story chapters.
- **Campaign and run recovery differ.** A/B protects campaign progress; the run
  has one showstreak-run.jkr. Back up all three files together. No recovery of
  every missing/corrupted run snapshot is promised.
- **Existing series keep their rules/catalog.** New content or Intermission
  policies do not silently enter old saves. Missing required providers may block
  continuation. Modded decks require adapters.
- **Showman uses eight frames.** There are no separate mouth drawings for every
  expression. Reactions return to idle and voice can be disabled separately.
- Physical controller use, comfortable sound levels, smaller windows and normal
  play on Easy/Standard/Hard still need author/player evaluation. Tests do not
  measure balance, pacing or native-speaker quality of all translations.
- Linux, macOS, Steam Deck, arbitrary mod collections and every future loader
  version are not certified by these Windows checks. A smoke test of one mod
  combination cannot guarantee all of its gameplay hooks.

Report problems in the discussion accompanying the build/download. The
[README](PLAYER_GUIDE_EN.md) lists useful diagnostics and backup files. Do not delete a
failing save as the first troubleshooting step.

## По-русски

**Showstreak 1.0.0: первая публичная версия на [Nexus Mods](https://www.nexusmods.com/balatro/mods/947).** Проверки ниже относятся к испытанной сборке 1.0.0; публикация не расширяет область проверки.

Архив распакован в отдельную папку модов и проверен в настоящей игре на
**Windows, Balatro 1.0.1o-FULL, Lovely 0.9.0, Steamodded 26.829.0**. Отдельная
папка сохранений изолировала проверку от личного прогресса. Использовались
настоящие объекты игры, экранные кнопки и поток записи. Некоторые состояния
победы, поражения, масок и Антракта задавались программно: это проверка
интеграции, а не прохождение обычных серий игроком.

Пройдены 27 наборов Lua-тестов, включая 179 проверок восстановления и 32 проверки
звука, а также проверка семи патчей и протокола потока записи. В самой игре
проверены магазин, покупка/Use, подготовка, запуск, полный перезапуск процесса
и продолжение без повторных бонусов; победа, поражение, прерывание, маски и
принятие/отказ от Антракта.

При искусственном отказе записи поражения показано одно окно; кнопка «Повторить»
сохранила исходную причину без повторной истории. После порчи обоих A/B-файлов
только в тестовом профиле открылась ошибка без перезаписи данных; восстановленные
файлы удалось прочитать повтором. Обычное сохранение не менялось от действий
Showstreak и затем было продолжено.

Проверены регистрация голоса в памяти, раздельные настройки голоса/эффектов,
переход 7 -> 6 и idle при уменьшении движения, интерфейс EN/RU/zh_CN при 1280x720.
Для всех 15 локалей есть автопроверки и исторический полный просмотр. Отдельный
LÖVE-прогон подтвердил побайтовое совпадение голосов в памяти с прежними WAV.
Голосовые WAV в архив не включены. Текстуры 1x/2x действительно перезагружены
и проверены. Отдельный короткий прогон с Pantomime Paradox 1.16.0 подтвердил
магазин, старт, сохранение/продолжение и изоляцию обычного сохранения при
стандартных настройках; дополнительные режимы PP не включались.
Примеры API прошли 12 сценариев; PDF просмотрен
после отрисовки страниц.

Остаются осознанные ограничения: лор пока без покупок и глав; A/B не заменяет
резервную копию run-файла; старые серии сохраняют прежний каталог; модовым колодам
нужны адаптеры; Шоумен использует восемь кадров. Нужны обычные игры на трёх
пресетах для оценки темпа, прослушивание автором, реальный геймпад, меньшие окна
и редактура переводов носителями. Linux/macOS/Steam Deck, произвольные наборы
модов и будущие загрузчики не объявляются проверенными.

О проблемах сообщайте в обсуждении сборки/скачивания. Нужные сведения и файлы
резервной копии перечислены в [русской инструкции](PLAYER_GUIDE_RU.md).
