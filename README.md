# Corne 5-Column ZMK Config

Personal ZMK firmware for a wireless 5-column Corne (`nice_nano_v2` + `nice!view`).

Layout goals: keep Vim and tmux muscle memory intact, keep Space and Backspace on
plain keys so they repeat normally, and put every common programming symbol at most
two keys away.

- Keymap: [`config/corne.keymap`](config/corne.keymap)
- Firmware options: [`config/corne.conf`](config/corne.conf)
- Build matrix: [`build.yaml`](build.yaml)

---

## Layer 0 — Base

Plain US QWERTY, with four letter-key holds: **`Q` is `Tab`, `A` is `Ctrl`, `Z` is
`Shift`** on the left pinky column, and **`/` is `Ctrl`** on the right pinky. Every
other modifier and every layer lives on a thumb. There is no `GUI` anywhere.

```
╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│  Q/⇥  │   W   │   E   │   R   │   T   │   │   Y   │   U   │   I   │   O   │   P   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  A/^  │   S   │   D   │   F   │   G   │   │   H   │   J   │   K   │   L   │   ;   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  Z/⇧  │   X   │   C   │   V   │   B   │   │   N   │   M   │   ,   │   .   │  /-^  │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │       │  ESC  │  ENT  │   │       │  SPC  │ BSPC  │
                │ Shift │  Alt  │  Sym  │   │  Nav  │       │       │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯
```

In the thumb block the **upper label is the tap, the lower is the hold**. A blank row
means the key does nothing there.

Only two thumbs are genuinely tap/hold: `Esc`/`Alt` and `Enter`/`Sym`. The other four
are single-purpose. `Space` and `Backspace` are plain `&kp`, so they auto-repeat
normally and can never leak a layer — that is precisely why neither layer thumb sits
on them. `Shift` is a plain `&kp` as well, and `Nav` is a bare `&mo` with no tap at
all, so neither has a tapping term that could misfire.

| Thumb | Tap | Hold |
| --- | --- | --- |
| Left outer | — | `Shift` |
| Left middle | `Esc` | `Alt` |
| Left inner | `Enter` | Layer 2 — Symbols |
| Right inner | — | Layer 1 — Nav |
| Right middle | `Space` | — |
| Right outer | `Backspace` | — |

`Q/⇥`, `A/^`, `Z/⇧` and `/-^` are the only keys with holds outside the thumbs — tap for
the character, hold for `Tab`, `Ctrl`, `Shift` and `Ctrl`. See **Letter-key holds**
below.

**Combo:** `J` + `K` pressed together → `Esc`. Base layer only, 50 ms window.

---

## Layer 1 — Navigation and numbers

Hold the **Nav** thumb (right inner). Right hand is movement, left hand is
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
                │  TAB  │   ·   │  ADJ  │   │  ▓▓▓  │   ·   │  DEL  │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯
```

`·` falls through to the base layer, so the thumb `Alt` and thumb `Shift` stay
available while navigating. `Ctrl` does **not** — this layer remaps `A` to
`Ctrl+H` and `/` to `F12`. To use `Ctrl` with Nav, hold `A` or `/` first, let it
resolve to `Ctrl`, and only then reach for the Nav thumb. `▓▓▓` is the thumb you are holding to reach this layer; `ADJ` is the Sym
thumb — hold that one too and you land on Layer 3.

### Right hand — movement

| Movement | Keys |
| --- | --- |
| Arrow keys | `Nav` + `H` `J` `K` `L` |
| Delete | `Nav` + `Backspace` |
| Select horizontally | `Shift` + `Nav` + `H` / `L` |
| Move by words | `Ctrl` + `Nav` + `H` / `L` |
| Select by words | `Ctrl` + `Shift` + `Nav` + `H` / `L` |
| Home / End | `Nav` + `N` / `.` |
| Numbers | `Nav` + `Q`…`P` |
| F11 / F12 | `Nav` + `;` / `/` |

Move-by-word and select-by-word both need `Ctrl`, which is overwritten on this layer.
Hold `A` or `/` **before** the Nav thumb for either to work. See **Letter-key holds**
below.

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
`/`, are themselves remapped by this layer. Baking the combination in means
pane movement is one thumb plus one finger, instant and misfire-proof.

Pane movement crossing the nvim/tmux boundary also needs `vim-tmux-navigator`
installed on the **nvim** side, not just in `~/.tmux.conf`. Without it, `Ctrl+H`
stops at nvim's edge instead of handing off to the tmux pane next door.

---

## Layer 2 — Programmer symbols

Hold the **Enter / Symbols** thumb (left inner).

The top row mirrors the shifted number row of a normal keyboard, so `!@#$%` sits
exactly where your fingers already expect it. Every bracket pair is typed with a
single hand: parens and brackets on the left, braces on the right.

