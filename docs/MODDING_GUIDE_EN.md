# Building a Showstreak integration

Use this guide with [API.md](API.md), the [1.3.0 API details](API_130.md) and [release compatibility notes](RELEASE_130.md). The downloadable PDF remains a historical API 1.1.0 baseline; this Markdown guide includes the current 1.3.0 behavior.

**English modding guide · Showstreak 1.3.0 · API contract 1, revision 1.1.0**  
TraditionalDimension

For downloads, the standalone comic editor and its project workflow, start with
[Toolkit and Lore Workshop](TOOLKIT.md). Normal player Lore is still In development.

### What changed for integration authors in 1.3.0

- Query `get_api_info().capabilities`, because the API revision remains 1.1.0.
  `pack_pools=2` adds supported voucher/mixed packs; `action_policy_sources=1`
  exposes declarations for restrictions relevant to Bets and assigned bosses.
- Give an external Bet its authored `bet_tier`, or let it appear under Other.
  Explicitly opt a safe consumable into Reprise with `copy_safe=true`; declare
  item-generating effects with `generates_items=true` to exclude them.
- Declare `starting_joker_count` on a starting category when its maximum is
  known. Missing metadata is unknown and can block a promised starting Joker.
- Save schema 7 is separate from API revision 1.1.0 and story schema 2. Required
  save migration and optional catalog updates are separate player decisions.
  Neither authorizes a provider to rewrite the saved campaign directly.

If an integration requires these additions, declare `Showstreak (>=1.3.0)`
and check the relevant capability. The complete teaching example below uses
the older, still-supported baseline with a minimum dependency of 1.1.0.

## 1. Choose the right extension

Showstreak connects runs into a series with its own shop, rules, Casting choices,
conditions and Showman. An integration describes content for those systems and,
when needed, supplies a versioned run-start callback. It does not replace the
native Balatro mod that implements your deck, Sleeve, Partner or other content.

Start with the smallest public extension that expresses your idea:

| Your idea | Use |
| --- | --- |
| An extra hand for the next run | A prop with built-in `hands` |
| Extra starting cash until Intermission | A voucher with `money` |
| A harder next run for a victory reward | A contract |
| A reveal or named next deck | A supported trick |
| A choice of held items | A pack from `props` or `tricks` |
| Voucher or mixed rewards | Version-2 `vouchers` / `mixed` pack |
| An accumulating Act restriction | A condition/mask |
| Distinct run-start behavior | A registered custom effect |
| A new pre-run choice category | A generic starting addon |
| A content restriction | An additive ban source |
| A restriction on actions or boss schedules | An action-policy source |
| A contextual line and expression | A Showman reaction |

Use a built-in effect for an ordinary resource change. Its existing preview,
bounds, lifetime and save behavior are already integrated. A custom effect is
useful when your behavior cannot be represented by that fixed list, not simply
as another way to add two dollars.

This guide follows the [ShowstreakExample source](examples/integration/README.md). It has no
image files: the example references resources in the installed game. It adds
gameplay content and changes the available series catalog. Use a separate test
profile. For every accepted field and numeric range, keep [API.md](API.md) open.

The example is supplied for learning and local testing under the kit's existing
terms. Your independently written integration should be a separate dependency
mod. Chapter 18 explains the distribution boundary.

<!-- page-break -->

## 2. Install the local example

Keep Showstreak 1.3.0 installed normally. The repository deliberately stores
the example as inert `.example` files. Create a separate `Mods/ShowstreakExample`
folder, copy [the manifest](examples/integration/ShowstreakExample.json.example)
and [the Lua source](examples/integration/main.lua.example) there, and remove
only their final `.example` suffixes. The result must be:

```text
Mods/
  Showstreak/
    Showstreak.json
    main.lua
  ShowstreakExample/
    ShowstreakExample.json
    main.lua
```

Do not put the example inside the Showstreak runtime folder. Do not copy the
entire Toolkit to Mods: its guides are not a second runtime installation.
Restart the game so SMODS discovers the new manifest and declarations.

The manifest has ID `ShowstreakExample`, prefix `ssex`, priority `0`, and a
dependency on `Showstreak (>=1.1.0)`. The dependency prevents this example from
pretending it can run against an older API revision. The early priority exercises
the initialization hook rather than depending on manual load-order adjustments.

