# Showstreak: A Guide for Integration Authors

**Showstreak 1.0.0 · API v1 · TraditionalDimension**

A practical guide to building your own provider mod: items, decks, localization, Showman dialogue, saves, and releases. The contracts have been checked against the 1.0.0 implementation. The companion [API reference](API.md) and [tutorial example](examples/integration/README.md) are available in the source/developer materials, distributed separately from the lean player ZIP. You can follow the examples in this guide without those companion files.

This guide covers integration through the API. Distribute your own mod separately and declare Showstreak as a dependency. Do not include Showstreak runtime files, original artwork, audio, or player saves in your integration archive. The [LICENSE](../LICENSE) governs permitted use; providing an API does not grant permission to redistribute the entire mod or its resources.

## 1. What You Can Extend

The API has three registration functions: `register_item`, `register_deck`, and `register_showman_reaction`. They add item data to Showstreak's catalog, describe the resources of an existing deck, and add contextual dialogue, respectively. `showman_event` reports your own public event. For diagnostics, use `provider_manifest`, `mod_report`, and `environment_report`.

| Task | Supported mechanism |
|---|---|
| Prepare an item for the next run | `kind='prop'` with one supported effect |
| Immediately change a selection or condition | `kind='trick'` |
| Add a voucher that remains active between runs | `kind='voucher'` |
| Offer a contract with a victory reward | `kind='contract'` |
| Offer a selection of items in a pack | `kind='pack'` |
| Add your own deck | Register a normal Back first, then call `register_deck` |
| React to a purchase or your own event | `register_showman_reaction` |

The API does not register new effect types, arbitrary gameplay callbacks, condition/mask families, new shops, custom pack pools, paid contracts, save migrations, or settings. Rule presets are imported as JSON through the menu; there is no `register_preset` function. Adding an invented field does not implement a new behavior.

Some Lua functions exposed on `Showstreak` serve the mod's own implementation. A function's presence in that table does not make it a stable public contract. Use the entry points listed here for integrations; do not modify internal `core`, `store`, UI tables, or the saved catalog in production gameplay code. In this guide, a **campaign** is a Showstreak series spanning multiple individual Balatro **runs**.

<!-- pagebreak -->

## 2. Load Order and Identifiers

Register content once, while your mod is loading: **after Showstreak and before `SMODS.booted`**. After boot, the registration functions return `nil, 'registration_closed'`. Do not register content in update handlers, button callbacks, purchase events, or at the start of every run.

Showstreak uses priority `10000010`; Steamodded loads lower priorities first. A separate provider can use `10000011` and depend on version 1.0.0. Showstreak itself requires Steamodded 26.829.0+ and Lovely 0.9+; do not bundle these loaders with your integration.

For a local tutorial installation, create `Mods/ShowstreakExample/` and place this manifest inside it:

```json
{
  "id": "ShowstreakExample",
  "name": "Showstreak Integration Example",
  "author": ["Your name"],
  "description": "A local Showstreak API tutorial.",
  "prefix": "sse",
  "main_file": "main.lua",
  "version": "1.0.0",
  "priority": 10000011,
  "dependencies": ["Showstreak (>=1.0.0)"]
}
```

Name the file `ShowstreakExample.json`. The `main.lua` that goes beside it is shown in the next section. Ready-to-use local versions of both files are also in `examples/integration` in the source/developer materials; remove their `.example` suffixes for the tutorial installation. Inside the original Showstreak folder, those example files remain inactive.

**Keep these three naming systems separate:**

| Name | Example | Purpose |
|---|---|---|
| Mod ID | `ShowstreakExample` | Provider identity, dependency, and the start of custom event names |
| Steamodded prefix | `sse` | Ordinary SMODS centers/atlases, such as `b_sse_reserve` |
| Showstreak namespace | `showstreakexample_` | Item, family, and reaction IDs |

The namespace is derived from the provider ID: convert it to lowercase, replace characters other than letters, digits, and underscores with `_`, then append `_`. Full item and reaction IDs must match `^[a-z][a-z0-9_]*$`. `Other-Mod` and `Other_Mod` produce the same prefix; item namespace collisions are rejected. Choose your own stable ID for a published mod.

