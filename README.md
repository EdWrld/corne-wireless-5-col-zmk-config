# Corne 5-Column ZMK Config

Personal ZMK firmware for a wireless 5-column Corne (`nice_nano_v2` + `nice!view`),
built around Vim and tmux.

- Keymap: [`config/corne.keymap`](config/corne.keymap)
- Firmware options: [`config/corne.conf`](config/corne.conf)
- Build matrix: [`build.yaml`](build.yaml)
- Cheat sheet overlay: [`tools/keymap-overlay`](tools/keymap-overlay)
  (`./tools/keymap-overlay/zmk-cheatsheet`)

---

## How the layers work

Base thumbs **31** (left, under the index) and **34** (right) are L1 and L2:

| Gesture | L1 (symbols) | L2 (numbers) |
| --- | --- | --- |
| tap | sticky, one key | same |
| hold | momentary | same |
| both together | **from base:** NAV, while held. **from L1–L4:** back to base | same |

L1 and L2 do not step into each other. On L1 the right layer thumb taps `+`
(hold still stacks L2 for NAV). On L2 the left layer thumb taps `,` (hold
stacks L1). Space stays Space on L1, L2 and L4 so a missed layer does not
send Alt.

**NAV** is one layer, two ways in: hold L1+L2, or hold `/` on base. **P** on
NAV toggles mouse (L3). **`/`** on NAV is L4 (adjust). L4 is off the thumb
ring.

---

## L0 — Base

Plain US QWERTY. No home-row mods. Thumb mods are sticky and can be held.

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   Q   │   W   │   E   │  R/$  │   T   │   │   Y   │   U   │   I   │  O/0  │   P   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   A   │   S   │   D   │   F   │   G   │   │   H   │   J   │   K   │   L   │  '/;  │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   Z   │   X   │   C   │   V   │   B   │   │   N   │   M   │  ,/_  │  ./!  │ /NAV  │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │ sCtrl │  L1   │  s⇧   │   │   ␣   │  L2   │ ⌫/C⌫  │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

`s` = sticky. `x/y` = tap `x`, hold `y`. `R` held is `$`, `O` held is `0`,
`'` held is `;` (idle-gated), comma held is `_`, dot held is `!`, `/` held
is NAV. Backspace held is Ctrl+Backspace.

Combos: **Q+W** Tab, **J+K** Esc, **L+'** Enter, **D+K** colon, **F+H** copy,
**V+N** paste.

---

## L1 — Symbols

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│  Esc  │   @   │   #   │   %   │   $   │   │   &   │   *   │   {   │   }   │   -   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   ⇥   │   <   │   >   │   ?   │   |   │   │   =   │   (   │   )   │   :   │   ;   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   -   │   !   │   `   │   /   │   ~   │   │   "   │   ^   │   [   │   ]   │   \   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │   ·   │  L0   │   ·   │   │   ␣   │  +   │ sAltGr│
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

`+` on the right layer thumb: tap `+`, hold stacks L2 for NAV.

---

## L2 — Numbers and media

Top row is `1`–`0`. Right hand below that is math; left is media and
clipboard on the same columns as base `X C V`.

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   1   │   2   │   3   │   4   │   5   │   │   6   │   7   │   8   │   9   │   0   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   ⇥   │  Vol- │  Vol+ │  Bri- │  Bri+ │   │   =   │   -   │   +   │   _   │   ⏎   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│ PrtSc │  Cut  │  Copy │  Pste │  Mute │   │   /   │   *   │   (   │   )   │   .   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │ sCtrl │   ,   │ Super │   │   ␣   │  L0   │ sAltGr│
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

`,` on the left layer thumb: tap comma, hold stacks L1 for NAV. Clipboard
keys send HID Cut/Copy/Paste, not Ctrl+X/C/V.

---

## L3 — Mouse (and browser)

Toggle from **P on NAV**. Left hand is the pointer; right hand is arrows and
browser. Thumbs fall through (Space and Backspace still work).

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│  Esc  │  Scr← │  Mv↑  │  Scr→ │  C-T  │   │  A-←  │ C-S-⇥ │  C-⇥  │  A-→  │   ⌫   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   ⇥   │  Mv←  │  Mv↓  │  Mv→  │   ⌦   │   │   ←   │   ↓   │   ↑   │   →   │   ⏎   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   ·   │  RClk │  Scr↑ │  Scr↓ │  C-W  │   │  Home │  PgDn │  PgUp │  End  │ C-S-A │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │   ·   │   ·   │  LClk │   │   ·   │   ·   │   ·   │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