On a fresh test series, look for the example content in the catalog and setup.
The Rehearsal Deck has two extra starting dollars. Opening Ticket is a new
starting category; enable it explicitly in custom rules to see its five choices.
It is off in built-in modes. Showstreak's shop is seeded, so an example item is
not guaranteed to appear in the first shop. The corresponding ID and registration
can be inspected through the API independently of a random offer.

The sample ban source denies the native Credit Card Joker within Showstreak.
That behavior is intentional and useful when testing deny union. Remove the
example folder after local evaluation if you do not want its content in future
series. Existing test series that require it will correctly report a missing
provider; start a new ordinary series instead of editing the saved catalog.

<!-- page-break -->

## 3. Create your own manifest and hook

Choose a permanent mod ID before publishing. Showstreak uses that ID for provider
ownership, namespaced content and compatibility. A display name can change
without renaming those identifiers.

```json
{
  "id": "MyStageMod",
  "name": "My Stage Mod",
  "author": ["Your name"],
  "prefix": "myst",
  "main_file": "main.lua",
  "version": "1.0.0",
  "priority": 0,
  "dependencies": ["Showstreak (>=1.1.0)"]
}
```

Create native SMODS objects before assigning your hook. A Back adapter must refer
to an already registered `SMODS.Back`; an item using your atlas needs that atlas
registered first. Then assign `showstreak_init` on your own mod object:

```lua
local mod = assert(SMODS.current_mod)
mod.showstreak_init = function(api)
    assert(api.version == 1, 'Unsupported Showstreak API')
    assert(api.get_api_info().capabilities.conditions == 1)
    -- Use api.register_item, api.register_condition, etc. here.
end
if Showstreak and Showstreak.api and Showstreak.api.ready then
    assert(Showstreak.initialize_integration(mod.id))
end
```

When you load earlier, Showstreak discovers this hook at its own initialization.
When you load later, the final conditional invokes the same once-only host path.
Do not call `mod.showstreak_init()` yourself or register the same content outside
the hook as well. Registration closes at `SMODS.booted`.

Use the facade's dot calls. It supplies your provider without changing the SMODS
current mod globally. The SDK uses this same pattern and is checked in both load
orders. A thrown initialization error stops loading with its provider diagnostic;
fix the faulty definition and restart rather than retrying a half-finished hook.

<!-- page-break -->

## 4. Read errors and protect stable identifiers

`MyStageMod` owns IDs beginning `mystagemod_`. Its SMODS prefix `myst` is separate:
a native Back might be `b_myst_reserve`, while a Showstreak prop is
`mystagemod_spare_hand`. Lowercase the provider ID, replace punctuation with
underscores, then append `_`. Do not claim another mod's namespace.

Every registration returns its ID/key on success. On failure it returns nil, a
short code and structured detail. Use the short code in tools; show the message
when a human needs to repair the definition.

```lua
local result, code, detail = api.register_item(definition)
if not result then
    error(tostring(code) .. ': '
        .. tostring(detail and detail.message))
end
```

That block illustrates error handling; `definition` must be a complete real item.
The kit's full main.lua has a shared registration helper with the same behavior.
`api.get_api_error()` returns a copy of the most recent structured diagnostic.
It includes operation, code, field, provider and ID where known. A successful
registration clears it. Older two-return-value integrations still work.

Do not mutate `Showstreak.content`, `Showstreak.api` registries or a campaign's
catalog to bypass an error. Data definitions must be serializable: finite
numbers, strings, booleans and plain acyclic tables. A function belongs only in
a documented callback field. A sparse tier array is not a compact list.

Keep published IDs stable even when improving names or translations. Do not
reuse an old ID for an unrelated effect. `content_version` records item/condition
metadata; it is not executable migration code. Custom effect/addon versions
have stricter restore implications, explained in chapter 15.

Useful early checks are `get_api_info().registration_open`, capability membership
and whether the native center or atlas exists before your registration refers
to it. These checks make a loading error actionable without making players read
internal field names in normal item descriptions.

<!-- page-break -->

## 5. Build a prop and its description