You can usually omit `provider`: the registration function uses `SMODS.current_mod.id`. Set an explicit provider only when you deliberately need to register on behalf of another available mod. Do not claim another author's namespace.

<!-- pagebreak -->

## 3. A Working Minimal main.lua

This version uses native artwork, so it needs no additional PNGs or sounds. Put it in the separate tutorial mod from section 2. It is an alternative minimal file; the ready-made example on disk also includes a voucher.

```lua
local S = assert(Showstreak, 'Showstreak must load first')
assert(S.api and S.api.version == 1, 'Expected API v1')

local id, reason = S.register_item{
    id = 'showstreakexample_spare_hand',
    kind = 'prop', effect = 'hands', value = 1,
    price = 2, art = 'j_joker', content_version = 1,
    loc_txt = {
        ['en-us'] = {
            name = 'Spare Hand',
            text = {'{C:blue}+#1#{} hand each round',
                    'during the next run'},
        },
        ru = {
            name = 'Запасная рука',
            text = {'{C:blue}+#1#{} рука каждый раунд',
                    'в следующем забеге'},
        },
    },
}
assert(id, tostring(reason))

local reaction, why = S.register_showman_reaction{
    id = 'showstreakexample_spare_hand_bought',
    event = 'buy',
    match = {item_id = id, source = 'buy'},
    priority = 60,
    text = {
        ['en-us'] = {'An extra hand. A useful precaution.'},
        ru = {'Лишняя рука. Полезная предосторожность.'},
    },
}
assert(reaction, tostring(why))
```

After fully restarting the game, the provider should appear as integrated in the mod report. **Create a new campaign:** an existing campaign retains its previous catalog. Offers are random, so an item being absent from the first shop is not, by itself, evidence of a problem.

The expected sequence is to buy the item for 2 Stars, find it in your held inventory, activate it with Use, and start the next run. Buying it alone does not grant an extra hand. After Use, the bonus applies in every round of the next run; resuming a save must not apply it a second time.

If registration fails, stop loading the tutorial mod with a clear diagnostic. A finished integration may handle the failure differently, but it must not silently treat a failed registration as successful.

<!-- pagebreak -->

## 4. The Item Contract

`Showstreak.register_item(def)` returns the ID on success, or `nil, reason` on failure. The definition is copied. It must contain plain data: strings, booleans, finite numbers, and tables without metatables or cycles. Functions, userdata, NaN, and infinities are rejected even inside an additional nested table.

| Field | Requirement |
|---|---|
| `id` | Unique ID in the provider's namespace |
| `provider` | Optional ID; defaults to the current mod |
| `kind`, `effect`, `value` | A required, compatible combination from section 5 |
| `price` | Integer 0..999 Stars; contracts require 0 |
| `art` | A known center key, such as `j_joker` or `v_seed_money` |
| `atlas`, `pos` | Alternative to art: a known atlas and integer x/y coordinates in 0..9999 |
| `loc_txt` | Steamodded description; preferably complete EN and RU text |
| `weight` | Optional integer 1..100; defaults to 1 |
| `family` | Optional group ID in the same namespace |
| `family_weight` | Integer 1..100; identical for all family members; defaults to 1 |
| `requires` | Voucher only; ID of an already registered parent voucher |
| `reward` | Required for a contract, 0..999; also allowed for trick/ante |
| `max_once` | Optional boolean, only for trick/ante |
| `deck` | Only for set_deck; an existing supported deck |
| `pool`, `choose` | Pack only; pool props/tricks, choose from 1 to value |
| `content_version` | Integer 1..999999; defaults to 1; metadata |

If both `atlas` and `art` are supplied, the explicit atlas is used. Coordinates refer to cells, not pixels. Registration does not guarantee that an incorrectly sized PNG will be detected.

The API assigns `activation`, `lifetime`, and `visual` based on kind. Do not set these fields as a way to change an item's semantics. Item registration does not apply a strict schema to every unknown key: a plain extra field may be copied, but it does not create any new behavior.

