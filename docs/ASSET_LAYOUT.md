# Showstreak 1.3.0 — artwork and sound layout

This reference describes the **1.3.0 player distribution**, whose asset files are attached to the [GitHub release](https://github.com/TraditionalDimension/Showstreak/releases/tag/1.3.0). Paths below are relative to the installed Showstreak folder; this documentation repository does not contain the runtime assets.

Sources and attribution are listed in [CREDITS](../CREDITS.md), and permissions in [LICENSE](../LICENSE). This reference does not grant permission to republish the artwork. See [known limitations](KNOWN_ISSUES.md) for the tested scope.

## Showman atlas

The active portrait files are `assets/1x/emotions.png` and `assets/2x/emotions.png`, registered as `sstreak_faces`.

| Property | 1× | 2× |
| --- | --- | --- |
| Full PNG size | 568 × 665 px | 1136 × 1330 px |
| Cell size | 71 × 95 px | 142 × 190 px |
| Grid width | 8 cells | 8 cells |
| Populated rows | 4, containing 32 authored drawings | Matching drawings |
| Remaining rows | 3 transparent rows | Matching transparent rows |

The image height includes unused transparent rows; they are not additional expression frames. Each populated cell is a complete character drawing. The current renderer does not build expressions by compositing eyes and mouths over a base portrait.

The mapping is defined in `scripts/showman/animation.lua`. Frame indices are zero-based, proceeding left to right and then down:

| Frames | Use |
| --- | --- |
| 0–4 | Neutral, blink and gaze drawings |
| 5–7 | Pleased, curious and concerned |
| 8, 12, 24 | Joyful expression and its matching speaking poses |
| 9–10 | Playful variants |
| 11, 26 | Surprised variants |
| 13–14 | Awkward and embarrassed |
| 15, 30 | Relaxed variants |
| 16–20 | Thoughtful, confident, skeptical, annoyed and crying |
| 21, 31 | Calm variants |
| 22–23 | Uncertain and serious |
| 25, 27 | Reserved drawings without a matching closed-mouth base |
| 28–29 | Angry and smirking |

Only the matching joyful set changes mouth frames during speech. Other expressions keep their authored drawings; speech does not swap unrelated eyes, winks or tears. Neutral blinking follows `0 → 1 → 2 → 1 → 0`. Leaving frame 7 passes through frame 6.

Animation has its own cosmetic randomness and does not consume gameplay RNG. Reduced motion suppresses recurring idle movement and speaking-mouth changes while retaining brief reactions. The Showman's presentation, speech planning and sign lights are separate systems; the 1.3.0 menu speech cadence does not speed up with game speed.

When preparing replacement art, preserve the cell grid, transparency and alignment of the head, hat and shoulders. Check both resolutions and small in-game portrait sizes after a full restart. Do not treat the old one-row, eight-frame specification as the current layout.

## Voice and feedback sounds

`scripts/showman/audio.lua` reads these sounds from the player's installed Balatro:

`voice1.ogg`, `voice2.ogg`, `voice4.ogg`, `voice5.ogg`, `voice7.ogg`, `voice9.ogg`, `voice10.ogg`.

The mod prepares complete voice samples in memory, using resampling, filtering, saturation, fades and gain adjustment. The sound manager receives in-memory data. Original or processed Balatro voice files are not included in the player archive, and the runtime does not write generated voice files into the mod folder. If preparation fails, the matching native voice is used.

Version 1.3.0 schedules complete samples with gaps between them. Delayed frames do not compress pending sounds into a burst. Showman's voice has its own preference and does not replace Jimbo's global voice.

`assets/sounds/purchase.wav` and `assets/sounds/ready.wav` are synthesized Showstreak feedback cues. They remain registered; current purchase and item reactions also use native game sound keys. Voice and effect preferences are separate.

## Currency, sign and interface atlases

| Artwork | File in `assets/1x` | 1× cell size | Runtime atlas |
| --- | --- | --- | --- |
| Ordinary star | `star.png` | 24 × 24 px | `sstreak_star` |
| Dark Star | `dark-star.png` | 24 × 24 px | `sstreak_dark_star` |
| Sign | `sign.png` | 192 × 44 px | `sstreak_sign` |
| Lamp states | `lamp-state-0.png` through `lamp-state-2.png` | 9 × 9 px | `sstreak_lamp_0` through `sstreak_lamp_2` |
| Mod icon | `icon.png` | 34 × 34 px | `sstreak_modicon` |

Matching files in `assets/2x` have twice the width and height. Keep the two texture scales aligned. The 34 px game icon is separate from any distribution-platform artwork.

Ordinary stars belong to the current series shop. Dark Stars represent persistent profile progress. The sign's lamps are separate sprites; their motion reverses direction at varied intervals in 1.3.0, and reduced motion keeps them still.

Item cards reference installed Balatro/Steamodded artwork; original game card atlases are not bundled. Showstreak item illustrations avoid Legendary Joker and The Soul artwork, with Matador as the safe fallback. Actual Jokers presented through Casting retain their own artwork and sources.

## Lore typography and author tools

Comic rendering uses DejaVu Sans. The player distribution includes `assets/lore/DejaVuSans.ttf` and its copyright and permission notice, `assets/lore/LICENSE-DejaVu.txt`. The separate Lore Workshop also uses DejaVu Sans Bold for its interface.

The font has its own license. The normal player Lore screen remains in development; font and reader support do not imply that a player story catalog has been released. See the [Toolkit overview](TOOLKIT.md) for authoring tools.

## Legacy files

`host.png` and `showman-aura.png` at both scales, plus `assets/shaders/host_face.fs` and `assets/shaders/marquee.fs`, are retained legacy resources. The current portrait/sign renderer does not use them.

Development importers and image-generation masters are not required at runtime. Do not run an old importer over the author's later drawings. The installed mod loads its own assets; it does not need the development workspace or an online image service.
