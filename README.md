# Corne 5-Column ZMK Config

Personal ZMK firmware for a wireless 5-column Corne (`nice_nano_v2` + `nice!view`).

Layout goals: keep Vim and tmux muscle memory intact, keep Space and Backspace on
plain keys so they repeat normally, and put every common programming symbol at most
two keys away.

- Keymap: [`config/corne.keymap`](config/corne.keymap)
- Firmware options: [`config/corne.conf`](config/corne.conf)
- Build matrix: [`build.yaml`](build.yaml)
- Cheat sheet overlay: [`tools/keymap-overlay`](tools/keymap-overlay) — an
  always-on-top window rendered straight from the keymap, for learning the
  layers (`./tools/keymap-overlay/zmk-cheatsheet`)

---

## Layer 0 — Base

Plain US QWERTY, with four pinky-column holds, mirrored across the hands: **`A` is
`LCtrl` and `Z` is `LShift`** on the left pinky, **`;` is `RCtrl` and `/` is `RShift`**
on the right. Every other modifier and every layer lives on a thumb. There is no `GUI`
anywhere.

The **top row doubles as the number row on hold**: `Q`..`P` send `1`..`0`. Same guard
as the pinky mods — `tap-preferred` flavor plus a 150 ms `require-prior-idle-ms`, so a
hold inside a word always resolves as the letter. The Nav layer keeps its own digit row
for runs of numbers; these holds are for one-off digits without a layer change.

```
╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│  Q/1  │  W/2  │  E/3  │  R/4  │  T/5  │   │  Y/6  │  U/7  │  I/8  │  O/9  │  P/0  │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  A/^  │   S   │   D   │   F   │   G   │   │   H   │   J   │   K   │   L   │  ;/^  │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  Z/⇧  │   X   │   C   │   V   │   B   │   │   N   │   M   │   ,   │   .   │  //⇧  │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │       │   ⇥   │  ENT  │   │  SPC  │       │ BSPC  │
                │ Shift │  Sym  │  Alt  │   │       │  Nav  │       │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯
```

In the thumb block the **upper label is the tap, the lower is the hold**. A blank row
means the key does nothing there.

Both layer thumbs sit on the **middle** thumb of each hand — `Sym` left, `Nav` right —
so the two are mirror images and Adjust is one thumb per hand. Only two thumbs are
genuinely tap/hold: `Tab`/`Sym` and `Enter`/`Alt`. The other four are single-purpose.
`Space` and `Backspace` are plain `&kp`, so they auto-repeat normally and can never
leak a layer — that is precisely why neither layer thumb sits on them. `Shift` is a
plain `&kp` as well, and `Nav` is a bare `&mo` with no tap at all, so neither has a
tapping term that could misfire.

| Thumb | Tap | Hold |
| --- | --- | --- |
| Left outer | — | `Shift` |
| Left middle | `Tab` | Layer 2 — Symbols |
| Left inner | `Enter` | `Alt` |
| Right inner | `Space` | — |
| Right middle | — | Layer 1 — Nav |
| Right outer | `Backspace` | — |

`A/^`, `Z/⇧`, `;/^` and `//⇧` are the only keys with holds outside the thumbs — tap for
the character, hold for the modifier. See **Letter-key holds** below.

**Combo:** `J` + `K` pressed together → `Esc`. Base layer only, 50 ms window.

---

## Layer 1 — Navigation and numbers

Hold the **Nav** thumb (right middle). Right hand is movement, left hand is
the tmux / pane cluster.

```
╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   1   │   2   │   3   │   4   │   5   │   │   6   │   7   │   8   │   9   │   0   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  ^H   │  ^J   │  ^K   │  ^L   │  PFX  │   │   ←   │   ↓   │   ↑   │   →   │  F11  │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│ Wprev │ Wnext │ SPLT- │ SPLT| │ ZOOM  │   │ HOME  │ PGDN  │ PGUP  │  END  │  F12  │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │  TAB  │  ADJ  │   ·   │   │   ·   │  ▓▓▓  │  DEL  │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯
```

`▓▓▓` is the thumb you are holding to reach this layer; `ADJ` is the Sym thumb —
hold that one too and you land on Layer 3.

`·` falls through to the base layer, so the thumb `Alt` and `Space` stay available
while navigating. **Thumb `Shift` does not** — the left outer thumb is `Tab` on this
layer. Neither `Ctrl` survives either: this layer remaps `A` to `Ctrl+H` and `;` to
`F11`, and `/` (the right `Shift`) to `F12`. To use `Ctrl` or `Shift` with Nav, hold
the modifier key first, let it resolve, and only then reach for the Nav thumb.

### Right hand — movement

| Movement | Keys |
| --- | --- |
| Arrow keys | `Nav` + `H` `J` `K` `L` |
| Delete | `Nav` + `Backspace` |
| Select horizontally | hold `Z` or `/`, then `Nav` + `H` / `L` |
| Move by words | hold `A` or `;`, then `Nav` + `H` / `L` |
| Select by words | hold `A` + `Z` (or `;` + `/`), then `Nav` + `H` / `L` |
| Home / End | `Nav` + `N` / `.` |
| Numbers | `Nav` + `Q`…`P` |
| F11 / F12 | `Nav` + `;` / `/` |