If native center registration fails, the item is not added to the catalog and the function returns `center`; details are available in `Showstreak.api.last_error`. Fix the original definition instead of manually appending entries to `Showstreak.content.items` or `order`.

<!-- pagebreak -->

## 5. Effects and Lifetimes

There are nine resource effects: `money`, `hands`, `discards`, `hand_size`, `joker_slots`, `consumable_slots`, `deck_size`, `shop_slots`, and `booster_slots`. Their values are integers from -999 to 999. The value is a change to the resource, not its final absolute amount.

The `blind`, `boss`, and `prices` effects accept finite fractional changes. `0.15` means +15%; `-0.20` means -20%. Contributions to the same modifier add together: +0.15 and -0.05 produce +0.10. The campaign's score factor is applied separately. The validator does not judge balance or promise sensible results for extreme percentages.

| Kind | Effects | Behavior |
|---|---|---|
| `prop` | Resources, blind/boss/prices | Buy/Take adds it to held inventory; Use prepares its effect for the next run |
| `contract` | Resources, blind/boss/prices | Accepting it for free immediately prepares the effect; victory pays reward |
| `voucher` | All of the above, plus reward, held_slots, reveal_act, free_reroll | Buying activates it until an accepted Intermission or the end of the campaign |
| `trick` | reveal, reveal_all, deck, set_deck, stake, ante, remask, remove_condition | Buy/Take adds it to held inventory; Use performs the action immediately |
| `pack` | Only pack | Buying opens a selection; chosen items enter held inventory |

A fixed `value=1` is required for reveal, reveal_all, reveal_act, deck, set_deck, remask, and remove_condition. Stake requires `-1`. Ante requires a nonzero integer from -15 to 15. The voucher effects reward, held_slots, and free_reroll require a positive integer up to 999. For packs, value is 1..5 and choose cannot exceed value.

`deck` selects another deck according to the rules; `set_deck` assigns a specific one. The latter is eligible only if its target deck is enabled for the campaign and differs from the deck already selected for the next run. It preserves that run's seed and does not draw an additional deck from the deck bag.

A contract must be free to accept. Its `reward` means Stars awarded for victory, not items, dollars, or Dark Stars. For a voucher with `effect='reward'`, set the bonus in value, not in the reward field.

Accepting Intermission clears vouchers, held items, accumulated conditions, and preparation; it preserves the overall win streak, Dark Stars, frozen rules, and catalog. Describing a voucher as lasting "forever" would therefore be inaccurate.

<!-- pagebreak -->

## 6. More Item Patterns

You can append the following block to the `main.lua` from section 3. It uses the same provider with separate IDs. The parent voucher is registered before its upgrade, and every item explicitly declares its price.

```lua
local S = assert(Showstreak)
local function add(def)
    local id, why = S.register_item(def)
    assert(id, tostring(why))
end
add{
    id = 'showstreakexample_budget', kind = 'voucher',
    effect = 'money', value = 2, price = 3,
    art = 'v_seed_money',
    loc_txt = {name = 'Budget', text = {'Start with +$#1#'}},
}
add{
    id = 'showstreakexample_budget_plus', kind = 'voucher',
    effect = 'money', value = 3, price = 5,
    requires = 'showstreakexample_budget', art = 'v_seed_money',
    loc_txt = {name = 'More Budget', text = {'Start with +$#1#'}},
}
add{
    id = 'showstreakexample_risk', kind = 'contract',
    effect = 'blind', value = 0.15, price = 0, reward = 4,
    art = 'j_joker',
    loc_txt = {name = 'Risk', text = {'Higher blinds',
                                    'Win to earn #2# Stars'}},
}
add{
    id = 'showstreakexample_low_stake', kind = 'trick',
    effect = 'stake', value = -1, price = 3, art = 'c_world',
    family = 'showstreakexample_tickets', family_weight = 1,
    loc_txt = {name = 'Lower Stake', text = {'Lower next stake'}},
}
add{
    id = 'showstreakexample_case', kind = 'pack', effect = 'pack',
    value = 3, choose = 1, pool = 'props', price = 3,
    art = 'p_arcana_normal_1',
    loc_txt = {name = 'Case', text = {'Choose #2# of #1# items'}},
}
```