```
╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   !   │   @   │   #   │   $   │   %   │   │   ^   │   &   │   *   │   ?   │   \   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   (   │   )   │   [   │   ]   │   =   │   │   {   │   }   │   |   │   "   │   '   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   -   │   _   │   `   │   ~   │   +   │   │   :   │   ;   │   <   │   >   │   ·   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │   ·   │   ·   │  ▓▓▓  │   │  ADJ  │   ·   │   ·   │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯
```

No common symbol needs `Symbols` + `Shift` + another key.

| Sequence | Keys | Hand |
| --- | --- | --- |
| `()` | `Sym` + `A` `S` | left |
| `[]` | `Sym` + `D` `F` | left |
| `{}` | `Sym` + `H` `J` | right |
| `&&` | `Sym` + `U` `U` | right |
| `\|\|` | `Sym` + `K` `K` | right |
| `!` | `Sym` + `Q` | left |
| `!=` | `Sym` + `Q` `G` | left |
| `===` | `Sym` + `G` `G` `G` | left |
| `=>` | `Sym` + `G` `.` | both |
| `->` | `Sym` + `Z` `.` | both |
| `++` | `Sym` + `B` `B` | left |
| `--` | `Sym` + `Z` `Z` | left |
| `__` | `Sym` + `X` `X` | left |
| `??` | `Sym` + `O` `O` | right |
| `::` | `Sym` + `N` `N` | right |

---

## Layer 3 — Adjust

Hold **both** inner thumbs at once — Sym on the left, Nav on the right. One thumb per
hand. This is a conditional layer, not a thumb of its own.

```
╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│  A-1  │  A-2  │  A-3  │  A-4  │  A-5  │   │  A-6  │  A-7  │  A-8  │  A-9  │  A-0  │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│ BT 0  │ BT 1  │ BT 2  │ BT 3  │ BT 4  │   │  F1   │  F2   │  F3   │  F4   │  F5   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│ BTCLR │  USB  │  BLE  │ RESET │ BOOT  │   │  F6   │  F7   │  F8   │  F9   │  F10  │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │   ·   │   ·   │  ▓▓▓  │   │  ▓▓▓  │  A--  │  A-=  │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯
```

`A-1` … `A-0`, `A--` and `A-=` are `RAlt` + that key, used by an AutoHotkey screen
switcher. `A--` and `A-=` sit on the right thumbs because the F-row took their old
home-row slots.

Bluetooth profile select, profile clear, USB/BLE output toggle, soft reset and
bootloader are on the left hand. `F1`–`F10` are on the right; `F11` and `F12` stay
on the Nav layer's pinky column, where they are one thumb cheaper to reach.

---

## Hold-tap tuning

Two behaviours, deliberately tuned differently:

| Behaviour | Used on | Flavor | Tapping term | Quick tap |
| --- | --- | --- | --- | --- |
| `lt_fast` | Enter/Sym | `hold-preferred` | 200 ms | 175 ms |
| `mt_slow` | Esc/Alt | `tap-preferred` | 220 ms | 175 ms |
| `hm` | Q/Tab, A/Ctrl, Z/Shift, //Ctrl | `balanced` | 200 ms | 175 ms |

Only two thumbs are hold-taps at all now. `Sym` uses `hold-preferred` so the layer
engages the instant another key is pressed — no waiting on the tapping term mid-word.
The `Alt` thumb uses `tap-preferred`, which only engages the modifier after the term
expires, so a quick tap can never leak a stray Alt into an editor.

`Nav` is a plain `&mo` and `Shift`, `Space` and `Backspace` are plain `&kp`, so four
of the six thumbs have no timing behaviour whatsoever.

---

## Letter-key holds

`Q` held is `Tab`, `A` held is `Ctrl`, `Z` held is `Shift`, `/` held is `Ctrl`.

`Ctrl` is deliberately on both hands. The three left-hand holds all share the pinky
column, so none of them can combine with each other — `/` is the right-hand `Ctrl`
that covers exactly those cases. Reach for `/` when the target is on the left hand,
`A` when it is on the right.

All three use the `hm` behaviour, whose important setting is `require-prior-idle-ms = 150`:
if any key was pressed within the last 150 ms the hold is **disabled entirely**, so
mid-word rolls like `as`, `ar` or `qu` always resolve as plain letters. The hold only
arms when you come to the key from rest, which is what you actually do when reaching
for a shortcut.

Holding `Q` sends `Tab` and keeps it held, so the host auto-repeats it — hold it long
enough and you get several tabs, not one.

**All three sit in the left pinky column, so no two of them can ever combine**, and
none can modify a letter in its own column. The thumbs cover every case they cannot:

A key cannot be its own modifier, and two keys in the same finger column cannot be
held together. Every such case now has a fallback:

| Want | Cannot do | Use instead |
| --- | --- | --- |
| `Ctrl+A` (select all) | `A` is a `Ctrl` key | hold `/` |
| `Ctrl+Z` (undo) | `A` and `Z`, same finger | hold `/` |
| `Ctrl+Q` (quit) | `A` and `Q`, same finger | hold `/` |
| `Ctrl+/` (comment) | `/` is a `Ctrl` key | hold `A` |
| `Shift+Tab` | `Z` and `Q`, same finger | thumb `Shift` + hold `Q` |
| Capital `Q`, `A`, `Z` | `Z` cannot shift its own column | thumb `Shift` |

`Ctrl+C`, `Ctrl+V`, `Ctrl+X`, `Ctrl+S` and the rest work from either `Ctrl`.

**Neither `Ctrl` is live on the Nav layer** — that layer remaps `A` to `Ctrl+H` and `/`
to `F12`. For move-by-word or select-by-word, hold the `Ctrl` key **first**, let it
resolve, then reach for the Nav thumb.

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
| 1 | Nav | hold the right inner thumb |
| 2 | Sym | hold the left inner thumb (`Enter`) |
| 3 | Adj | hold both inner thumbs together |
