# Showstreak API v1

For Showstreak **1.0.0**, by **TraditionalDimension**. API contract version: **1**. This document describes the implemented API; it does not certify compatibility with every mod. Practical modding guides are available in [English](MODDING_GUIDE_EN.md) and [Russian](MODDING_GUIDE_RU.md).

## Registering an integration

Public functions are available after Showstreak initializes:

```lua
Showstreak.api.version                 -- 1
Showstreak.register_item(def)          -- id, or nil, reason
Showstreak.register_deck(def)          -- deck key, or nil, reason
Showstreak.provider_manifest()        -- registered providers and loaded mods
Showstreak.mod_report()               -- real mods, including changed/missing entries
Showstreak.environment_report()       -- game and loaders: id, name, version
Showstreak.register_showman_reaction(def) -- reaction id, or nil, reason
Showstreak.showman_event(kind, data)   -- report a public extension event
```

Register during mod loading, after Showstreak and before `SMODS.booted`. Registration after boot returns `registration_closed`. Steamodded loads priorities in ascending order; Showstreak uses `10000010`. An integration can use a later priority and declare its dependency:

```json
{
  "id": "ExampleMod",
  "name": "Showstreak Example",
  "author": ["Your name"],
  "prefix": "exm",
  "main_file": "main.lua",
  "version": "1.0.0",
  "priority": 10000011,
  "dependencies": ["Showstreak (>=1.0.0)"]
}
```

The provider defaults to `SMODS.current_mod.id`. An explicit provider must identify the current or another loaded, enabled mod. Item and family namespaces use the **mod ID**, lowercased with punctuation replaced by underscores, followed by `_`. Thus `ExampleMod` owns `examplemod_`, regardless of its Steamodded prefix `exm`. Namespace collisions between different providers are rejected. IDs must match `^[a-z][a-z0-9_]*$`.

Item definitions must contain plain serializable data. Functions, userdata, metatables, cyclic tables, NaN and infinity are rejected. Definitions are copied on registration. Do not mutate `Showstreak.content`, its order, or a campaign's saved catalog.

## Item definition

```lua
local id, reason = Showstreak.register_item{
    id = 'examplemod_spare_hand',
    kind = 'prop', effect = 'hands', value = 1,
    price = 3, art = 'j_joker',
    loc_txt = {
        name = 'Spare Hand',
        text = {'{C:blue}+#1#{} hand each round', 'during the next run'},
    },
}
assert(id, reason)
```

| Field | Contract |
| --- | --- |
| `id` | Required unique namespaced ID. |
| `provider` | Optional loaded provider ID; defaults to the current mod. |
| `kind`, `effect`, `value` | Required; supported combinations are listed below. |
| `price` | Integer 0–999 stars. Contracts require 0. |
| `art` | Existing native or registered SMODS center key whose artwork is reused. |
| `atlas`, `pos` | Alternative static artwork: known atlas and integer `pos.x`, `pos.y` in 0–9999. Explicit atlas takes precedence over `art`. Register custom atlases first. |
| `loc_txt` | Optional Steamodded localization data for the generated center. Supply English source text so saved items keep meaningful descriptions after an integration update. |
| `weight` | Optional integer 1–100; default 1. |
| `family`, `family_weight` | Optional namespaced family and integer 1–100 group weight. All members must agree on group weight; default 1. A family weight without a family is rejected. |
| `requires` | Voucher only: an already registered parent voucher ID. Register parents first. |
| `reward` | Required integer 0–999 for contracts; also permitted for an Ante trick. Other uses are rejected. |
| `max_once` | Optional boolean for an Ante trick only. |
| `deck` | Required destination for `set_deck`; an existing supported `b_...` Back. A modded Back requires a registered deck adapter. |
| `choose`, `pool` | Packs only: choose 1–`value`, pool `props` or `tricks`. The props pool includes props and tricks. |
| `content_version` | Optional integer 1–999999, default 1. Metadata, not an automatic migration hook. |

### Kinds and effects

Resource effects are `money`, `hands`, `discards`, `hand_size`, `joker_slots`, `consumable_slots`, `deck_size`, `shop_slots`, and `booster_slots`. Their values are integer changes from -999 to 999. Run effects additionally include `blind`, `boss`, and `prices`; these use finite fractional changes, such as `0.15` for +15%. Contributions to the same modifier add together. The series score factor is separate.