The parent and upgrade vouchers stack: buying both produces a +5 money bonus. `requires` controls the upgrade's eligibility; it does not declare that the upgrade automatically replaces its parent.

A family receives one weight as a group, and member weights are then applied within that group. Ten tickets in one family should not occupy ten times as much of the shop's offer pool. All members must share the same family_weight. An item's ordinary weight and the family's family_weight serve different purposes.

The `props` pool contains props and tricks; `tricks` contains only tricks. The API does not accept an array of your own IDs in place of pool. An item ID is not repeated within a single opened pack. Eligibility is checked before selection.

<!-- pagebreak -->

## 7. Localization and Custom Artwork

The generated center belongs to the `Showstreak` description set and uses the key `sstreak_item_<full ID>`. For the tutorial item, that is `sstreak_item_showstreakexample_spare_hand`. You can provide languages directly in loc_txt, as in section 3, or add them through your mod's ordinary localization files. English in loc_txt itself is useful when an old item must be restored from a save.

Description variables are `#1#` for value, `#2#` for reward or the number of choices allowed by a pack, and `#3#` for the value plus the sum of its prerequisite chain. For blind/boss/prices, `#1#` contains localized words for an exact ratio, such as "one fifth", rather than automatically supplying a numeric percentage. Write the surrounding sentence to fit that substitution. Balatro formatting such as `{C:blue}` is allowed in loc_txt; those braces are forbidden in Showman dialogue.

To use your own PNG, register the atlas before the item. This additional example **requires files you supply** at `assets/1x/my_cards.png` and `assets/2x/my_cards.png` in the provider mod. The cell sizes are 71x95 and 142x190 respectively; pos below selects the first cell. The basic ready-made example needs no PNGs.

```lua
local S = assert(Showstreak)
SMODS.Atlas{
    key = 'showstreakexample_cards',
    prefix_config = {key = false},
    path = 'my_cards.png', px = 71, py = 95,
}
local id, why = S.register_item{
    id = 'showstreakexample_custom_art',
    kind = 'prop', effect = 'hands', value = 1, price = 2,
    atlas = 'showstreakexample_cards', pos = {x = 0, y = 0},
    loc_txt = {name = 'Custom Prop', text = {'+#1# hand'}},
}
assert(id, tostring(why))
```

The unique full atlas key is explicitly selected through prefix_config; the same key is passed to register_item. If you use Steamodded's standard prefixing, use the actual registered key instead of guessing it from the PNG filename.

`art` reuses only an existing center's static atlas/pos. It does not inherit that center's animation, shaders, or gameplay calculate function. Registration before injection into G.P_CENTERS is allowed if the center is already in SMODS.Centers. If a texture is missing after an update, a restored item may use native artwork while retaining its saved effect.

Check both texture scales, long translations, tooltips, and language changes. Text color, artwork, and audio feedback must not suggest an incorrect item lifetime.

<!-- pagebreak -->

## 8. Adapting an Existing Deck

An adapter **does not create a Back or execute its effect**. It tells Showstreak the deck's resource baseline for calculations and eligibility filtering. Register an ordinary deck with Steamodded first, then register its adapter. This tutorial deck grants +2 hands and uses the native centers atlas, so it needs no new PNG.

```lua
local S = assert(Showstreak)
SMODS.Back{
    key = 'reserve',
    pos = {x = 0, y = 0},
    config = {hands = 2},
    unlocked = true, discovered = true,
    loc_txt = {name = 'Reserve Deck',
               text = {'Start with {C:blue}+2{} hands'}},
}
local key, why = S.register_deck{
    key = 'b_sse_reserve', version = 1,
    baseline = {
        money = 4, hands = 6, discards = 3, hand_size = 8,
        joker_slots = 5, consumable_slots = 2, deck_size = 52,
        shop_slots = 2, booster_slots = 2,
    },
}
assert(key, tostring(why))
```

