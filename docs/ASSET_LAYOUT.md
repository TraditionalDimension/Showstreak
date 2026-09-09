# Showstreak 1.0.0 artwork and sound layout

This describes Showstreak 1.0.0. Sources and attribution are listed
in [CREDITS.md](../CREDITS.md); conditions for using Showstreak's files are in
[LICENSE](../LICENSE). This layout reference does not grant permission to republish
the artwork. See KNOWN_ISSUES.md for the tested release scope.

## Showman expressions

Current runtime file: `assets/1x/emotions.png`.

- PNG with real transparency, **568 × 95 px**.
- One horizontal row of **8 cells**, **71 × 95 px** each.
- `assets/2x/emotions.png`: **1136 × 190 px**, exact nearest-neighbour 2× copy.
- Keep the head, hat and shoulders aligned between cells. Each cell is a complete
  character image; the runtime does not overlay eyes or a mouth.
- The game displays the same 3.08 × 4.18 canvas regardless of expression.

| Cell, left to right | Code index | Expression |
|---|---:|---|
| 1 | 0 | Idle, eyes open, mouth closed |
| 2 | 1 | Half blink |
| 3 | 2 | Eyes closed |
| 4 | 3 | Temporary gaze variant |
| 5 | 4 | Temporary gaze variant |
| 6 | 5 | Brief event expression |
| 7 | 6 | Brief event expression; transition out of frame 7 |
| 8 | 7 | Brief event expression; always exits through frame 6 |

The shared controller is `scripts/showman/animation.lua`. It uses these eight
existing frames and targets frame 0 for **65% of ordinary visible time**; the
proportion is a pacing target over time, not a quota for every short interval.
Blinks follow `0 → 1 → 2 → 1 → 0`. Frames 3 and 4 are temporary looks, while
frames 5, 6 and 7 are brief reactions. Every gesture returns to idle, directly
or through a blink. A transition away from frame 7 always displays frame 6 first,
including when another event arrives.

Speech and expression duration are independent: after a brief reaction, Showman
returns to idle even while its text remains visible. Cosmetic timing uses its own
random generator and does not consume gameplay RNG. Reduced motion keeps a quiet
frame-0 idle with brief event reactions, without recurring blinks or looks; the
`7 → 6` exit rule still applies.

Current figure bounds within a cell are approximately x=17..53, y=18..77.
These bounds are a drawing reference, not an engine crop: all 71 × 95 pixels
are rendered. Keep the overall visible size comparable if enlarging details.

These eight existing frames are the selected scope for the first release.
Additional mouth poses or a larger expression atlas are not required by this
layout. Replacing the two PNGs requires a full game restart; a changed atlas must
be reviewed in the actual game, including its small portrait sizes.

## Showman voice and feedback

`scripts/showman/audio.lua` reads `resources/sounds/voice1.ogg`, `voice4.ogg`,
`voice7.ogg` and `voice10.ogg` from the player's installed Balatro during sound
registration. It makes mono voice variants in memory using 0.85 playback-speed
resampling, a 2400 Hz low-pass filter, soft saturation and short fades. The WAV
data is passed directly to the sound manager as FileData; no voice file is written
to disk or included in the player archive. If preparation fails, the matching
unmodified native voice is used.

Only `assets/sounds/purchase.wav` and `assets/sounds/ready.wav` are shipped. They
are synthesized Showstreak feedback cues. Showman voice registration and the
feedback cues leave the global Jimbo voice unchanged and use the normal game
sound settings together with Showstreak's voice/effect preferences.

The historical development importer created processed voice files on disk. That
workflow is not part of the player distribution and must not be used to rebuild
its runtime assets.

## Currency stars

| Artwork | 1× PNG | 2× PNG | Runtime atlas |
|---|---|---|---|
| Gold Star | `assets/1x/star.png`, 24 × 24 px | `assets/2x/star.png`, 48 × 48 px | `sstreak_star` |
| Dark Star | `assets/1x/dark-star.png`, 24 × 24 px | `assets/2x/dark-star.png`, 48 × 48 px | `sstreak_dark_star` |

Each PNG contains one complete transparent sprite. The Dark Star uses the author's
prepared artwork at both texture scales and represents the current profile's
persistent balance on the movable menu panel and the Lore screen. Gold Stars
remain the currency of the active series. Keep both resolutions aligned and use
nearest-neighbour scaling for pixel art. Replacing artwork requires a full restart.

## Other active atlases

| Artwork | 1× PNG | 2× PNG | Cell size at 1× |
|---|---|---|---|
| Sign | `assets/1x/sign.png` | `assets/2x/sign.png` | 192 × 44 px |
| Lamp states | `assets/1x/lamp-state-0.png` through `lamp-state-2.png` | Matching files in `assets/2x` | 9 × 9 px |
| Mod icon | `assets/1x/icon.png` | `assets/2x/icon.png` | 34 × 34 px |

The 2× assets have twice the corresponding width and height. The sign uses child
sprites for its lamps; reduced motion keeps them steady. The 34 px game icon is
not a 256 px distribution-platform icon.

## Legacy files and development sources

`host.png` and `showman-aura.png` at both scales, together with
`assets/shaders/host_face.fs` and `assets/shaders/marquee.fs`, are retained legacy
assets and are not registered by the current visual module. The active Showman
does not depend on them and does not composite eyes or mouths using those shaders.

Development importers and image-generation masters are absent from the player
archive. Do not run an old importer over newer hand-edited assets. The runtime
loads its image files from the mod's own `assets/1x` and `assets/2x` directories;
it does not need a design workspace, image-generation tool or online service.