Move-by-word and select-by-word need `Ctrl` and `Shift`, all four of which are
overwritten on this layer. Hold the modifier **before** the Nav thumb for either to
work. See **Letter-key holds** below.

### Left hand — tmux and panes

| Key | Sends | Does |
| --- | --- | --- |
| `A` `S` `D` `F` | `Ctrl+H/J/K/L` | move pane / window left, down, up, right |
| `G` | `Ctrl+Space` | tmux prefix, as one key |
| `Z` | `Alt+Shift+H` | previous tmux window |
| `X` | `Alt+Shift+L` | next tmux window |
| `C` | prefix then `"` | split pane below, same directory |
| `V` | prefix then `%` | split pane right, same directory |
| `B` | prefix then `z` | zoom / unzoom pane |

The bottom three are ZMK macros — they send the prefix and the following key with
30 ms between them, so tmux sees a normal prefix sequence.

Why `Ctrl+H/J/K/L` is baked in here as `&kp LC(H)` rather than composed from a live
`Ctrl`: there is no `Ctrl` available on this layer at all. Both `Ctrl` keys, `A` and
`;`, are themselves remapped by this layer. Baking the combination in means
pane movement is one thumb plus one finger, instant and misfire-proof.

Pane movement crossing the nvim/tmux boundary also needs `vim-tmux-navigator`
installed on the **nvim** side, not just in `~/.tmux.conf`. Without it, `Ctrl+H`
stops at nvim's edge instead of handing off to the tmux pane next door.

---

## Layer 2 — Programmer symbols

Hold the **Tab / Symbols** thumb (left middle).

Split by **how a symbol is typed, not by what it is**. The right hand holds everything
that appears in a sequence — brackets, operators, comparisons — laid out so no common
pair lands on the same finger. The left hand holds everything you type in isolation.

`-` and `_` stay on the left deliberately, so `->` and `+=` become cross-hand
alternations, which are faster than any same-hand roll.

```
╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   ·   │   @   │   #   │   $   │   %   │   │   ?   │   [   │   ]   │   {   │   }   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   '   │   "   │   `   │   ~   │   \   │   │   !   │   (   │   =   │   )   │   :   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   -   │   _   │   +   │   ^   │   ·   │   │   &   │   |   │   <   │   >   │   *   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │   ·   │  ▓▓▓  │   ·   │   │   ·   │  ADJ  │   ·   │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯
```

No common symbol needs `Symbols` + `Shift` + another key. `/` `;` `,` `.` are absent
because they are already on the base layer — `//` and `*/` need no Sym slot at all,
you simply stop holding the thumb.

| Sequence | Keys | Fingers |
| --- | --- | --- |
| `()` | `Sym` + `J` `L` | index → ring |
| `[]` | `Sym` + `U` `I` | index → middle |
| `{}` | `Sym` + `O` `P` | ring → pinky |
| `<>` | `Sym` + `,` `.` | middle → ring |
| `=>` | `Sym` + `K` `.` | middle → ring |
| `>=` | `Sym` + `.` `K` | ring → middle |
| `!=` | `Sym` + `H` `K` | index → middle |
| `==` | `Sym` + `K` `K` | middle ×2 |
| `===` | `Sym` + `K` `K` `K` | middle ×3 |
| `&&` | `Sym` + `N` `N` | index ×2 |
| `\|\|` | `Sym` + `M` `M` | index ×2 |
| `??` | `Sym` + `Y` `Y` | index ×2 |
| `::` | `Sym` + `;` `;` | pinky ×2 |
| `->` | `Sym` + `Z` `.` | **cross-hand** |
| `+=` | `Sym` + `C` `K` | **cross-hand** |
| `--` | `Sym` + `Z` `Z` | left pinky ×2 |
| `__` | `Sym` + `X` `X` | left ring ×2 |

`@ # $ %` stay on the `W E R T` columns, where `2 3 4 5` sit on a number row, so that
much of the old mnemonic survives. `!` moved right because it is chained (`!=`,
`!==`), which is the whole premise of the split.

**Two known rough edges.** `<=` puts `<` and `=` on the same finger (middle) — the one
common chain that would not place cleanly without displacing `=>`, which is more
frequent. And `::` sits on the pinky, which is fine for JS/TS and annoying for Rust or
C++.

---

## Layer 3 — Adjust

Hold **both** middle thumbs at once — Sym on the left, Nav on the right. One thumb per
hand. This is a conditional layer, not a thumb of its own.

```
╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│  A-1  │  A-2  │  A-3  │  A-4  │  A-5  │   │  A-6  │  A-7  │  A-8  │  A-9  │  A-0  │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│ BT 0  │ BT 1  │ BT 2  │ BT 3  │ BT 4  │   │  F1   │  F2   │  F3   │  F4   │  F5   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│ BTCLR │  USB  │  BLE  │ RESET │ BOOT  │   │  F6   │  F7   │  F8   │  F9   │  F12  │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │   ·   │  ▓▓▓  │   ·   │   │   ·   │ (A--) │  A-=  │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯
```