This block assumes the manifest prefix `sse`. Change the adapter key if your prefix differs. Do not use the same Back key in two mods. Add the tutorial deck when creating a **new** campaign; registering an adapter does not add the deck to a previously saved selection.

Baseline must contain **exactly the nine resources** from section 5, without ante. Money must be an integer from -999 to 999; the other resources must be integers from 0 to 999. A missing, additional, or incorrectly typed resource causes rejection. The key must identify an existing Back, start with `b_`, and not already have an adapter. Version is 1..999999 and defaults to 1.

A static baseline describes the deck under factory starting settings, before stake effects. For explicit player-defined starting values, Showstreak adds the difference from the factory value. In this example, default hands are 4, the deck adds +2, and the baseline is 6. If the player sets base hands to 7, the result before stake effects is 9.

Any change to the starting cards must agree with the actual Back. Baseline deck_size does not itself implement your deck's suits, ranks, or special rules. The preview must agree with the actual run start.

<!-- pagebreak -->

## 9. A Computed Baseline and a Named Deck Ticket

Instead of a static table, register_deck accepts a baseline function. This is an exception to the data-only rule for item definitions. The function receives a copy of rules and returns the **final resources before stake effects**, already accounting for the player's settings. Showstreak does not add the starting-value difference a second time.

This is an alternative to the static registration in section 8; do not run both for the same key:

```lua
local S = assert(Showstreak)
local key, why = S.register_deck{
    key = 'b_sse_reserve', version = 1,
    baseline = function(rules)
        local s = rules and rules.version == 2 and rules.start
        if not s then
            s = {money = 4, hands = 4, discards = 3,
                 hand_size = 8, joker_slots = 5,
                 consumable_slots = 2, deck_size = 52,
                 shop_slots = 2, booster_slots = 2}
        end
        return {
            money = s.money, hands = s.hands + 2,
            discards = s.discards, hand_size = s.hand_size,
            joker_slots = s.joker_slots,
            consumable_slots = s.consumable_slots,
            deck_size = s.deck_size, shop_slots = s.shop_slots,
            booster_slots = s.booster_slots,
        }
    end,
}
assert(key, tostring(why))
```

The callback is trusted code from your mod. It may be called for previews, so do not draw from RNG, make network requests, write saves, or mutate global state. The returned table is validated when used; an incomplete result raises a diagnostic naming the provider and deck. Successful registration alone does not prove that every callback result is valid.

After either adapter, you can add a ticket:

```lua
local id, why = Showstreak.register_item{
    id = 'showstreakexample_reserve_ticket', kind = 'trick',
    effect = 'set_deck', value = 1, deck = 'b_sse_reserve',
    price = 3, art = 'c_world',
    family = 'showstreakexample_tickets', family_weight = 1,
    loc_txt = {name = 'Reserve Ticket',
               text = {'Use Reserve Deck for the next run'}},
}
assert(id, tostring(why))
```

<!-- pagebreak -->

## 10. Showman Dialogue

Registration accepts only `provider`, `id`, `event`, `match`, `priority`, `once`, and `text`. An unknown field is rejected with `field`. Fields such as presentation, animation, calculate, sprite, and custom callbacks are not supported here: Showstreak's controller chooses the expression and transition back to idle.

`text['en-us']` is required. Each language contains an array of 1..4 strings with a combined length of at most 140 UTF-8 codepoints. Strings cannot contain `{`, `}`, CR, or LF. Multiple source strings are joined into one reaction and reflowed; they do not prescribe four fixed on-screen lines.

The current renderer measures text width, targets five displayed lines within width 2.72, and reduces scale from 0.30 to 0.24. At the minimum scale it preserves the words even if the target line count is exceeded. Test your longest names and substitutions, and shorten the wording yourself rather than relying on automatic ellipsis.

