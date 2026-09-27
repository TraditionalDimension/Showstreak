# ShowstreakExample — local SDK mod / локальный пример

Provider: `ShowstreakExample` · native prefix: `ssex` · version: 1.1.0.

This repository keeps the sample inert. For deliberate local testing alongside
Showstreak 1.3.0, create `Mods/ShowstreakExample`, copy
[ShowstreakExample.json.example](ShowstreakExample.json.example) and
[main.lua.example](main.lua.example) there, and remove the final `.example`
suffix from each file. Keep Showstreak installed separately and restart Balatro.
Use a new test series and a separate profile. The initialization hook supports
both earlier and later loading. The sample references installed game art.

В репозитории пример не исполняется. Для локальной проверки рядом с Showstreak
1.3.0 создайте `Mods/ShowstreakExample`, скопируйте два файла по ссылкам выше и
уберите только последний суффикс `.example`: останутся `ShowstreakExample.json`
и `main.lua`. Перезапустите Balatro и используйте отдельный профиль с новой
тестовой серией. Hook поддерживает загрузку до и после Showstreak.

The sample version/minimum dependency stays 1.1.0 because it teaches the
established API contract. It does not demonstrate every 1.3.0 addition. In
particular, Opening Ticket omits `starting_joker_count`, so its saved occupancy
is unknown even though its callback grants dollars. Select None when trying
Blue Skittles/Red Brain, or author a new versioned category with an explicit zero.
It also omits `bet_tier` and `copy_safe`: its Bet appears under Other and external
consumables are not automatically eligible for Reprise. See [API additions](../../API_130.md).

Минимальная зависимость и версия примера 1.1.0 оставлены для базового контракта.
Пример не объявляет `starting_joker_count`, `bet_tier` и `copy_safe`: вместимость
считается неизвестной, пари попадает в Other, расходники не допускаются к Reprise
автоматически. Для Blueprint/Brainstorm выберите «Нет» или напишите новую
версионированную категорию с явным нулём. Подробности — в [API 1.3.0](../../API_130.md).

[English guide](../../MODDING_GUIDE_EN.md) · [Русское руководство](../../MODDING_GUIDE_RU.md) · [Toolkit](../../TOOLKIT.md)

All IDs below except the native Back have prefix `showstreakexample_`.
Все ID ниже, кроме игровой колоды, начинаются с `showstreakexample_`.

| ID suffix | English behavior | Поведение |
| --- | --- | --- |
| `b_ssex_rehearsal` (full Back key) | Rehearsal Deck: native +$2 | Репетиционная колода: +$2 |
| `spare_hand` | Prop: Use for +1 next-run hand | Предмет: +1 рука после использования |
| `stage_budget` | Voucher: +$2 starts until Intermission | Ваучер: +$2 на стартах до Антракта |
| `short_set` | Contract: -1 hand, +2 stars on win | Контракт: -1 рука, +2 звезды за победу |
| `rehearsal_ticket` | Trick: next run uses supported deck | Трюк: смена следующей колоды |
| `backstage_case` | Pack: three props/tricks, choose one | Пак: три предложения, один выбор |
| `sixth_spotlight` | Voucher: reveal Casting position 6 | Ваучер: открыть шестую карту Кастинга |
| `lucky_tip` | Handler: value × seeded 1–3 dollars | Обработчик: величина × случайные $1–$3 |
| `tip_envelope` | Prop: +$2–$6 at next start | Предмет: от +$2 до +$6 на старте |
| `tip_tax` | Condition: negative Lucky Tip tiers | Условие: отрицательные уровни чаевых |
| `cash_only` | Source: deny `j_credit_card` | Источник: запрет Credit Card |
| `pocket` | Start category: one of five tickets | Стартовая категория: один из пяти билетов |
| `spare_hand_bought` | Purchase reaction, playful/bright | Реплика покупки, playful/bright |

Enable Opening Ticket in Custom rules before starting the series. Its frozen
keys are `copper`, `silver`, `gold`, `velvet`, `encore`, worth +$1 through +$5.
The host offers all five in a seeded order, and the player selects one or None.
This deliberately simple catalog demonstrates the API, not balanced alternatives.

Включите «Стартовый билет» в собственных правилах до старта серии. Сохранённые
ключи: `copper`, `silver`, `gold`, `velvet`, `encore`, бонусы от +$1 до +$5.
Хост предлагает все пять в порядке по сиду, игрок выбирает один или «Нет».
Простой каталог показывает работу API, а не баланс равнозначных альтернатив.

For Lucky Tip, the preview and apply callbacks use identical RNG calls. Once a
native run is saved, resume keeps its dollars rather than repeating the grant.
All vouchers and conditions follow normal Intermission reset. The sample ban
is additive: removing Banner's own Credit Card ban does not remove this source.

В Lucky Tip предпросмотр и применение вызывают одинаковый RNG. Продолжение
забега сохраняет его доллары без повторной выдачи. Ваучеры и условия сбрасываются
обычным Антрактом. Бан складывается с другими: отмена запрета в Banner не отменяет
этот источник.

For learning/local testing under [the existing terms](../../../LICENSE).
Для обучения и локальных проверок по [существующим условиям](../../../LICENSE).