A prop is bought or taken into held inventory. The player then presses Use to
prepare its effect for the next run. Buying it does not immediately change the
current native run. This distinction should appear in your description.

The following is a complete registration inside the SDK provider hook. Change
the provider-derived prefix when writing your own integration.

<!-- sdk-check: hook -->
```lua
assert(api.register_item {
    id = 'showstreakexample_guide_hand',
    kind = 'prop', effect = 'hands', value = 1,
    price = 2, art = 'j_joker', content_version = 1,
    loc_txt = {
        ['en-us'] = {
            name = 'Spare Hand',
            text = {'{C:blue}+#1#{} hand each round',
                'during the {C:attention}next run{}'},
        },
        ru = {
            name = 'Запасная рука',
            text = {'{C:blue}+#1#{} рука каждый раунд',
                'в {C:attention}следующем забеге{}'},
        },
    },
})
```

Price is Gold Stars, not native run dollars. `value` is an integer delta for
resource effects. Native description `#1#` displays that value; colored markup
is valid here. `art='j_joker'` references an existing center's artwork. The host
creates an omitted Showstreak center, not another ordinary Joker in native packs.

The resource effects cover starting cash, hands, discards, hand size, Joker and
consumable slots, deck size and shop slots. Native `blind`, `boss`, `prices` use
fractional changes. A value of `0.15` means +15%, not a multiplier of fifteen.
Contributions to the same modifier add together. Test costs and penalties at
the strongest allowed tier and with other modifiers; validation is not a
balance designer.

Use the Run Info preview to verify the effect before starting. Then test the
actual next run, save/reload it, and confirm the prop does not grant again.

<!-- page-break -->

## 6. Vouchers, contracts, tricks and packs

Choose a kind for the player's interaction and lifetime, not for its card art.
The host assigns activation and visual family from that kind.

| Kind | The player does | What remains |
| --- | --- | --- |
| Voucher | Buys it | Active across runs until Intermission/end |
| Contract | Accepts it for free | Next-run change and victory reward |
| Trick | Holds it, then Uses it | Its immediate supported action |
| Pack | Opens, then picks | Props/tricks enter inventory; version-2 vouchers/Bets activate immediately |

Version-2 packs also grant vouchers or Bets immediately when selected. They do
not put those kinds in held inventory. The next choice rechecks prerequisites,
Bet conflicts and capacity; a blocked choice is not consumed, and selection
does not cost additional stars.

The SDK's Stage Budget is a voucher adding $2 to subsequent starts. Short Set
is a free contract with one fewer hand and two extra stars on victory. Its
description must not promise the reward merely for accepting. Rehearsal Ticket
uses `set_deck` for the next run; it requires the target deck to be enabled.

Vouchers can use the normal run effects and extras such as `reward`,
`held_slots`, `free_reroll`, `reveal_act`. A voucher's `requires` must name an
already registered voucher. Register a parent before its upgrade and verify
the cumulative chain description. Do not simulate a prerequisite by checking
live purchase state while registering.

Backstage Case is a three-offer pack from `props`, choose one. This pool includes
eligible props and tricks. The `tricks` pool restricts it to tricks. Counts are
1–5; selection count cannot exceed the offered count. The API does not expose
arbitrary pack generation callbacks or new shop categories.