Priority is an integer from 1 to 90, with a default of 50. A higher priority helps a matching reaction get selected, but repetition history and context also matter. `once=true` means once per profile: the reaction is remembered when it is selected for a visible screen. Merely sending an event does not guarantee that the player sees the text.

Language fallback for an external reaction checks the requested code, normalized code, base language, game language, default, and en-us. Supported game codes are `en-us`, `ru`, `de`, `es_419`, `es_ES`, `fr`, `id`, `it`, `ja`, `ko`, `nl`, `pl`, `pt_BR`, `zh_CN`, and `zh_TW`. Preserve filename spelling and case on case-sensitive systems.

`match` defines exact comparisons against public fields. Strings, booleans, and finite numbers are accepted. A field absent from the event does not match. Campaign context may replace supplied stars/deck/stake values with their current values.

| Group | Fields for matching and substitution |
|---|---|
| Item/action | item_id, source, effect, kind, lifetime, value, reward, price |
| Campaign/shop | stars, free_slots, deck, stake, ante, cost, rerolls, remaining |
| Native game | native_key, native_set, native_action |
| Result | wins, reason, amount, dark_amount, has_active, record, act_reward, won_bet |

`#item_name#`, `#deck_name#`, and `#native_name#` are additional localized **display** names, not match fields. An unknown or missing substitution becomes `?`. Input strings are sanitized and trimmed to 160 codepoints; numeric values must be finite. There is no separate 1e9 magnitude limit for event data. Do not supply hidden masks, future offers, or any other result that the player has not yet been shown.

<!-- pagebreak -->

## 11. Events and Player Preferences

Built-in successful actions include buy, voucher, contract, pack, take, use, reveal, remask, and mask. `intermission` reports an accepted Intermission. For Showstreak cards, source distinguishes buy, held, pack, and mask. Purchasing a preparation item and using it are separate events. Do not manually send a second buy event on top of the one Showstreak has already generated.

A custom event name at registration must begin with the exact provider ID and a colon, such as `ShowstreakExample:encore`. Event names may contain letters, digits, `_`, `:`, and `-`. The reaction ID itself still uses `showstreakexample_`.

```lua
local S = assert(Showstreak)
local id, why = S.register_showman_reaction{
    id = 'showstreakexample_encore',
    event = 'ShowstreakExample:encore',
    match = {source = 'encore'}, priority = 60, once = false,
    text = {
        ['en-us'] = {'An encore worth #amount# Stars.'},
        ru = {'Выход на бис принёс #amount# звезды.'},
    },
}
assert(id, tostring(why))
```

After your mod's action succeeds, report the **actual** result:

```lua
-- Only after your own action has actually succeeded:
Showstreak.showman_event('ShowstreakExample:encore', {
    source = 'encore', amount = 2,
})
```

This call does not award two Stars, perform a transaction, or open a screen; it requests presentation of the event. Do not call it every frame or put it in startup code expecting the player to see the reaction immediately. Context, preferences, and repetition rules determine what is actually displayed.

Players have independent quips, voice, effect_sounds, and reduced_motion preferences. Do not force them on for an integration. Visual reactions may still work with voice disabled; text delivery is not promised with quips disabled. Showman's randomness is separate from gameplay randomness.

The existing helper `Showstreak.visuals.configure_host_sprites` changes the frame mapping for newly created portraits, but it is not one of the API v1 registration functions and does not give a reaction author control over a specific frame. Ordinary provider mods do not need it. Replacing the global appearance requires a separate check against the current visual controller; do not use the reaction API to introduce it as a hidden callback.

<!-- pagebreak -->

## 12. Saves and Provider Updates

A new campaign copies its rules, item and condition definitions, catalog order, and provider information. This is a frozen snapshot. Changes in a new integration version apply to new campaigns; changing value in the live catalog does not update an existing campaign.

Providers represented in the saved catalog and enabled decks are required, **not just providers of items the player has purchased**. If a required provider disappears, continuation is blocked with an explanation. Leaving the mod folder installed while removing all its registrations is not enough to retain its previous integration capability.