| Kind | Effects | Activation and lifetime |
| --- | --- | --- |
| `prop` | Any run effect | Buy/Take adds it to held items. Manual Use prepares its change for the next run. |
| `trick` | `reveal`, `reveal_all`, `deck`, `set_deck`, `stake`, `ante`, `remask`, `remove_condition` | Buy/Take adds it to held items. Manual Use performs the action immediately. |
| `voucher` | Any run effect, plus `reward`, `held_slots`, `reveal_act`, `free_reroll` | Buying activates it until an accepted Intermission or the series ends. |
| `contract` | Any run effect | Accepting prepares the next run's change and victory reward. Free to accept. |
| `pack` | `pack` | Opening presents choices; selected items enter the held inventory. |

Values for `reveal`, `reveal_all`, `reveal_act`, `deck`, `set_deck`, `remask`, and `remove_condition` must be `1`; `stake` must be `-1`. An Ante trick uses a nonzero integer from -15 to 15. `reward`, `held_slots`, and `free_reroll` effects require positive integers up to 999. A pack's `value` is its offered item count, 1–5.

The API assigns activation, lifetime and visual category. It does not accept arbitrary effect callbacks, new effect types, custom pack pools, paid contracts or condition-family registration. Validation checks supported behavior, not balance: extreme percentage combinations can still be unreasonable.

### Localization and artwork

The generated description key is `sstreak_item_<full item ID>` in the `Showstreak` description set. Description variables are `#1#` = value (localized exact ratio words for blind/boss/prices, such as `one fifth`), `#2#` = reward or pack choice count, and `#3#` = value plus its voucher prerequisite chain's values. Preserve Balatro's formatting tokens in translations.

Artwork registration resolves a center to static atlas coordinates, including centers registered before injection into `G.P_CENTERS`. Validation checks known atlas IDs and coordinates, not actual image dimensions, file availability or animation fidelity. Frozen definitions retain the `loc_txt` supplied at registration; they do not embed texture files or translations supplied only by external localization files.

### Weighted families

An eligible family receives its group weight once; an item is then selected inside that family using member weights. Adding fifteen deck choices therefore does not give their family fifteen times the weight. Eligibility is checked before drawing. Packs do not repeat an item ID within one opening. Custom contracts and packs join the corresponding eligible pools.

Built-in order is preserved; external items are placed in stable ID order. Register voucher prerequisites first even though final catalog order is sorted. A started series keeps its copied order and definitions.

## Deck adapters

Modded decks are opt-in. A deck must have an existing registered Back and an adapter describing all nine resource baselines. The adapter does not implement the native Back: its values must agree with that Back's effects.

```lua
-- Assumes b_exm_reserve is already registered and normally grants +2 hands.
local key, reason = Showstreak.register_deck{
    key = 'b_exm_reserve',
    version = 1,
    baseline = {
        money = 4, hands = 6, discards = 3, hand_size = 8,
        joker_slots = 5, consumable_slots = 2,
        deck_size = 52, shop_slots = 2, booster_slots = 2,
    },
}
assert(key, reason)
```

`key` must be an existing `b_...` Back and cannot be registered twice. `provider` follows item provider rules; `version` is an integer 1–999999. The baseline requires exactly the nine resource fields, excluding Ante. Values must be integers: money -999–999, others 0–999.

A static baseline describes the deck under factory starting values, before stake effects. Showstreak adds the difference between the player's explicit starting value and its factory value. In the example, setting base hands to 7 produces 9 hands before stake effects.

Alternatively, `baseline` may be a function receiving a copied rules table and returning the **final** resource baseline before stake effects:

```lua
baseline = function(rules)
    local s = rules and rules.version == 2 and rules.start
        or Showstreak.core.START_DEFAULTS
    return {
        money = s.money, hands = s.hands + 2, discards = s.discards,
        hand_size = s.hand_size, joker_slots = s.joker_slots,
        consumable_slots = s.consumable_slots, deck_size = s.deck_size,
        shop_slots = s.shop_slots, booster_slots = s.booster_slots,
    }
end
```

The callback is trusted mod code, not sandboxed. It may run for previews, so keep it deterministic and free of RNG draws, save writes and game-state mutation. Its returned data is validated when used; invalid output raises a diagnostic naming the deck/provider. Do not return an incomplete baseline.

A registered deck can be offered by a named deck trick:

