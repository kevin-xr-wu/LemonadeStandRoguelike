---
name: pixelwright
description: Our rules for making and changing World Lemonade Stand art with the Pixelwright pixel-art connector, from the first pixel to the sprite in the game. Use for any sprite, prop, icon or tile, any Pixelwright tool call, and any request for an "asset" or "art" for the game, including when someone says "playwright connector" but wants art (Playwright is the browser tool; Pixelwright draws).
---

# Pixelwright playbook

How Claude makes game art in Pixelwright and puts it in `world-lemonade-stand.html`. Pixelwright's own guides (`guide_get`) teach pixel-art technique; this playbook adds our rules. Git and PR steps follow `github-playbook`.

## The project

- All game art lives in one Pixelwright project, **World Lemonade Stand** (`prj_af777136694500f7`), so every asset shares one palette. Don't start another project for game art.
- Assets so far: **Lemonade Stand** (`spr_e52075dc`), 64×64, layers Shadow, Stand, Sign, Props, Awning.
- Name each asset after what it is in the game, in title case ("Lemonade Stand").
- Before the first drawing step, give kevin-xr-wu the studio link from `studio_open`, so he can watch it come together.

## Palette

The palette comes from the game's CSS colour tokens, so the art matches the UI. Draw only with these:

| Ramp | Steps, dark to light | Game token |
| --- | --- | --- |
| `outline` | #2b2118 | `--ink` |
| `wood` | #4a2a1c #7a4428 #a86a3a #d39a5c | none |
| `lemon` | #c9780a #f5b301 #ffd83d #fff3a0 | `--lemon2`, `--lemon` |
| `awning` | #8e2a2e #c23b2e #e4572e #f28a5b | `--red` |
| `paper` | #c9a77a #ead6a8 #fff7e0 | `--paper` |
| `leaf` | #2e7d46 #58b368 #9be08a | `--green2`, `--green` |
| `glass` | #3f7ea8 #7fc4e8 #d8f2ff | none |

- For a new material, add a ramp with `palette_update` (3 or 4 steps, dark to light). Start from a game token when one fits.
- Don't change existing colours. Every asset drawn with them changes too.

## Drawing

- Draw at the size it's shown. The game shows sprites at 2× on desktop and 1× on screens 720px wide or less, so a sprite shown 128px wide on desktop is 64px wide in Pixelwright. One pixel size keeps sprites looking like a set.
- Light from the top left. Outline in `outline`, 1px.
- Draw in the studio, one part per write, so kevin-xr-wu can follow along. Working out geometry in a script first is fine.
- Don't use layer opacity or blend modes for anything the game needs. The PNG export drops or snaps them: the stand's 35% ground shadow came out missing. The game gives sprites a soft CSS drop shadow instead.
- Note how many empty rows sit under the sprite. You'll need it to put the sprite on the ground.

## Checking

- `render_preview` it on a sky colour from `.sky.*` in the game, at zoom 2 or 3, to judge it at game size.
- Run `project_audit` and fix what it finds. Accept a finding by key only when it's on purpose (the stand's awning fold, a 1px straw with no outline), and say which ones you accepted.

## Exporting

- `export_build` with preset `png` and scale 1. The CSS does the scaling.
- The download link lasts an hour, so save the PNG to the scratchpad right away.

## Putting it in the game

- The game is one file with nothing to install. Embed the PNG as a base64 data URI in a constant next to `STAND_PNG`, with a one-line comment, and use it as `<img class="…" src="${CONST}" alt="">`.
- In CSS, give it `image-rendering:pixelated` and a width of 2× its pixel size. Set 1× in the `max-width:720px` and short-landscape (`max-height:500px`) blocks.
- Anything that stands in the scene stands on the grass, the bottom 14px of `.sky`. Never leave it floating. Count the sprite's empty bottom rows when you set `bottom`.
- When a sprite changes size or place, move what's anchored to it too: the menu sign, customers, coins and speech bubbles.
- Before pushing, look at the scene at desktop, phone (390px) and short landscape sizes. Nothing should cover the menu sign, the location box or the weather icon.
- Link the studio project in the PR as the editable source: https://studio.pixelwright.app/?cloud=prj_af777136694500f7

## Changing art

- The source of truth is the Pixelwright asset, not the base64 in the game. Edit the asset, export again, and replace the data URI.