| Update | Expected behavior |
|---|---|
| Add a new item | It appears in new campaigns |
| Change value for an existing ID | New campaigns use the new value; existing ones use the saved value |
| Retire an ID while keeping its provider | The old definition can be restored when its card is shown |
| Remove a required mod | Continuation is blocked |
| Change only the version number or artwork | Neither automatic rejection nor a guarantee of compatibility |
| Change the native Back or external hooks | The author remains responsible; frozen data does not freeze Lua code |

For an ID removed from the live catalog, Showstreak restores the center from its saved definition. Current provider translations take precedence; loc_txt from the save supplies a fallback. Textures and code are not embedded in the save. If the artwork is unavailable, native artwork is used without changing the saved effect.

Keep public IDs stable while the item's meaning remains the same. For a fundamentally different meaning, prefer a new ID that does not replace the old one in players' saves. An item's `content_version` and an adapter's `version` are metadata, not automatic migration mechanisms. There is no public migrate/register_migration API. Do not edit players' *.jkr files or schema numbers to bypass diagnostics.

Intermission preserves the frozen catalog; accepting it does not load items from a newer provider version. Older campaigns' policies are not silently extended with new content either.

Before releasing an update, test an existing save with a purchased or retired item, EN/RU text, a restart, and a temporarily disabled provider. Keep compatible registrations that can be tested while players may still be continuing campaigns from previous versions.

<!-- pagebreak -->

## 13. Diagnostics and Common Errors

The registration functions return **two values**. Use `local id, why = ...`; an assert message without reason hides the cause. For a center error, inspect `S.api.last_error`.

| Reason | What to check |
|---|---|
| registration_closed | The call runs after boot; move it into mod loading |
| data_only | A function, metatable, cycle, userdata, NaN, or infinity is present |
| provider | The ID is incorrect or the mod is unavailable |
| id / namespace | Mod ID, SMODS prefix, and item namespace have been confused |
| duplicate | The ID/adapter is already registered, or the code ran twice |
| kind_effect / value | An incompatible pair, wrong type, or incorrect fixed value |
| price / reward | Missing price, a paid contract, or a contract without reward |
| requires | The parent is not a voucher, has not been registered yet, or refers to itself |
| art / atlas | A missing center/atlas, non-integer pos, or incorrect key |
| deck / deck_adapter / baseline | A missing Back/adapter or an incomplete set of nine resources |
| family / family_weight / weight | Another provider's namespace, inconsistent group weights, or a value outside 1..100 |
| pack | An unsupported pool, value outside 1..5, or choose greater than value |
| text / match / event / field | Invalid dialogue, a non-public data field, an invalid event name, or an unknown field |
| version / content_version | An invalid integer for the corresponding version |

`provider_manifest()` returns copies of the providers and mods tables. A provider has version and capabilities: items, decks, and showman. `mod_report()` includes id, name, version, loaded, integrated, capabilities, changed, and missing. `environment_report()` lists the game and loaders separately. These functions provide diagnostics; they do not authorize modifying internal state.

**An item does not appear:** first verify the registration return value, then check that this is a new campaign, that parent voucher requirements are met, that the deck/effect is eligible, and that shop offers are random. Successful registration does not guarantee an offer.

**The item was bought but has no effect:** a prop requires Use; vouchers/contracts activate when bought/accepted; tricks act immediately on Use. Compare the result against the selected deck's and stake's resources.

**A reaction does not play:** inspect text/match, once status for the profile, player preferences, and the active screen. showman_event does not guarantee an interruption of the current reaction. **A campaign will not continue:** check the missing provider and required decks; do not erase saves to hide the problem.

<!-- pagebreak -->

## 14. Testing Before Release

Start with Lua syntax checks and contract tests in the source tree. The player ZIP does not need to contain tests. From the Showstreak source tree, run the test runner with the LuaJIT DLL from your Windows game installation:

```text
python Mods/Showstreak/tests/run_tests.py <path-to-lua51.dll>
```

This runner uses a Windows DLL and reads the corresponding Balatro.exe. It is a development tool, not an instruction for players to install Python. The examples in this guide have also been executed separately against the real API functions with a mocked environment; that does not replace testing in the graphical game.

