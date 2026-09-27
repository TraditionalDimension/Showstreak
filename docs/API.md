# Showstreak API reference

Showstreak **1.3.0** · API contract **1** · revision **1.1.0**  
Author: TraditionalDimension

This reference covers the established API and the current 1.3.0 metadata. Read the [1.3.0 appendix](API_130.md) for complete version-2 pack receipts, action policies, safe-copy and Bet-tier rules, and starting-Joker occupancy. The [Toolkit guide](TOOLKIT.md) separates integration examples from the standalone story editor.

This reference describes the implemented extension contract. The practical
[English](MODDING_GUIDE_EN.md) and [Russian](MODDING_GUIDE_RU.md) guides explain
how to build, test and maintain an integration. The complete executable example
is supplied as [inert source files](examples/integration/README.md), including
[main.lua.example](examples/integration/main.lua.example). Rename them only in a
separate test mod as explained there.

## 1. Initialization and ownership

Register while mods initialize, before `SMODS.booted`. Register a real native
Back or an atlas before an adapter or item that refers to it. Register custom
effects before their items/conditions and voucher parents before upgrades.

The recommended entry point is a function on the integration's SMODS mod object.
It receives a provider-bound facade. Use dot calls, not colon calls.

```lua
local mod = assert(SMODS.current_mod)
mod.showstreak_init = function(api)
    assert(api.version == 1)
    -- Register your definitions here, using api.register_item {...}.
end
if Showstreak and Showstreak.api and Showstreak.api.ready then
    assert(Showstreak.initialize_integration(mod.id))
end
```

Showstreak discovers earlier-loading hooks after all its APIs are initialized.
The conditional handles a later-loading integration. Hooks execute once per
provider during successful initialization. Do not also call the hook manually.
Showstreak loads at priority `10000010`; the SDK's priority `0` deliberately tests
the early path. A dependency alone does not replace the hook.

Declare `Showstreak (>=1.1.0)` for the baseline contract, or
`Showstreak (>=1.3.0)` when requiring the additions documented here. Query the
relevant capabilities as well; the API revision alone does not identify them. Keep the provider ID
stable. `ShowstreakExample` owns the prefix `showstreakexample_`, regardless of
its SMODS prefix `ssex`. Normalization lowercases the provider ID, replaces each
non-alphanumeric/underscore character with `_`, and appends `_`. Namespaced IDs
must match `^[a-z][a-z0-9_]*$`. Colliding normalized provider namespaces fail.
Native Back keys follow SMODS naming, not this item namespace rule.

Registrations return `id`/`key`, or `nil, code, detail`. Successful initialization
returns `true`. Existing callers that read only the first two values remain valid.
An initialization error is not a transaction: earlier successful registrations
in the same failed hook are not rolled back. Correct the error and restart.

## 2. Public entry points

Unless qualified otherwise, these are functions on `Showstreak`. A facade
returned by `for_provider(id)` contains the eight registration methods and the
nine query/event methods explicitly listed as facade methods below.

