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

Plain US QWERTY. No letter key has any hold behaviour — every modifier and every
layer lives on a thumb. There is no `Alt` and no `GUI` on this layer at all.

```
╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   Q   │   W   │   E   │   R   │   T   │   │   Y   │   U   │   I   │   O   │   P   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   A   │   S   │   D   │   F   │   G   │   │   H   │   J   │   K   │   L   │   ;   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   Z   │   X   │   C   │   V   │   B   │   │   N   │   M   │   ,   │   .   │   /   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │       │  ESC  │  ENT  │   │       │  SPC  │ BSPC  │
                │ Shift │ Ctrl  │  Sym  │   │  Nav  │       │       │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯
```

In the thumb block the **upper label is the tap, the lower is the hold**. A blank row
means the key does nothing there.

Only two thumbs are genuinely tap/hold: `Esc`/`Ctrl` and `Enter`/`Sym`. The other four
are single-purpose. `Space` and `Backspace` are plain `&kp`, so they auto-repeat
normally and can never leak a layer — that is precisely why neither layer thumb sits
on them. `Shift` is a plain `&kp` as well, and `Nav` is a bare `&mo` with no tap at
all, so neither has a tapping term that could misfire.

| Thumb | Tap | Hold |
| --- | --- | --- |
| Left outer | — | `Shift` |
| Left middle | `Esc` | `Ctrl` |
| Left inner | `Enter` | Layer 2 — Symbols |
| Right inner | — | Layer 1 — Nav |
| Right middle | `Space` | — |
| Right outer | `Backspace` | — |

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

`·` falls through to the base layer, so `Ctrl` and `Shift` stay available while
navigating. `▓▓▓` is the thumb you are holding to reach this layer; `ADJ` is the Sym
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

Select-by-word is the one awkward motion: `Ctrl` and `Shift` are both on the left
thumb, on adjacent keys, so it needs a thumb roll. See **Hold-tap tuning** below.

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

Why `Ctrl+H/J/K/L` lives here rather than just holding the Ctrl thumb: the Ctrl
thumb is `tap-preferred` at 220 ms, so pressing `H` before the term expires yields
**Esc then h**, not a pane switch. The Nav thumb is a bare `&mo` with no tap and no
tapping term, so it engages the moment it goes down — pane movement is instant and
cannot misfire.

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
| `mt_slow` | Esc/Ctrl | `tap-preferred` | 220 ms | 175 ms |

Only two thumbs are hold-taps at all now. `Sym` uses `hold-preferred` so the layer
engages the instant another key is pressed — no waiting on the tapping term mid-word.
The `Ctrl` thumb uses `tap-preferred`, which only engages the modifier after the term
expires, so a quick tap can never leak a stray Ctrl into an editor.

`Nav` is a plain `&mo` and `Shift`, `Space` and `Backspace` are plain `&kp`, so four
of the six thumbs have no timing behaviour whatsoever.

**Known edge case:** `Ctrl` + `Shift` puts both mods on the left thumb across two
adjacent keys. `Ctrl+Shift+Nav+H/L` (select by word) needs a thumb roll. If it bites,
add a combo rather than moving to home-row mods.

**Tab** no longer has a base-layer key. It lives on `Nav` + left outer thumb.

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