In 1.3.0, use `pack_version=2` and `capabilities.pack_pools >= 2` for `vouchers`
or `mixed`. Version 2 permits one or two selections (vouchers: exactly one),
normalizes the expansion cap to six, and lets Prop Master add an offered option
without adding a selection. Mixed pools guarantee a paid option or compatible
paid pair, not only free Bets. Open choices persist across reloads. The
[API appendix](API_130.md#pack-pools-capability-2) contains a registration example.

Weights are integers 1–100. A namespaced family groups related variants before
member weighting, so adding many sibling tickets need not flood the entire
shop. Every sibling must agree on `family_weight`. Keep families about selection
frequency; they do not change ownership, lifetime or prerequisite semantics.

See the exact fixed trick values and allowed extra fields in API.md. A custom
callback cannot be smuggled into a data-only trick definition; use a registered
run effect when the intended operation belongs at run start.

<!-- page-break -->

## 7. Describe a real native deck

Registering a deck adapter does not create a playable Back. The SDK first uses
`SMODS.Back` with key `rehearsal`, native atlas art and `config.dollars=2`.
Steamodded gives it key `b_ssex_rehearsal`. The hook then registers that exact key
with Showstreak and supplies a complete starting baseline.

```lua
-- This is the SDK's factory baseline for its native +$2 Back.
baseline = {
    money = 6, hands = 4, discards = 3, hand_size = 8,
    joker_slots = 5, consumable_slots = 2, deck_size = 52,
    shop_slots = 2, booster_slots = 2,
}
```

All nine fields are required. Money may be negative within the documented range;
other baseline resources are nonnegative integers. Do not include Ante, current
round values or the player's spent resources.

A static baseline means the deck at factory starts, before stake effects.
Showstreak applies custom rule deltas automatically. If your deck has two extra
hands, factory hands is 6. When the player changes base hands from 4 to 7, the
preview should become 9, not stay 6 or accidentally become 11.

For a deck whose start depends on finalized rules, supply a deterministic
`baseline(rules_copy)` function. It must return the final complete pre-stake
baseline and account for those custom starts itself. Do not also add the static
delta. Keep this callback pure: previews may call it repeatedly. It is not a
place to shuffle cards, draw RNG or grant resources.

Test the deck in ordinary Balatro first, then in a Showstreak series using
factory and custom starts, several stakes, and a named deck trick. The adapter
improves preview and bounds checking; it cannot verify all hooks in a complex
foreign deck. Increase its adapter version if the saved baseline contract
becomes incompatible, and keep the required provider installed for old series.

<!-- page-break -->

## 8. Register conditions and Casting peeks

A condition joins the existing mask/Act engine. You provide its category, effect
and ordered tier values; the host handles offers, visibility, selection,
accumulation, removal and reset. `register_mask` names the same registration API.

<!-- sdk-check: hook -->
```lua
assert(api.register_condition {
    id = 'showstreakexample_guide_toll', category = 'economy',
    effect = 'money', values = {-1, -2, -3}, minimum = 0,
    loc_txt = {
        ['en-us'] = {name = 'Stage Toll',
            text = {'Start runs with $#2# less'}},
        ru = {name = 'Плата за сцену',
            text = {'На старте на $#2# меньше'}},
    },
})
```

This example uses the native money effect. The SDK's Tip Tax instead uses its
custom Lucky Tip handler. Categories are `economy`, `blinds`, `resources`;
values are a dense array of 1–16 nonzero tiers. `minimum` is a resource eligibility
floor, not a callback. Temporary conditions use tier 1. Higher tiers become
eligible as local Acts progress, subject to the engine's other checks.

Only Casting enables native `ban_jokers`, `ban_consumables`, `ban_vouchers`,
`ban_packs` conditions. Normal registered resource conditions work with their
ordinary lifetimes. Do not add conditions directly to a running saved table;
new registrations belong to new series catalogs.

A Casting reveal voucher uses `effect='casting_reveal'`, `value=6` to expose
the sixth offer. Positions 2–7 are valid; the first is already visible. It does
not choose a Joker for the player or add another pick. The player still chooses
three of seven. The SDK's Sixth Spotlight demonstrates this exact position.

Like other vouchers, it is removed by an accepted Intermission. Do not keep a
parallel permanent flag that secretly leaves its position visible afterward.
Test purchase, the next Casting offer, save/resume, and an accepted Intermission.

<!-- page-break -->

## 9. Define a custom run effect

Lucky Tip demonstrates a behavior that the native fixed `money` effect does not
describe: multiply an active value by a deterministic seeded integer from one
to three. Its preview and actual grant draw from the same independent stream.

Register the handler first, then reference its ID from items or conditions.
The following is a complete minimal registration under the SDK provider. Its
fixed counter is an authoring example; the executable SDK uses visible dollars.

<!-- sdk-check: hook -->
```lua
assert(api.register_effect {
    id = 'showstreakexample_guide_tokens', version = 1,
    kinds = {prop = true, voucher = true, contract = true},
    conditions = true,
    value = {min = -20, max = 20, integer = true},
    loc_txt = {
        ['en-us'] = {name = 'Admission Tokens', text = {'#1# tokens'}},
        ru = {name = 'Входные жетоны', text = {'#1# жетонов'}},
    },
    apply = function(value, context)
        context.game.example_tokens = value
        return {tokens = value}
    end,
})
```

The handler opts into item kinds and/or conditions. `value` specifies the range
for an individual definition, not the total after multiple active contributions.
Showstreak sums contributions with the same effect ID and invokes one callback
with the aggregate; zero produces no callback. Multiple handlers execute in
sorted ID order. Avoid relying on another provider's ID ordering for correctness.

Use `version` as an exact gameplay/saved-state contract. Keep the code loaded
under the same provider. Only handler metadata enters the frozen catalog;
functions never enter the save. An item captures the handler provider/version
when it registers, so registering the handler afterward cannot repair an
already rejected item.

Custom handlers support props, vouchers, contracts and conditions. They do not
create arbitrary instantaneous trick operations, new shop pools or unrestricted
save migration. For broader native behavior, your own mod owns its own SMODS
hooks and must keep them compatible with this defined start/resume contract.

<!-- page-break -->

## 10. Keep previews and saves honest

The custom effect context supplies `series_id`, `run_id`, `seed`, `locale`, a
copied `rules` table, `phase`, an opaque operation `token`, and `random(upper)`.
Apply/resume also receive the live native `game`. Preview receives no game object,
hidden mask, campaign object or future offer.

Use `context.random(upper)` for reproducible effect-local choices. Preview starts
at the same initial seed as apply without consuming campaign or native RNG.
Matching draws in matching order give matching results. Repeated previews must
not grant resources, write files, change registrations or advance a private
global random counter. The SDK's displayed Lucky Tip amount equals its actual
starting-cash change.

`apply` returns nil or a plain saved-state table. Store facts you need on resume,
such as the seeded amount; do not store cards, game objects, functions or cycles.
The host binds the saved record to token, value and effect version. It checkpoints
before application and after completion. A completed effect cannot grant again
when a native save resumes.

`resume(value, context, state_copy)` is optional. Restore transient observers or
UI handles; never repeat the opening grant. The player may already have spent
those dollars. Return nil/true on success, false or an exception to fail. A
restored game receives at most one resume callback for that completed effect.

A save captured before the first apply can complete the pending operation.
An already-started incomplete or failed operation blocks rather than guessing
whether to replay side effects. Native saving is asynchronous: a submitted
checkpoint is not a durable disk acknowledgment or cross-service transaction.
If your own external operation needs idempotency, honor the stable token too.

Installed Lua mods are trusted code. Copied context is an interface boundary,
not a sandbox preventing global access. Your provider remains responsible for
native invariants and for not performing unrelated side effects in a preview.

<!-- page-break -->

## 11. Add restrictions without an allow override

A ban source contributes a deny. It never gains final authority to allow an
object that another source denied. This is the useful compatibility rule for
Banner, Casting locks, conditions and your own integration.

<!-- sdk-check: hook -->
```lua
assert(api.register_ban_source {
    id = 'showstreakexample_guide_cash_only', version = 1,
    is_banned = function(key, context)
        return key == 'j_credit_card'
    end,
})
local result = api.query_object_ban('j_credit_card')
assert(result.banned)
```

For a static list, replace `is_banned` with `keys={j_credit_card=true}`. Exactly
one form is allowed. Predicate context contains public series/run IDs, phase and
copied rules. Return a boolean on every path. A missing return is not false;
it is an invalid predicate result and fails closed for that object.

`query_object_ban` returns the key, a boolean and a reasons array. A reason
identifies Banner, your source, an external native restriction, Casting lock or
condition. Use this information to explain availability in your own tooling.
Do not edit `G.GAME.banned_keys` directly as your integration contract.

Banner can rebuild its native table when a player changes its settings.
Showstreak's adapter reasserts its own snapshot restrictions afterward. Removing
Banner's reason does not remove another source's reason. Querying recursively
from a source's own predicate is rejected to avoid a loop.

The host filters supported acquisition paths and preserves existing owned cards.
A foreign mod with an unrelated direct object-construction path still needs
native testing. A registered badge cannot prove every foreign hook is covered.
Keep source version/provider stable for a saved series; an incompatible version
or missing required source should report an error, not silently loosen its rules.

<!-- page-break -->

## 12. Offer a new starting category

Use `register_start_addon` for content chosen before a run: a category such as
charms, tickets or another mod's starting companion. Sleeves and Partner already
have their own adapters. Do not reuse their keys or change their callbacks.

The SDK registers `showstreakexample_pocket`, shown to players as Opening Ticket.
The category is off in built-in rules. With it enabled, each run offers five
frozen entries: Copper, Silver, Golden, Velvet and Encore Ticket, granting one
through five opening dollars. This is a deliberately obvious test catalog,
not a recommendation for five equally balanced choices.

Register required `available`, `catalog`, `display`, `apply` functions and an
optional `resume`. Add a required integer contract `version`, localized category
`name` and `description`. Addon localization uses strings or language-to-string
maps, not the native `{name,text}` card format.

For a current 1.3.0 category, add `starting_joker_count` as a conservative maximum
integer from 0 to 999. Zero explicitly declares no starting Jokers; omission
means unknown. Blue Skittles/Red Brain need that capacity guarantee for a selected
category. The baseline teaching example leaves this field absent, so choose None
when testing those combinations, or author a versioned 1.3.0 category declaring
zero for its money-only grant. A new declaration does not modify old frozen series.

`catalog(context)` returns at most 512 dense records with `key`, optional `data`,
optional native `center`. Keys must be unique and cannot be `none`. Put only
plain serializable content in data. The example stores English/Russian names
and dollar amounts in that frozen data and uses `j_joker` for native preview art.

`display(entry_copy, context)` returns a name and optional description. It is
pure and can run repeatedly; no card objects, callbacks or UI node trees belong
in the return value. The native art comes from the entry's `center`. Ban checks
use that center when provided, otherwise the entry key itself.

Addon context uses `campaign_id`, `run_id`, `deck`, `stake`, `seed`, copied `rules`
and `language`. These names differ from effect context's `series_id` and `locale`.
Use the documented context for that callback rather than sharing an untested
generic helper that assumes all contexts have identical fields.

<!-- page-break -->

## 13. Preserve the player's starting choice

The host freezes an enabled addon's catalog for a new series. It draws up to five
distinct eligible entries for each next run, and saves those offers. The player
selects one or the explicit None option. Fewer entries give fewer choices; an
empty eligible pool resolves to None. Reloading must not redraw the offer or
silently select the best entry for the player.

You do not need an additional settings editor or random picker to reproduce
this behavior. Register the category; Showstreak exposes the pre-series toggle
and the next-run review. The flag is `rules.start_addons[addon_id]`. It remains
fixed after the series begins, including across Intermission. New live entries
do not enter that old frozen catalog.

`apply(entry, context)` runs after native setup through the event queue. It
gets the confirmed frozen entry, live game, stable token and `phase='start'`.
Return nil/true for success; false or an error prevents a silent incomplete
start. The host records an `applying` checkpoint before calling you, then marks
the operation applied and saves after it succeeds.

Pending saved operations can finish their first apply on restore. Already applied
ones call only optional `resume(entry, context)`, with `phase='resume'`; use it
for ephemeral observers, not a second grant. An interrupted applying/failed
record cannot safely be guessed into completion and is blocked. The same caveat
about asynchronous native saves applies as with custom effects.

If your provider becomes unavailable, its version changes, or a required center
is missing, continuation explains the problem. Do not return a different catalog
to hide missing saved content. If a callback accesses external resources, use
the opaque token for its own idempotency and preserve the normal saved choice.

Test a pending checkpoint, a completed save after spending resources, and repeated
resume of the same restored game. The SDK checks the opening dollars remain
unchanged after resume.

<!-- page-break -->

## 14. Give the Showman truthful, varied dialogue

A Showman reaction is data attached to a confirmed public event. The sample
`showstreakexample_spare_hand_bought` matches `event='buy'`, the exact item ID
and `source='buy'`. It does not claim the prop has already been used.

Use `api.get_showman_info()` to inspect current events, match fields,
placeholders, expressions and deliveries. Fields vary by event. Exact matching
supports strings, booleans and finite numbers; arbitrary callbacks and hidden
mask inspection are not part of reaction selection.

Each language provides one to four plain strings, at most 140 Unicode characters
in total before substitution. Use `text['en-us']` as the required fallback.
No braces or embedded newlines: speech is not a colored native tooltip. Named
substitutions such as `#value#` and `#item_name#` refer to visible public facts.
Missing fields display `?`; test your chosen event, not only an artificial preview.

Add multiple distinct reactions for the same situation. `family` and `topic`
help the host avoid repetitive variants. Priority is 1–90, default 50; increasing
everything to 90 does not create better timing. `once=true` means a visible
selection remembered per profile, not a once-per-run gameplay flag.

An explicit `presentation` chooses an expression, gaze and delivery. The sample
uses playful/center/bright. Aliases map to existing drawings; they do not imply
extra sprite frames. Use the artist map API only with your own authored art.

Supply `compact_text` when a long translated or substituted line may not fit.
Make it shorter while preserving the same meaning. Preview through
`api.preview_showman_reaction(id, data)` to inspect text, layout and presentation
without speaking or altering gameplay RNG. It is still worth viewing the actual
bubble at native UI scale.

For your own completed action, register `MyStageMod:encore` and emit that same
event afterward. Emission requests speech; player settings, context and repetition
controls may keep it silent. Never use a visible line as proof gameplay succeeded.

<!-- page-break -->

## 15. Understand frozen content and Intermission

A series is a saved agreement about its rules and catalog. Registration affects
new series. Once a series exists, its own item/condition definitions, order,
baselines and relevant extension metadata stay frozen. Its saved data contains
no executable provider callbacks.

In 1.3.0, a required update to save schema 7 has a preview and verified backup.
It is separate from **Rules → Add new content**, which can add compatible
missing IDs between runs. Existing definitions, rules and frozen offers remain
intact apart from explicitly previewed compatibility changes. New catalog entries
can change future random offers. Providers must not bypass either player decision.

| Change | Effect on an existing series |
| --- | --- |
| Add a new item/condition | Stays outside until the player accepts a compatible catalog update |
| Improve an available translation | Display can improve without new gameplay |
| Retire an item ID, keep required provider | Frozen definition can rebuild display |
| Remove required provider code | Continuation reports missing provider |
| Incompatible effect/addon contract version | Continuation reports incompatibility |
| Accept Intermission | Normal inventory/condition/reset behavior applies |

`content_version` is not a migration hook. A mod's overall version is useful
diagnostic metadata, while deck/source/effect/addon contracts have their own
saved version checks. Do not keep an old custom effect version while radically
changing its callback state shape. There is no automatic callback migration API
to reconcile arbitrary changes.

Intermission keeps the series identity, total win streak, profile Dark Stars,
settlement history and frozen rules/catalog. It clears held items, owned vouchers,
conditions and prepared next-run effects, and resets local progression and Gold
Stars to the applicable starts. The next segment uses a new deterministic seed.

Your Casting peek is a voucher and resets. Your custom voucher/condition effects
follow those same normal lifetimes. A generic starting category remains enabled
as a fixed rule, but its next run still receives a new normal choice and a new
operation token. Do not store a separate permanent upgrade to evade the reset.

The setup screen determines Dark Star eligibility. Foreign gameplay catalog
content or enabled start addons do not earn built-in Dark Stars. Cosmetic
Showman lines and ban sources alone do not automatically remove that eligibility.
Never promise a reward independently of the host's actual rule check.

<!-- page-break -->

## 16. Localize for players, not the implementation

Use a player-facing name and describe the action, timing and amount. “Start
with $2 extra until Intermission” tells a player what they gain. “Sets the money
field and persists a series modifier” belongs in this guide, not on a voucher.

| Surface | Write this data |
| --- | --- |
| Item or condition | Native `loc_txt` name and text |
| Custom effect | `loc_txt`, optional pure preview description |
| Starting category | Plain localized name/description strings |
| Starting entry | Localized display name/description |
| Showman | Plain lines, expression/delivery, optional compact lines |

Use exact locale keys such as `en-us` and `ru`; provide English fallback.
Regional/base-language fallback helps missing variants, but it is not a reason
to put English strings under every language key and claim a complete translation.
The SDK provides actual English/Russian text for every visible example surface.

Preserve `#1#`, `#2#`, `#3#` and native `{C:...}` tokens in card descriptions.
Item `#1#` is value, `#2#` is reward or pack choice count, and `#3#` is the
voucher-chain cumulative value. Condition `#2#` is its absolute tier value.
Custom effect fallback preview replaces `#1#` with the aggregate. Do not assume
one numbered variable means the same thing on every surface.

For speech, preserve named substitutions and keep text within its source limit.
Actual names may be longer after translation. Test the longest card/deck name,
maximum amount, narrow bubble and compact fallback. Use the preview's fit data
as a check, then inspect actual game rendering. Font width is not character count.

Keep localization data serializable so frozen items and retired condition IDs
retain meaningful text. Artwork fallback cannot restore a missing provider's
code, and a readable tooltip cannot make an incompatible custom callback safe.
Translate player-facing failure explanations in your mod without hiding the
provider/field details available to modder diagnostics.

<!-- page-break -->

## 17. Verify behavior at the right level

The kit's automated verification loads production Showstreak modules using the
game's LuaJIT runtime. It checks the SDK in both load orders and executes marked
complete tutorial registrations. Native SMODS/game objects are represented by
a small harness; that verifies API behavior, not visual engine acceptance.

For the supplied sample, the expected landmarks are:

| Test | Expected result |
| --- | --- |
| Rehearsal Deck | Native +$2 and baseline money 6 at factory starts |
| Spare Hand | Held after purchase, next-run +1 hand after Use |
| Stage Budget | +$2 starts, normal Intermission reset |
| Short Set | -1 hand, extra stars only on its win |
| Backstage Case | Three eligible offers, one pick |
| Sixth Spotlight | Exact Casting position 6 visible |
| Lucky Tip | Seeded preview equals actual opening cash |
| Opening Ticket | Five offers, chosen grant once |
| Cash Only source | Credit Card denied even if another source allows |
| Showman reaction | Correct purchase event and playful expression |

In native Balatro, repeat the purchase/use/start/resume path. Save after spending
some granted cash, quit and continue: spending must remain spent. Accept an
Intermission and verify normal resets. Enable Banner's same ban, remove its own
ban, and confirm the SDK source still denies. Also test Banner alone in ordinary
Balatro so the integration does not disturb its original behavior.

Run missing-provider and changed-version tests on disposable saves. Their
expected result is a clear block, not a silent reward replay or replacement
catalog. Test English/Russian tooltips and controller focus in the real menu.
Do not modify a live player's save to force offers just for a screenshot.

Maintain separate contract tests for pure logic and native acceptance for game
hooks. Passing either alone is not a promise of compatibility with every mod,
platform, font, audio setting or large-number library.

<!-- page-break -->

## 18. Package and maintain your integration

Ship your own independently written integration as its own mod, with stable IDs,
its own manifest, and a declared Showstreak dependency. Include your own authored
files and resource licenses. Reference installed native artwork only under the
original owners' applicable terms; the API does not grant rights to that art.

Do not bundle Showstreak runtime files, its authored sprites, supplied guides,
sample source or player data inside your distributed integration. The supplied
example is for learning and local testing, not a redistribution license. If you
want to redistribute these supplied files, obtain the author's separate
permission. The kit's [LICENSE](../LICENSE) preserves the existing limited-use
terms; this is not an open-source license.

Before releasing an update, list which contracts changed. Translation-only fixes
usually need no effect-version bump. A changed callback state shape or meaning
does. Keep an old provider available for saved series if you intend them to
continue; do not delete required registrations merely because the current shop
no longer offers that item.

Use `provider_manifest`, `mod_report` and `environment_report` to understand a
support report. An integrated badge means registered supported content; it does
not certify all unrelated hooks in that mod. Include the game/loader versions,
provider versions, exact action and diagnostic code in a reproducible bug report.
Never request a player's unrelated personal files for a routine API error.

The kit contains three complementary documents: this workflow, the Russian
workflow and the exact API reference. The installable sample is the tested full
program; short unmarked schema fragments illustrate fields and are not whole
mods. Keep your own tests aligned with the version of Showstreak you declare.

A useful final release check is simple: a player can read the item, understand
when it acts, choose it, start the run, save, return and receive exactly the
promised behavior. The API work exists to make that ordinary play reliable.