| Check | Expected result |
|---|---|
| Clean startup | No loading errors; the provider is integrated |
| New campaign | Your entries are included in the saved catalog |
| Buy/Use/Take | Correct activation timing and price; failed actions have no effect |
| Contract | Victory reward is paid once |
| Voucher chain | The parent unlocks the upgrade; effects stack |
| Deck | Preview and run start agree under default/custom rules and stakes |
| Save/restart | Resources are not reapplied; the catalog is retained |
| Old save | A changed/retired ID uses its frozen definition |
| Missing provider | Continuation is correctly blocked with a reason |
| UI and input | Tooltips, languages, 1x/2x textures, mouse, and controller are usable and readable |
| Showman | No repeated event spam; voice/quips/reduced motion are respected |
| File package | One provider folder; no bundled external runtime, saves, or QA files |

Check both successful purchases and rejection with insufficient Stars, a full inventory, or a stale confirmation. For a deck callback, use the minimum, default, and maximum permitted settings, plus rules=nil. For text, test long names and an absent optional field.

A test profile keeps your ordinary gameplay history separate, but it is not an independent sandbox for all mod code. Keep backups of test saves and do not run debug setters against your main profile. Do not alter gameplay RNG merely to demonstrate an item in a public build.

The "integrated" label means an API capability was registered. It does not guarantee compatibility with every external hook, game balance, or every combination of mods.

<!-- pagebreak -->

## 15. Packaging Your Own Mod

Your ZIP should contain one folder with your manifest, main.lua, your own scripts/assets/localization, player instructions, and terms for your code and resources. Declare Showstreak >=1.0.0, or the later minimum version your integration actually requires, as a dependency. State the exact game/loader combination you tested in the release description.

Do not bundle Showstreak, Balatro, Steamodded, Lovely, original Showstreak resources, config/Showstreak.jkr, user presets, *.jkr files, test profiles, dumps, or QA scripts. Players install Showstreak separately. The LICENSE permits independently written integrations; supplied tutorial examples and documentation are for learning and local testing, without permission to redistribute them. Distributing copies of the examples or other Showstreak files requires separate permission from the author.

Before publishing, extract **your actual final ZIP** into a test environment and repeat the minimum sequence: load, create a new campaign, obtain an item, Use, start a run, save, and restart. Testing a developer's working folder will not reveal a PNG or module omitted from the archive.

Explain to players that removing a provider may block continuation of campaigns whose catalogs contain its items. Do not promise unrestricted removal in the middle of a campaign. For updates, explain which changes apply only to new campaigns.

## 16. Where to Verify the Contract

The main companion reference is [API.md](API.md); the tutorial provider is documented in [examples/integration/README.md](examples/integration/README.md). These materials cover Showstreak 1.0.0 and API v1. They are supplied with the source/developer materials, separately from the lean player ZIP; this guide contains the code needed for its standalone tutorial.

Primary implementation sources:

- `scripts/integration/api.lua`: registration, value limits, artwork, families, adapters, and reports.
- `scripts/showman/showman.lua`: accepted reaction fields, event/match/text, substitutions, repetition, and public event delivery.
- `scripts/localization/locale.lua`, `localization.lua`, `text.lua`: fallback, description variables, and text wrapping.
- `scripts/ui/cards.lua`: creating and restoring centers from the saved catalog.
- `scripts/integration/integration.lua`: applied resources, baselines, provider manifests, and continuation checks.
- `scripts/core/core.lua`: the deterministic catalog, items, packs, vouchers, contracts, and Intermission.

In the source version of the project, further checks are in `tests/api_v5_spec.lua`, `api_restore_v7_spec.lua`, `showman_v7_spec.lua`, `presets_v5_spec.lua`, and the save/Intermission tests. The v5/v7 suffixes identify when the tests were introduced, not the required public API version.

This guide does not introduce mechanisms beyond the implementation. If the extension you need is absent from the documented contract, first agree on and implement a new API capability, then document it and update your provider's required version.