| Key | Sends | Does |
| --- | --- | --- |
| `A-←` `A-→` | Alt+Left / Alt+Right | Back / Forward |
| `C-S-⇥` `C-⇥` | Ctrl+Shift+Tab / Ctrl+Tab | previous / next tab |
| `C-S-A` | Ctrl+Shift+A | Chrome tab search (Firefox: Add-ons) |
| `C-T` `C-W` | Ctrl+T / Ctrl+W | new tab / close tab |

Pointer needs `CONFIG_ZMK_POINTING=y`. After enabling it, re-pair if the
mouse is ignored over Bluetooth (`BTclr` on L4).

---

## L4 — Adjust

Radios, output, reset, F-row. **NAV `/` only** — not on the thumb ring.

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│  Esc  │   ·   │   ·   │   ·   │  BT4  │   │   F1  │   F2  │   F3  │   F4  │   ⌫   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   ⇥   │  BT0  │  BT1  │  BT2  │  BT3  │   │   F5  │   F6  │   F7  │   F8  │   ⏎   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│ BTclr │  USB  │  BLE  │ Reset │  Boot │   │   F9  │  F10  │  F11  │  F12  │   ·   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │   ·   │  L0   │   ·   │   │   ␣   │  L0   │ sAltGr│
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

---

## L5 — NAV

Hold **L1+L2**, or hold **`/`**. Same layer either way.

Top row is `Alt+1`–`9`. **P** toggles mouse. **`/`** is L4. ASDF are arrows;
HJKL are tmux panes (`Alt+Shift+H/J/K/L`). ZXCV are sticky Super/Alt/Shift/Ctrl.
**M** is tmux prefix then `[` (copy/scroll).

Thumbs: zoom (`prefix+z`), new window (`prefix+c`), next tab, prev tab. The
two layer thumbs are new-window / prev-tab only if you entered with `/`
(they stay L1/L2 if that is how you got here).

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│  A-1  │  A-2  │  A-3  │  A-4  │  A-5  │   │  A-6  │  A-7  │  A-8  │  A-9  │ MOUSE │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   ←   │   ↓   │   ↑   │   →   │   ·   │   │ pane← │ pane↓ │ pane↑ │ pane→ │   ·   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│ sGUI  │ sAlt  │  s⇧   │ sCtrl │   ·   │   │   ·   │ scrl  │   ·   │   ·   │  L4   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │ zoom  │  new  │   ·   │   │  Tab> │  Tab< │   ·   │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

---

## Combos

| Keys | Sends | Window | Live on |
| --- | --- | --- | --- |
| Q+W | Tab | 50 ms | every layer |
| J+K | Esc | 50 ms, idle 100 ms | every layer |
| L+' | Enter | 50 ms | every layer |
| D+K | colon | 50 ms | every layer |
| F+H | Copy | 50 ms, idle 100 ms | every layer |
| V+N | Paste | 50 ms, idle 100 ms | every layer |
| both layer thumbs | base | 50 ms | L1–L4 |

Esc/copy/paste will not fire within 100 ms of another key. Enter/Tab/colon
have no idle gate (`:w` then Enter, completion Tab).

---

## Behaviours and timing

| Behaviour | Used on | Flavor | Term | Notes |
| --- | --- | --- | --- | --- |
| `slm` | base L1/L2 thumbs | hold-preferred | 250 ms | mo hold, sl tap |
| `td` | R, O, `,`, `.` | tap-preferred | 200 ms | `$` `0` `_` `!` on hold; quick-tap 200 |
| `qt` | `'` | tap-preferred | 180 ms | `;` on hold; idle 100 ms |
| `wb` | Backspace | tap-preferred | 200 ms | Ctrl+Backspace on hold |
| `ns` | `/` | tap-preferred | 200 ms | hold = NAV (macro L1+L2) |
| `lk` | L1 `+`, L2 `,` | hold-preferred | 250 ms | hold stacks the other layer |
| `hm` `slt` `stp` `&mt` | unused | | | |
| `&mmv` | mouse | — | 680 ms to max | |

Do not `&mo` L5; the conditional layer owns it.

---

## Building

Pushing to `master` triggers GitHub Actions ([`build.yaml`](build.yaml));
download `firmware.zip` and flash left and right separately.

ZMK Studio is enabled. Studio edits live on the keyboard, not in this file.

---

## Layer index

| Index | Name | Contents | Reached by |
| --- | --- | --- | --- |
| 0 | L0 | QWERTY | default |
| 1 | L1 | symbols | left thumb, tap or hold |
| 2 | L2 | numbers, media, clipboard | right thumb, tap or hold |
| 3 | MOUSE | pointer + browser | P on NAV (toggle) |
| 4 | L4 | Bluetooth, output, reset, F-row | `/` on NAV |
| 5 | NAV | windows, tmux, arrows, tabs | L1+L2, or hold `/` |