```lua
assert(Showstreak.register_item{
    id = 'examplemod_reserve_ticket', kind = 'trick',
    effect = 'set_deck', value = 1, deck = 'b_exm_reserve',
    price = 3, art = 'c_world',
    family = 'examplemod_deck_tickets', family_weight = 1, weight = 1,
    loc_txt = {name = 'Reserve Ticket', text = {'Use the Reserve Deck', 'for the next run'}},
})
```

A named deck trick is eligible only when that deck is in the series' enabled deck list and differs from the next deck. Using it changes only the next deck; the next run's seed is retained and no additional deck-bag draw occurs.

## Frozen campaigns and diagnostics

New campaigns include an optional Intermission policy. An offer may appear at a completed Act boundary after three local Acts. Accepting it keeps the campaign ID, total win streak, profile Dark Stars, settlement history, and frozen rules/catalog/provider manifest, while clearing held items, owned vouchers, all accumulated conditions and next-run preparation. Gold Stars and local Act/Stake progression return to their rule-defined starting values. Future gameplay draws use a new deterministic segment seed; run IDs and Dark Star payout checkpoints remain global. Declining continues the existing progression. Older saved campaigns without this policy do not silently receive it or new condition-pool entries.

A new campaign copies finalized rules, item definitions, condition definitions and catalog order. Editing or removing a live item definition does not rewrite that saved data. Gameplay signatures exclude artwork coordinates and similar cosmetic fields. Required providers include providers represented in the saved catalog and enabled decks, not just purchased items. Missing required providers block continuation with an explanation.

The manifest records mod/provider versions and capabilities. New manifests do not reject every cosmetic update solely because a version changed. Older saves without a manifest retain their legacy environment checks before migration. If an item ID is retired while its required provider remains registered, its native omitted center is rebuilt from the saved definition when shown. Available provider translations are preferred; saved `loc_txt` supplies the fallback. If neither exists, the item ID and a supported-effect description are shown. Missing textures fall back to native artwork without changing the saved effect. Frozen data cannot preserve missing provider code, texture files or arbitrary external hooks.

`provider_manifest()` reports loaded mods and registered integration capabilities. `mod_report()` distinguishes loaded, integrated, changed and missing real mods. Balatro and the loaders are listed separately by `environment_report()`; Lovely-only content remains a real mod. Environment versions are still retained in diagnostic manifests. An integration badge means that the mod registered supported Showstreak content; it is not proof that all of its unrelated hooks are compatible. A mod with no integration is unverified, not automatically incompatible.

Registration returns a short reason such as `registration_closed`, `data_only`, `provider`, `id`, `namespace`, `duplicate`, `kind_effect`, `price`, `value`, `reward`, `requires`, `max_once`, `content_version`, `art`, `atlas`, `deck`, `deck_adapter`, `weight`, `family`, `family_weight`, `pack`, or `center`. A native center registration failure leaves the Showstreak catalog unchanged; inspect `Showstreak.api.last_error` for details.

Use the API tests and an isolated in-game test profile to verify registration, purchase, manual Use, fresh-run application and resume behavior. Headless validation does not establish rendering, controller, audio or full-mod compatibility.


## Contextual Showman reactions

The Showman is local and event-driven. Integrations may register data-only reactions; arbitrary callbacks and generated network dialogue are not part of this API.

```lua
local id, reason = Showstreak.register_showman_reaction{
    id = 'examplemod_spare_hand_bought',
    event = 'buy',
    match = {item_id = 'examplemod_spare_hand', source = 'buy'},
    priority = 60,
    once = false,
    text = {
        ['en-us'] = {'An extra hand. Keeping something up your sleeve?'},
        ru = {'Лишняя рука. Припрятал ещё один козырь?'},
    },
}
assert(id, reason)
```

Register after Showstreak loads and before `SMODS.booted`. `provider` defaults to the current mod and IDs use the same provider namespace as items. `text['en-us']` is required. For external reactions, selection checks the exact requested locale, normalized code, base language, the game's language, `default`, then English. Built-in UI and Showman dialogue cover all 15 supported game languages; unsupported regional variants use their supported base language. Keep a complete English fallback even when supplying other translations. A language contains one to four source strings, with at most 140 UTF-8 codepoints in total. Source strings cannot contain braces, CR or LF: Showman dialogue is plain text, not Balatro formatting markup. Strings are joined and reflowed by measured text width. The current renderer targets five displayed lines in width 2.72, reducing scale from 0.30 to 0.24; its 29-column fallback is not a fixed per-line character contract. It preserves text at the minimum scale even if the target cannot be met, so keep substitutions short and visually test the longest translated result. `priority` is an integer from 1 to 90, default 50. `once` is an optional boolean: `true` means once per profile, remembered when the reaction is selected for a visible screen, not merely when an event is emitted. The profile stores reaction IDs; text is not a saved gameplay effect. Reactions are cosmetic and do not become a campaign's required gameplay providers.