| Function | Result / purpose | Facade |
| --- | --- | --- |
| `register_item(def)` | Namespaced item ID | Yes |
| `register_deck(def)` | Existing native Back key | Yes |
| `register_condition(def)` | Namespaced condition ID | Yes |
| `register_mask(def)` | Alias of `register_condition` | Yes |
| `register_effect(def)` | Namespaced custom effect ID | Yes |
| `register_ban_source(def)` | Namespaced deny source ID | Yes |
| `register_start_addon(def)` | Namespaced start category ID | Yes |
| `register_showman_reaction(def)` | Namespaced reaction ID | Yes |
| `register_action_policy_source(def)` | Declares action restrictions; see [1.3.0](API_130.md#action-policy-sources) | No |
| `get_api_info()` | Copied API metadata | Yes |
| `get_api_error()` | Latest copied diagnostic, or nil | Yes |
| `get_catalog(campaign?)` | Copied frozen/live catalog | Yes |
| `get_effect_info(id, campaign?)` | Copied effect metadata | Yes |
| `preview_effect(id, value, campaign?)` | Copied visible effect description | Yes |
| `query_object_ban(key, campaign?)` | Copied deny result | Yes |
| `get_showman_info()` | Copied speech contract metadata | Yes |
| `preview_showman_reaction(id, data?)` | Rendered preview, without speaking | Yes |
| `showman_event(event, data?)` | Requests contextual speech | Yes |
| `for_provider(id)` | Bound facade, or registration-style error | No |
| `initialize_integration(id)` | Runs a provider hook once | No |
| `provider_manifest()` | Loaded mods and provider capabilities | No |
| `mod_report()` | Loaded/integrated/changed/missing real mods | No |
| `environment_report()` | Game and loader versions | No |
| `deck_provider(key)` | Adapter provider, default `Showstreak` | No |
| `showman_delivery(name)` | Copied delivery settings; unknown → calm | No |
| `visuals.configure_host_sprites(def)` | Optional artist frame map | No |

`initialize_integrations()` is the host's end-of-load scan; integrations should
use their hook and singular `initialize_integration`. Other `core`, `api` helper,
UI, lifecycle and serialization functions are internal. The public API does not
expose preset registration, arbitrary save migration, arbitrary instant trick
callbacks, arbitrary pack-pool generators or a general shop replacement.
The supported `vouchers` and `mixed` pack pools are available in 1.3.0 through
`pack_version=2`; they do not expose arbitrary pool registration.

The facade also has `provider`, `version`, `revision`. It binds `provider` without
altering `SMODS.current_mod`. An explicitly different provider is rejected.
Direct registration infers the current mod or accepts an enabled loaded provider.

## 3. Metadata and diagnostics

`get_api_info()` returns `version=1`, `revision='1.1.0'`, `ready`,
`registration_open`, `capabilities`, `item_effects`, `condition_effects`, and
`condition_categories`. Effect maps are boolean membership maps. Registered
custom effects appear in the appropriate maps; categories are `economy`,
`blinds`, `resources`. Query the maps instead of assuming all future revisions
have exactly the same capabilities.

| Capability | Value in Showstreak 1.3.0 |
| --- | --- |
| `items`, `decks`, `conditions`, `masks` | 1 |
| `casting_reveal`, `ban_sources`, `object_ban_query` | 1 |
| `custom_effects`, `start_addons` | 1 |
| `pack_pools` | 2 |
| `action_policy_sources` | 1 |
| `provider_facade`, `initialization_hook`, `diagnostics` | 1 |
| `showman` | 2 |
| `showman_compact_text`, `showman_preview` | 1 |

The latest diagnostic has `operation`, `code`, `field`, `provider`, `id`,
`message`; unknown metadata may be absent. The short `code` remains suitable for
programmatic checks; `message` explains a field or callback failure. A successful
registration clears the latest structured diagnostic. Query methods are not
guaranteed to clear it. Legacy `Showstreak.api.last_error` remains a string.

Common codes: `registration_closed`, `data_only`, `provider`, `id`, `namespace`,
`duplicate`, `field`, `center`. Initialization also uses `not_ready`,
`showstreak_init`, `initialization_error`. Native center errors retain the
underlying error string. Diagnostics sanitize thrown non-string error objects.

Except for documented callbacks, definitions contain finite numbers, strings,
booleans and plain acyclic tables. Functions, userdata, metatables, NaN and
infinities cannot be frozen into a series. Arrays must be dense where specified.
Do not mutate registries, copied metadata or saved catalogs to register content.

## 4. Items: fields

| Field | Contract |
| --- | --- |
| `id`, `provider` | Namespaced ID; provider inferred/bound when omitted |
| `kind`, `effect`, `value` | Required supported combination below |
| `price` | Integer 0–999 Gold Stars; contracts require 0 |
| `art` | Existing native or registered SMODS center key |
| `atlas`, `pos` | Alternative known atlas, integer x/y 0–9999 |
| `loc_txt` | Native localization data; English fallback recommended |
| `content_version` | Integer 1–999999, default 1; not a migration hook |
| `weight` | Integer 1–100, default 1 |
| `family`, `family_weight` | Namespaced group, integer 1–100, default 1 |
| `requires` | Voucher only, previously registered parent voucher ID |
| `reward` | Integer 0–999; required for contracts, optional for Ante tricks |
| `max_once` | Optional boolean, Ante tricks only |
| `deck` | Existing supported Back for `set_deck` only |
| `pool`, `choose` | Legacy packs: `props`/`tricks`, choose 1–`value`; [version 2 also supports vouchers/mixed](API_130.md#pack-pools-capability-2) |
| `pack_version`, `pack_expandable`, `max_options` | Version 2 normalizes to expandable, maximum 6; see [pack contract](API_130.md#pack-pools-capability-2) |
| `bet_tier` | Optional contract difficulty: `easy`, `normal`, `hard`; otherwise Other |
| `copy_safe`, `generates_items` | Explicit Reprise opt-in and item-generator exclusion; see [metadata](API_130.md#bet-difficulty-and-reprise-metadata) |

Explicit `atlas` takes precedence over `art`. Register custom atlases first.
Native atlases include `Joker`, `Tarot`, `Voucher`, `Booster`, `centers`. Static
positions are copied. Referencing installed game artwork does not bundle it or
grant redistribution rights. Generated centers use `sstreak_item_<id>` in the
`Showstreak` description set and are omitted from ordinary native item pools.

All members of a family must agree on `family_weight`; a group weight without
a family fails. A family is a single group in weighted selection, followed by
weighted member selection. It is not a separate shop category. Native entries
keep their established ordering; external items sort by ID.

### Effects and lifetimes

The nine resource effects are `money`, `hands`, `discards`, `hand_size`,
`joker_slots`, `consumable_slots`, `deck_size`, `shop_slots`, `booster_slots`.
Values are integer deltas -999–999. Run effects also include `blind`, `boss`,
`prices`: finite fractional changes, e.g. `0.15` means +15%. Item percentage
definitions do not enforce the condition tier range; choose sensible values and
test resulting bounds. Contributions to one modifier add, rather than compound.

| Kind | Effects | When it acts |
| --- | --- | --- |
| `prop` | Run effects, supported custom effects | Manual Use prepares next run |
| `contract` | Run effects, supported custom effects | Accept prepares next run and win reward |
| `voucher` | Run effects, supported custom effects, extras below | Purchase, until accepted Intermission/series end |
| `trick` | Fixed actions below | Manual Use performs an immediate action |
| `pack` | `pack` | Open, select rewards using the pack's receipt version |

For version-2 packs the receipt follows the selected kind: Props/Tricks go into
inventory, vouchers activate immediately, and Bets are accepted immediately.
Selections cost no additional stars and recheck capacity and eligibility. A
blocked choice is not consumed. See the [version-2 receipt contract](API_130.md#pack-pools-capability-2).

Props/tricks enter held inventory when bought or taken. Purchase does not use
them. Contracts are free to accept; their reward requires the relevant win.
The host assigns `lifetime`, `activation`, `visual`; overriding those fields
does not create a new lifetime contract.

Voucher extras: `reward`, `held_slots`, `free_reroll` use integers 1–999;
`reveal_act` uses 1; `casting_reveal` uses an exact position 2–7. Position 1 is
already visible. Casting reveal is eligible in Casting series and follows the
normal voucher reset at Intermission. A prerequisite permits its upgrade only
after the parent is owned. Both owned entries contribute; the chain is cumulative.

Tricks: `reveal`, `reveal_all`, `deck`, `set_deck`, `remask`, `remove_condition`
all use value 1; `stake` uses -1; `ante` uses a nonzero integer -15–15. A named
`set_deck` requires a supported `b_...` Back; a modded Back needs its adapter.
It is eligible only when the target deck is enabled for the series and differs
from the next deck. It retains the next seed without drawing the deck bag again.

For legacy packs without `pack_version`, `value` is offered count 1–5. `choose` is an integer 1–`value`. The `props`
pool contains eligible props and tricks; `tricks` contains only tricks. There is
no arbitrary pool field or nested custom pack generator.

Item-specific codes: `kind_effect`, `price`, `value`, `reward`, `requires`,
`max_once`, `content_version`, `art`, `atlas`, `deck`, `deck_adapter`, `weight`,
`family`, `family_weight`, `pack`.

<!-- sdk-check: hook -->
```lua
assert(api.register_item {
    id = 'showstreakexample_manual_hand', kind = 'prop',
    effect = 'hands', value = 1, price = 2, art = 'j_joker',
    loc_txt = {
        ['en-us'] = {name = 'Spare Hand',
            text = {'One extra hand each round', 'during the next run'}},
        ru = {name = 'Запасная рука',
            text = {'Ещё одна рука каждый раунд', 'в следующем забеге'}},
    },
})
```

## 5. Native deck adapters

`register_deck` fields: `key`, `provider`, `version`, `baseline`. Key must be an
already registered native Back matching `^b_[%w_]+$`; a key cannot be adapted
twice. Version is integer 1–999999, default 1. This API describes a Back's starting
resources; `SMODS.Back` implements the actual bonus and localization.

The baseline requires **exactly all nine resource fields**. Integers: money
-999–999; the other eight 0–999. Ante is not a baseline resource.

A static table describes factory starts before stakes. Showstreak adds each
custom rule start's delta from the factory default. A deck with two extra hands
has factory hands 6; a custom base of 7 therefore previews 9 before stake effects.

A callback `baseline(rules_copy)` instead returns the final complete baseline
before stakes. It must account for explicit custom starts itself. It may be
called repeatedly for previews: no gameplay mutation, saves or RNG draws. Invalid
output raises an error identifying the deck and provider. Codes include
`deck_adapter`, `deck`, `baseline`, `version` and common registration codes.

```lua
-- Callback body illustration; use it in a real registered Back adapter.
baseline = function(rules)
    local s = rules and rules.version == 2 and rules.start or {
        money = 4, hands = 4, discards = 3, hand_size = 8,
        joker_slots = 5, consumable_slots = 2, deck_size = 52,
        shop_slots = 2, booster_slots = 2,
    }
    return {
        money = s.money, hands = s.hands + 2, discards = s.discards,
        hand_size = s.hand_size, joker_slots = s.joker_slots,
        consumable_slots = s.consumable_slots, deck_size = s.deck_size,
        shop_slots = s.shop_slots, booster_slots = s.booster_slots,
    }
end
```

## 6. Conditions and masks

`register_mask` is an alias over the same condition registry. A mask is the
Act presentation of a condition, not a second independent effect. Registration
uses `id`, `provider`, `category`, `effect`, `values`, optional `minimum`,
`content_version`, `loc_txt`, `art` or `atlas`/`pos`. Unknown fields fail.

| Field | Limits |
| --- | --- |
| `category` | `economy`, `blinds`, `resources` |
| `values` | Dense 1–16 nonzero finite tiers |
| Resource tier | Integer -999–999 |
| `blind`, `boss`, `prices` tier | Fraction -0.95–10 |
| Ban tier | Integer 1–999 |
| Custom tier | Registered handler's numeric contract |
| `minimum` | Resource only; money -999–999, other resources 0–999 |
| `content_version` | Integer 1–999999, default 1 |

Native ban effects are `ban_jokers`, `ban_consumables`, `ban_vouchers`,
`ban_packs`. They participate only with Casting enabled. Counts describe the
engine's eligible object bans, not wildcard identifiers. Custom effect names
beginning with `ban_` do not acquire native ban behavior.

Higher condition tiers become eligible with local Act progression, up to the
available values; the ordinary tier limit is `max(1, floor(Act / 2))`. The engine
also checks resource floors and other eligibility. Temporary random conditions
use tier 1. Do not rely on a numerically sorted `values` array: tiers are authored
positions. Accepted masks, temporary conditions, reveal/remask, removal and
Intermission use the existing condition engine for registered definitions too.

`minimum` contributes to the condition's resource eligibility floor, including
later purchase/deck-change checks while that condition is active. It is not
a callback or a clamp that repairs arbitrary external native values.
Default artwork is `j_joker`.
`loc_txt` supports `{name,text}` or localized entries with `en-us`/`default`.
Name is 1–200 bytes; text is dense 1–12 strings, each at most 1000 bytes. Condition
variables are `#1#` signed value and `#2#` absolute value. Fractional native effects
use the existing localized ratio wording.

Codes include `category`, `effect`, `values`, `minimum`, `loc_txt`,
`content_version`, `art`, `atlas`, `center`, plus common codes.

## 7. Custom effect definitions

Use built-in effects for ordinary adjustments. Use `register_effect` when
gameplay needs a distinct deterministic run-start operation. Supported item
kinds are `prop`, `voucher`, `contract`; conditions can opt in. Custom instant
tricks and arbitrary native lifecycle hooks are not provided by this contract.

| Field | Contract |
| --- | --- |
| `id`, `provider` | Namespaced effect, enabled provider |
| `version` | Integer 1–999999, default 1 |
| `kinds` | Boolean map of prop/voucher/contract; required |
| `conditions` | Optional boolean; permits custom condition tiers |
| `value` | `{min, max, integer?}`; finite -1e12–1e12, min ≤ max |
| `loc_txt` | Optional same localized shape as conditions |
| `apply` | Required `function(value, context)` |
| `resume` | Optional `function(value, context, saved_state_copy)` |
| `preview` | Optional `function(value, context)` |

At least one kind must be true or `conditions=true`. Each item value and condition
tier must meet this contract. Active contributions are summed per effect ID;
the aggregate can exceed individual-definition bounds. A zero aggregate runs
no handler. Effect IDs run in sorted order. Items/conditions record the handler's
provider and exact version. Only serializable effect metadata is frozen.

All callback code lives in the loaded provider registry, never in save data.
Bump effect `version` when behavior or saved callback state changes incompatibly.
A missing provider or exact version mismatch prevents silent continuation.
Codes include `kinds`, `conditions`, `value`, `version`, `apply`, `resume`,
`preview`, `loc_txt`, `field`, plus common codes.

## 8. Custom effect execution and preview

| Context field | Meaning |
| --- | --- |
| `series_id`, `run_id`, `seed` | Current series/run identity and run seed |
| `locale` | Normalized language code |
| `rules` | Independent copy of finalized series rules |
| `phase` | `preview`, `start`, or `resume` |
| `token` | Opaque stable series/run/effect operation token |
| `random(upper)` | Independent deterministic integer 1–upper |
| `game` | Live native game handle, apply/resume only |

`upper` is an integer 1–2147483646. Each effect has its own seeded stream. A
preview starts a fresh stream at the same initial seed as apply; use the same
draw order to predict the result. It does not consume campaign or Balatro RNG.
Apply saves its cursor. A token helps an external idempotent operation recognize
the same run; do not parse its string representation or treat it as an API key.

`preview_effect(id, value, campaign?)` returns
`{id,provider,version,value,name,text}` or `nil, code`. It defaults to the active
campaign. Preview is synchronous and pure; receives no live game, campaign object,
hidden mask or future offer. Return `{name=string,text={...}}`, name 1–200 bytes,
dense 1–12 strings of at most 1000 bytes. Without a preview callback, frozen
`loc_txt` is used and `#1#` becomes the aggregate value. Do not reveal private
future state through text. Errors include `effect_contract`, `effect_value`,
`effect_preview`. `get_effect_info` returns copied `id`, `provider`, `version`,
`kinds`, `conditions`, `value`, `loc_txt` metadata, or nil with `id`/`effect`.
It contains no callbacks. A no-campaign authoring preview may have no operation
token or native run ID; do not require a live run just to format its text.

Apply occurs synchronously after native initialization and ordinary Showstreak
adjustments, before queued generic starting addons. Return nil or a plain acyclic
state table. Only serializable finite data belongs in that table. The host saves
it with schema/version, token, bound value, RNG and completion status.

The host checkpoints before applying and after success. Native disk writes are
asynchronous; submission is not a durable storage acknowledgment or a transaction
across arbitrary external side effects. A completed grant is not replayed. A
valid early save captured before application can finish the pending first apply.
An already-started incomplete/failed operation is blocked rather than replayed.

On restore, a completed effect can receive one optional resume call per restored
game object. It receives a copy of saved state. Return nil or true; false or an
exception fails continuation. Reattach transient observers only: the native save
already contains grants and resources spent since then. Never grant again.

These callbacks are trusted installed Lua mod code, **not a sandbox**. The host
cannot prevent a callback from reaching globals, foreign RNG or unrelated files.
The provider remains responsible for native invariants and cleanup of its own
transient observers. There is no general rollback/migration callback.

## 9. Deny sources and Banner

`register_ban_source` accepts exactly `id`, `provider`, optional `version`
(integer 1–999999, default 1), and exactly one of:

- `keys`: copied map of object key → boolean; key length 1–256 bytes;
- `is_banned(key, context)`: synchronous predicate returning a boolean.

Context contains copied public `series_id`, `run_id`, `phase`, `rules`.
Keep predicates pure, fast and independent of draw order. A predicate error,
nonboolean return or recursive query of its own source denies the queried object
and records `ban_source_error`. False only means this source does not deny.

`query_object_ban(key, campaign?)` returns `{key,banned,reasons}`. `reasons` is
an array of `{id,provider?,code}`. Codes: `banner`, `source`, `ban_source_error`,
`external`, `locked`, `condition`. Invalid inputs return `nil,'key'` or
`nil,'campaign'`. The active campaign is the default.

Banner, registered sources, native external restrictions, Casting locks and
conditions combine by **deny union**. No source's allow value overrides another
source's deny. Integration with installed Banner 1.2.1 uses its real configuration
and preserves Showstreak bans when Banner recomputes its native ban table.
Turning Banner's own ban off removes only Banner's reason.

This filters supported Showstreak/native acquisition paths; existing owned cards
are retained. It is not a universal interception guarantee for a foreign mod that
creates objects through an unrelated path. Test the actual acquisition methods
your mod uses. A missing required source or changed source version blocks a saved
series that froze that source contract. Old series without source metadata are
not silently rewritten with a new frozen source manifest.

## 10. Generic starting addons

Use this for an additional category of content chosen before a run. Sleeves and
Partner have built-in adapters; do not replace their IDs. New category flags live
in `rules.start_addons[id]`. The setup UI exposes registered categories. Built-in
modes keep them off; enabled start addons make the rules Custom and do not earn
built-in Dark Stars.

| Field | Contract |
| --- | --- |
| `id`, `provider` | Namespaced ID, at most 96 bytes |
| `version` | Required integer 1–999999 |
| `name`, `description` | Required string or localized string map |
| `available` | Required `function(context)` → true when usable |
| `catalog` | Required `function(context)` → dense entry array |
| `display` | Required `function(entry_copy, context)` → description |
| `apply` | Required `function(entry_copy, context)` |
| `resume` | Optional `function(entry_copy, context)` |
| `starting_joker_count` | Optional integer 0–999, conservative maximum occupied starting Joker slots; see [1.3.0 rules](API_130.md#starting-additions-declared-joker-occupancy) |

Localized string maps require `en-us`. Strings are nonempty, at most 600 bytes,
without forbidden control characters. These are strings, not native card
`{name,text}` tables. Callback context uses `campaign_id`, `run_id`, `deck`,
`stake`, `seed`, copied `rules`, `language`. Apply/resume additionally receive
`game`, `token`, `phase='start'/'resume'`. Note the deliberate field-name
difference from effect context (`series_id`, `locale`).

An omitted `starting_joker_count` is unknown, not zero. A selected addon with
unknown saved occupancy cannot combine with Blue Skittles or Red Brain; None
remains available. A later declaration does not backfill older frozen series.

### Catalog and choices

Catalog is called when a new enabled series freezes its entries. Return a dense
array of at most 512 `{key, data?, center?}` records. Key is unique, at most 128
bytes, matches `^[%w_%-]+$`, and cannot be `none`. Data is plain serializable
content. Center, if present, is an existing native center key used for art and
ban checks. No other entry fields are accepted. Entries sort by key before
freezing. The provider/version and localized category labels are frozen too.

The host draws up to five eligible distinct entries per run from that frozen
catalog. The player selects one, or the explicit None option. It never silently
chooses a random addon for the player. Smaller pools show fewer choices; an empty
pool resolves to None. Reloading preserves the offer and selected key. Newly
registered entries do not enter that older series. Ban checks apply to the
entry's center when supplied, otherwise to its key.

Display returns exactly `{name, description?}`, each string or localized string
map. It receives copies and no live game. Repeated previews must not mutate game,
catalog or RNG. The host uses `center` from the frozen entry for native art; do
not return `center`, UI nodes or live card objects from display.

### Start and resume

Apply is queued after native setup; receives the confirmed frozen entry. Return
nil/true on success; false or an exception fails. The host records an `applying`
guard and checkpoints before callbacks, marks `applied` after success and saves
again. A saved `pending` selection can finish its first apply after restore;
`applying`/`failed` cannot be silently replayed. Completed entries get only an
optional resume callback per restored native game object. Resume restores
ephemeral behavior and must not repeat grants. A callback return of false fails.

The opaque token is stable for series/run/category. Native save submission is
asynchronous; callbacks requiring external persistence must also honor that
token. There is no distributed transaction or arbitrary effect rollback.

Version is an exact saved contract, not just diagnostic metadata. Missing addon,
provider, center, incompatible version, malformed catalog or bad saved choice
blocks continuation with an explanation. Relevant codes: `name`, `description`,
`version`, `available`, `catalog`, `display`, `apply`, `resume` at registration;
`starter_missing`, `starter_catalog`, `starter_display`, `starter_apply` at use.

## 11. Showman reactions

`register_showman_reaction` accepts only `id`, `provider`, `event`, `match`,
`priority`, `once`, `text`, `compact_text`, `presentation`, `family`, `topic`.

| Field | Contract |
| --- | --- |
| `event` | Known event or exact `ProviderID:custom_event` |
| `match` | Public field → exact string/boolean/finite number |
| `priority` | Integer 1–90, default 50 |
| `once` | Optional boolean; selected visible reaction once per profile |
| `text` | Language → dense 1–4 plain source strings; en-us required |
| `compact_text` | Optional same shape, shorter authored fallback |
| `presentation` | Optional expression/gaze/delivery names |
| `family`, `topic` | Strings ≤80 bytes, `^[%w_:%-]+$` |

One language's source totals at most 140 Unicode codepoints. No braces, CR or LF
inside source strings: speech is plain text, not Balatro colored card markup.
Runtime substitutions may lengthen a line. The renderer measures width and
reflows text; source arrays are not fixed screen lines. Compact text is used
when the full authored line cannot fit. Supply genuinely shorter translations.

`get_showman_info()` exposes `version=2`, event membership, `match_fields`,
`placeholders`, expressions/aliases, delivery settings, gazes, source limits.
Use this metadata when building authoring tools; not every event has every field.

Match fields: `item_id`, `source`, `effect`, `kind`, `lifetime`, `value`, `reward`,
`price`, `stars`, `free_slots`, `deck`, `stake`, `ante`, `cost`, `paid`, `rerolls`,
`remaining`, `native_key`, `native_set`, `native_action`, `wins`, `reason`,
`amount`, `dark_amount`, `has_active`, `record`, `act_reward`, `act_complete`,
`won_bet`, `offered`, `picks`, `selected`. Match fields are exact scalar equality;
callbacks and hidden-state conditions are not accepted.

Substitutions use `#field#`. Display-only `#item_name#`, `#deck_name#`,
`#native_name#` resolve localized visible names and are not extra match fields.
Missing substitutions become `?`. Only whitelisted facts are forwarded; strings
are sanitized/limited, nonfinite numbers removed, and current public series facts
can replace supplied fields. Do not publish unrevealed information yourself.

Common action events include `buy`, `voucher`, `contract`, `pack`, `take`, `use`,
`reveal`, `remask`, `mask`. Use `get_showman_info().events` for the complete current
set. A prop purchase and its use are different moments. Failed actions do not
count as completed action events. Custom event names start with the **exact**
provider ID and colon; allowed characters are alphanumeric, `_`, `:`, `-`.

`showman_event(event, data)` emits after the real action succeeds. It is a speech
request: player preferences, current context, priorities and repetition controls
decide whether/when the line becomes visible. No guaranteed interruption or
gameplay acknowledgment is returned. Its result may be the new speech event,
an already active event, or nil; treat that object as host-owned and read-only.
Use several distinct matching reactions
instead of repeatedly emitting one event to force speech.

### Presentation and preview

Canonical expressions: `neutral`, `pleased`, `curious`, `questioning`,
`concerned`, `joyful`, `playful`, `surprised`, `awkward`, `embarrassed`, `relaxed`,
`thoughtful`, `confident`, `skeptical`, `annoyed`, `crying`, `calm`, `uncertain`,
`serious`, `angry`, `smirking`. Metadata also lists supported aliases; aliases
reuse an existing pose and are not additional drawings.

Gaze: `center`, `left`, `right`. Deliveries: `calm`, `bright`, `soft`, `question`,
`dry`, `measured`, `hushed`, `firm`, `celebrate`. Delivery metadata includes pace,
pause, hold, pitch and volume. A registered line selects names rather than
injecting animation callbacks or arbitrary numeric delivery fields.

`preview_showman_reaction(id,data?)` returns `key`, `text`, `lines`, `layout`,
`presentation`, or `nil,'id'`. Layout includes the fit result and whether compact
text was used. It does not speak, consume gameplay RNG or mark the reaction seen;
it does update the render cache. Verify the longest translated substitutions at
the actual UI scale. Cosmetic speech does not become a required gameplay provider.

Codes include `event`, `priority`, `once`, `text`, `compact_text`, `match`,
`presentation`, `family`, `topic`, `field`, and common registration codes.

## 12. Optional artist frame maps

`Showstreak.visuals.configure_host_sprites(def)` changes the map used by new
hosts. It accepts `atlas`, integer `columns`/`rows` 1–64, and `expressions`.
Register the atlas separately using SMODS. Frames are zero-based row-major
indices within declared bounds. Supply a `neutral` expression with valid `idle`.

Each expression allows `idle`, `left`, `right` frame integers and `blink`,
`variants`, `talk`, `talk_left`, `talk_right` dense sequences of 1–8 frames.
Expression names must already be supported. Artwork should retain native 71:95
cell proportions. Missing/unavailable textures safely fall back to working art.
Blank padding in the bundled atlas cannot be addressed as new poses. This API
maps authored pixels; it does not draw, upscale or interpolate artwork.

Returns true, or nil with `definition`, `field`, `atlas`, `dimensions`,
`expressions`, `expression`, `frame`, `sequence`, `neutral`. Invalid declarations
leave the working map unchanged. This artist API is not provider-bound and does
not add a provider gameplay capability. It cannot grant rights to supplied art.

## 13. Localization contracts

| Surface | Shape / variables |
| --- | --- |
| Item `loc_txt` | Native `{name,text}` or language entries |
| Condition/effect `loc_txt` | Validated native description shape |
| Start addon labels/display | Plain string or language → string |
| Showman `text`/`compact_text` | Language → 1–4 plain source strings |

Item variables: `#1#` value; `#2#` contract/Ante reward or pack choice count;
`#3#` value accumulated through its voucher prerequisite chain. Native fractional
effects use localized ratio words. Preserve both numbered placeholders and
Balatro formatting tokens in native descriptions. Speech uses named substitutions
and cannot use braces. Generic addon callbacks must format their own frozen data.

Prefer `en-us` and explicit translated keys such as `ru`. The locale layer checks
requested/normalized/base language candidates and English fallback; do not encode
a region as a new gameplay effect ID. `content_version` does not select language.
Frozen definitions preserve serializable localization, while live provider
translations can improve display without changing gameplay values. A retired ID
can be rebuilt from frozen data while its required provider remains available.

## 14. Frozen series, compatibility and Intermission

New series freeze rules, item and condition definitions, order, deck baselines,
relevant effect metadata, source contracts and enabled starting catalogs. Do not
insert new definitions into an existing series. Save files contain metadata and
data, not arbitrary provider functions. Required providers include those in the
frozen gameplay catalog, not only items the player has bought.

`get_catalog()` defaults to the active frozen catalog and otherwise returns the
live content catalog. It contains `items`, `order`, `conditions`,
`condition_order` and optional `effects`. Definitions are keyed by stable ID;
order arrays describe catalog order. Do not require `effects` in an old catalog.
The result is copied: editing it does not register anything.
Retired item/condition IDs can render from the frozen definition, including
temporary conditions. Missing artwork uses native fallback; missing executable
provider behavior cannot be reconstructed from a tooltip.

Mod version strings are diagnostic; they do not alone establish compatibility.
Deck adapter, ban source, custom effect and generic addon versions carry their
own saved contracts and exact checks where required. Keep the contract version
when making compatible cosmetic changes. Change it honestly for incompatible
semantics; there is no universal migration or code-from-save loader.

Accepting an offered Intermission keeps total win streak, series identity,
profile Dark Stars, settlement history, frozen rules/catalog and provider
contracts. It clears held items, owned vouchers (including Casting peeks),
conditions and next-run preparation; Gold Stars and local Act/Stake progression
reset to rule-defined starts. A new deterministic segment seed supplies future
draws. A registered effect follows its contributing item's/condition's normal
lifetime. It does not gain a bypass around this reset.

Required format or compatibility-metadata updates now use an explicit, backed-up
migration to schema 7. Existing catalogs remain frozen unless the player accepts
the separate additive content update between runs. See [1.3.0 notes](RELEASE_130.md).
Providers must not bypass these steps by rewriting saves or removing required content.

Foreign gameplay content and enabled starting addons affect eligibility for
built-in Dark Stars. Cosmetic Showman additions and deny sources alone do not
blanket-disable the reward. The player's setup UI reports actual eligibility;
an integration must not hardcode its own promise of Dark Star earnings.

## 15. Reports, testing and distribution

`provider_manifest()` returns `{providers,mods}`; provider entries include version
and registered capabilities. `mods` maps enabled loaded mod IDs to their version
strings. Provider capabilities include registrations such as `items`, `decks`,
`conditions`, `effects`, `start_addons`, `ban_sources`, `showman`, `integration`;
these per-provider flags are booleans, unlike numeric API capability revisions.

`mod_report()` returns rows sorted by ID with `id`, `name`, `version`, `loaded`,
`integrated`, `capabilities`, `changed`, `missing`. It separates real mods,
including Lovely-only content, and compares against the active saved manifest.
A currently disabled registry entry can be changed without being marked missing;
missing means absent from the current registry but present in the saved manifest.
`environment_report()` returns sorted `{id,name,version}` rows for Balatro and
loaders separately. `deck_provider(key)` returns its registered adapter provider
or `Showstreak` when no adapter exists; it is not a validation of the native key.
A badge
means an integration registered supported content, not universal compatibility
with all unrelated hooks from that mod.

Test both initialization orders, invalid definitions, actual shop acquisition
and Use, next-run application, saved-run resume, Intermission, removed provider,
changed contract version, longest localized text and relevant native pools.
Headless tests validate contracts; native acceptance validates rendering and
engine integration. Neither proves compatibility with every possible mod set.

The supplied SDK is for learning/local testing. Ship an independently written
integration as a separate mod depending on Showstreak. Do not bundle Showstreak
runtime, assets, docs, supplied examples or player saves. The existing limited
terms, not an open-source license, govern these files: see [LICENSE](../LICENSE).