`A-1` … `A-0`, `A--` and `A-=` are `RAlt` + that key, used by an AutoHotkey screen
switcher. They sit on the right thumbs because the F-row took their old home-row slots.

**`A--` is currently unreachable.** It sits on the right middle thumb — the Nav thumb
you are already holding to be on this layer, so it can never fire. Only `A-=`, on the
right outer thumb, actually works. Move it to the right inner thumb (`·` today) to fix.

Bluetooth profile select, profile clear, USB/BLE output toggle, soft reset and
bootloader are on the left hand. `F1`–`F9` are on the right. The tenth slot is `F12`,
not `F10` — so **`F10` is not bound anywhere**. `F11` and `F12` also sit on the Nav
layer's pinky column, where they are one thumb cheaper to reach.

---

## Hold-tap tuning

Two behaviours, deliberately tuned differently:

| Behaviour | Used on | Flavor | Tapping term | Quick tap |
| --- | --- | --- | --- | --- |
| `lt_fast` | Tab/Sym | `hold-preferred` | 200 ms | 175 ms |
| `mt_slow` | Enter/Alt | `tap-preferred` | 220 ms | 175 ms |
| `hm` | A/LCtrl, Z/LShift, ;/RCtrl, //RShift | `balanced` | 200 ms | 175 ms |

Only two thumbs are hold-taps at all now. `Sym` uses `hold-preferred` so the layer
engages the instant another key is pressed — no waiting on the tapping term mid-word.
The `Enter`/`Alt` thumb uses `tap-preferred`, which only engages the modifier after the
term expires, so a quick tap of `Enter` can never leak a stray Alt into an editor.

`Nav` is a plain `&mo` and `Shift`, `Space` and `Backspace` are plain `&kp`, so four
of the six thumbs have no timing behaviour whatsoever.

---

## Letter-key holds

`A` held is `LCtrl`, `Z` held is `LShift`, `;` held is `RCtrl`, `/` held is `RShift`.

The set is mirrored: `Ctrl` and `Shift` each exist on both hands. Each hand's pair
shares one pinky column, so `A`+`Z` cannot combine and neither can `;`+`/` — the
opposite hand covers exactly those cases. Reach for the right-hand pair when the target
is on the left hand, and the left-hand pair when it is on the right.

All four use the `hm` behaviour, whose important setting is `require-prior-idle-ms = 150`:
if any key was pressed within the last 150 ms the hold is **disabled entirely**, so
mid-word rolls like `as`, `ar` or `l;` always resolve as plain characters. The hold only
arms when you come to the key from rest, which is what you actually do when reaching
for a shortcut.

A key cannot be its own modifier, and two keys in the same finger column cannot be
held together. Every such case has a fallback on the other hand or on a thumb:

| Want | Cannot do | Use instead |
| --- | --- | --- |
| `Ctrl+A` (select all) | `A` is a `Ctrl` key | hold `;` |
| `Ctrl+Z` (undo) | `A` and `Z`, same column | hold `;` |
| `Ctrl+Q` (quit) | `A` and `Q`, same column | hold `;` |
| `Ctrl+;` | `;` is a `Ctrl` key | hold `A` |
| `Ctrl+/` (comment) | `;` and `/`, same column | hold `A` |
| `Ctrl+Shift+…` on the left | `A` and `Z`, same column | hold `;` + `/` |
| Capital `Q`, `A`, `Z` | `Z` cannot shift its own column | thumb `Shift`, or hold `/` |
| Capital `P`, `;`, `/` | `/` cannot shift its own column | thumb `Shift`, or hold `Z` |
| `Shift+Tab` | — | thumb `Shift` + tap the `Tab` thumb |

`Ctrl+C`, `Ctrl+V`, `Ctrl+X`, `Ctrl+S` and the rest work from either `Ctrl`.

**None of the four are live on the Nav layer** — that layer remaps `A` to `Ctrl+H`, `Z`
to `Alt+Shift+H`, `;` to `F11` and `/` to `F12`. Thumb `Shift` is gone there too (the
left outer thumb is `Tab`). For move-by-word or select-by-word, hold the modifier
**first**, let it resolve, then reach for the Nav thumb.

---

## Building

Pushing to `master` triggers the GitHub Actions build defined by
[`build.yaml`](build.yaml); download `firmware.zip` from the run's artifacts and
flash `corne_left` and `corne_right` separately.

ZMK Studio is enabled (`CONFIG_ZMK_STUDIO=y`, locking off), so the keymap can also
be edited live over USB at [zmk.studio](https://zmk.studio).

---

## Layer index reference

| Index | Name | Reached by |
| --- | --- | --- |
| 0 | Base | default |
| 1 | Nav | hold the right middle thumb |
| 2 | Sym | hold the left middle thumb (`Tab`) |
| 3 | Adj | hold both middle thumbs together |