`match` contains exact scalar comparisons against public event data. Supported fields are `item_id`, `source`, `effect`, `kind`, `lifetime`, `value`, `reward`, `price`, `stars`, `free_slots`, `deck`, `stake`, `ante`, `cost`, `rerolls`, `remaining`, `native_key`, `native_set`, `native_action`, `wins`, `reason`, `amount`, `dark_amount`, `has_active`, `record`, `act_reward`, and `won_bet`. Not every field is present on every event. String, boolean and finite numeric matches are supported. Named substitutions such as `#value#` refer to public fields. Derived display substitutions `#item_name#`, `#deck_name#` and `#native_name#` resolve localized names; they are display text rather than additional `match` fields. Missing substitutions become `?`. Input strings are sanitized and trimmed to 160 codepoints; numeric fields must be finite (NaN and positive or negative infinity are omitted). The campaign snapshot may supply or replace contextual fields such as stars, deck and stake. No callbacks, hidden mask IDs, future offers or gameplay RNG are exposed.

The only accepted top-level reaction fields are `provider`, `id`, `event`, `match`, `priority`, `once`, and `text`. In particular, `presentation`, custom sprite frames and animation callbacks are not accepted. The presenter chooses expression and idle/talking behavior. A custom event name must begin with the provider ID exactly, followed by `:`, for example `ExampleMod:encore`; names use letters, digits, underscore, colon or hyphen. Reaction IDs still use the lowercased item-style namespace.

Successful card events include `buy`, `voucher`, `contract`, `pack`, `take`, `use`, `reveal`, `remask`, and `mask`. The optional `intermission` event reports an accepted Intermission. For a Showstreak card, `source` identifies its interaction: `buy`, `held`, `pack`, or `mask`. Buying a prop and using it are separate events. Targeted reveal/remask events identify the consumed trick. Failed actions and repeated stale confirmations do not issue purchase/use events.

An integration may report its own public event after its action succeeds:

```lua
Showstreak.showman_event('ExampleMod:encore', {amount = 2})
```

Register a reaction with the same `event = 'ExampleMod:encore'`. Event delivery is a presentation request, not a guaranteed interruption: built-in context, priority, repetition controls and the player's dialogue settings decide what is shown. Keep event details truthful and limited to information already visible to the player.

## Versioning and an installable example

The 1.0.0 release keeps `Showstreak.api.version == 1`. Require the Showstreak mod version you tested and check the API version/functions you use. `content_version` and deck adapter `version` are metadata, not migration callbacks. Keep published IDs stable and register voucher parents before their upgrades. There is no public migration, provider-only, condition, settings, or preset-registration API. Unknown newer save schemas stop loading rather than being rewritten by an integration.

An existing series uses its saved item definitions and rules; newly registered items appear in new series. Provider versions are recorded for diagnostics, but a changed version alone does not certify or reject compatibility. Removing every registration from a required provider blocks continuation even if its mod is still loaded. Changes to a provider's native Back implementation or unrelated hooks remain that provider's responsibility.

A small installable example is in [examples/integration](examples/integration/README.md). Its source and manifest use `.example` extensions so merely installing Showstreak cannot load the example as a second mod. The example includes localized items and a contextual reaction. Rule presets remain JSON documents shared through the Rules importer; there is currently no public `register_preset` function.

Before publishing an integration, verify its actual artwork, translated tooltip, purchase, manual Use, next-run application, save/restart, update-with-retired-ID and missing-provider behavior in an isolated game profile. The headless suite verifies API contracts and recovered card construction, and cannot certify graphics, controller handling, every external hook or balance.

## Release and distribution

Distribute your own independently written integration as a separate mod that depends on Showstreak. Do not bundle Showstreak runtime files, original artwork, audio, documentation, supplied examples or player saves in the integration archive. Tutorial examples are for learning and local testing; they are not licensed for redistribution. Consult the release's [LICENSE](../LICENSE) for permitted uses and request separate author permission to redistribute supplied files. The public API does not grant redistribution rights to the mod's assets. Showstreak's limited-use license is not an open-source license.
