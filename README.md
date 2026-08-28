# Corne 5-Column ZMK Config

Personal ZMK firmware for a wireless 5-column Corne (`nice_nano_v2` + `nice!view`),
built around Vim and tmux.

- Keymap: [`config/corne.keymap`](config/corne.keymap)
- Firmware options: [`config/corne.conf`](config/corne.conf)
- Build matrix: [`build.yaml`](build.yaml)
- Cheat sheet overlay: [`tools/keymap-overlay`](tools/keymap-overlay) — an
  always-on-top window rendered straight from the keymap
  (`./tools/keymap-overlay/zmk-cheatsheet`)

---

## How the layers work

Two thumb keys drive every layer change: **position 31** (left, under the index)
and **position 34** (right). Both are hold-taps whose tap is a sticky layer, but
they differ on the hold, because symbols and numbers are typed differently:

| Gesture | Left thumb (L1, symbols) | Right thumb (L2, numbers) |
| --- | --- | --- |
| tap | that layer for **exactly one key** | same |
| hold | **momentary** — live while held, gone on release | past 250 ms, **locked on** and stays |
| both thumbs together | back to base, from any layer | same |

Symbols arrive in runs — `()`, `->`, `{}`, `=>` — so holding is the natural
gesture and locking has little to do. Numbers are the opposite: one long digit
string, which is exactly what a lock is for.

On every non-base layer those same two positions become plain `&to` switches —
**31 steps out, 34 steps in** — so the layers form a ring:

```
    L0 --34--> L1 --34--> L2 --34--> L3 ··hold 34··> L4
       <--31--    <--31--    <--31--
```

**L4 is the exception: hold, never tap.** Tapping the step-in thumb one time too
many used to land you on the adjust layer with `sys_reset`, `bootloader` and the
Bluetooth profiles under your fingers. Position 34 on L3 is `&mo L4`, so a tap
does nothing at all and reaching the radios takes a deliberate hold plus a
second key on the other hand.

L1 cannot be locked from its own key. To lock it, hold the right thumb past
250 ms to lock L2, then tap position 31 — which is `&to L1` there.

`Tab` and `Esc` sit at positions 0 and 10 on every non-base layer, so they are in
the same place no matter where you are.

---

## L0 — Base

Plain US QWERTY. No home-row mods: every letter is a bare `&kp`, so rolls are
never reinterpreted and there is no timing to fight. Modifiers live on the thumbs
as sticky keys, which can also be held like ordinary modifiers.

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   Q   │   W   │   E   │   R   │   T   │   │   Y   │   U   │   I   │  O/-  │   P   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   A   │   S   │   D   │   F   │   G   │   │   H   │   J   │   K   │   L   │   ;   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   Z   │   X   │   C   │   V   │   B   │   │   N   │   M   │  ,/_  │  ./!  │   /   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │ sCtrl │&slm L1 L1│   s⇧  │   │   ␣   │   L2  │   ⌫   │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

`s` prefixes a sticky modifier. `x/y` is a tapdance — tap `x`, hold for `y`:
`O` held is `-`, `,` held is `_`, `.` held is `!`.

`Backspace` is on the right outer thumb; `Tab` is not on base at all, since the
30 grid keys are exactly the 26 letters plus `; , . /`. `Enter` and `Esc` are
combos. `Delete` exists only on L3.

---

## L1 — Symbols

Every symbol is a direct keypress; nothing here needs `Shift`. `0` and `$` give
the Vim line-start and line-end motions on one layer.

All three bracket pairs share the same two columns — **openers on the index
finger, closers on the middle** — so the finger pair never changes and only the
row does: `{}` on top, `()` on home, `[]` below.

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   ⇥   │   @   │   #   │   %   │   $   │   │   0   │   {   │   }   │   &   │   ⌫   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  Esc  │   !   │   `   │   ?   │   |   │   │   '   │   (   │   )   │   =   │   ⏎   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   -   │   "   │   *   │   \   │   _   │   │   <   │   [   │   ]   │   >   │   /   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │   ·   │   L0  │ Super │   │ sAltGr│   L2  │   ·   │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

`~` and `+` are the two omissions — they are `Shift` + the `` ` `` and `=` on
this layer, the same as on a full keyboard. Position 32 falls through to base's
sticky `Shift`, so that works without leaving. `+` also has a direct key on L2.

`<` and `>` are also reachable from base as `Shift+,` and `Shift+.`; they are
duplicated here for convenience.

---

## L2 — Numbers and media

Right hand is a numpad in phone-pad order with the math operators on the left,
so `=`, `+`, `-` and `*` are all one hand away from the digits.

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   ⇥   │  Cut  │  Copy │  Pste │   -   │   │   _   │   7   │   8   │   9   │   ⌫   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  Esc  │ PrtSc │  Vol- │  Vol+ │   +   │   │   =   │   4   │   5   │   6   │   ⏎   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   ·   │   *   │  Bri- │  Bri+ │  Mute │   │   0   │   1   │   2   │   3   │   .   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │ sCtrl │   L1  │ Super │   │ sAltGr│   L3  │   ·   │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

The clipboard trio sits in the **same columns as base `X C V`** — ring, middle,
index — so it is the same finger sequence one row up. They send the HID
`Cut`/`Copy`/`Paste` usages rather than `Ctrl+X/C/V`, which a few GTK and Qt
apps ignore; swap them for `&kp LC(X)` and friends if that bites.

`Super` is a plain `&kp LGUI` here rather than sticky. To use `Super`+letter,
**tap** into this layer rather than holding it: `&sl` has `quick-release` and no
`ignore-modifiers`, so pressing `Super` drops the layer immediately and leaves
you back on base with the modifier still held.

---

## L3 — Pointer and navigation

Left hand drives the mouse, right hand is arrows plus browser control. This is
where `Delete`, `Home`/`End` and `PgUp`/`PgDn` live — each page key sits directly
under its arrow.

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   ⇥   │  Scr← │  Mv↑  │  Scr→ │  C-T  │   │  A-←  │ C-S-⇥ │  C-⇥  │  A-→  │   ⌫   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  Esc  │  Mv←  │  Mv↓  │  Mv→  │   ⌦   │   │   ←   │   ↓   │   ↑   │   →   │   ⏎   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│   ·   │  RClk │  Scr↑ │  Scr↓ │  C-W  │   │  Home │  PgDn │  PgUp │  End  │ C-S-A │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │   L0  │   L2  │  LClk │   │ sAltGr│   L4  │   ·   │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

| Key | Sends | Does |
| --- | --- | --- |
| `A-←` `A-→` | `Alt+Left` / `Alt+Right` | Back / Forward — works in Firefox, Chrome, file managers |
| `C-S-⇥` `C-⇥` | `Ctrl+Shift+Tab` / `Ctrl+Tab` | previous / next tab |
| `C-S-A` | `Ctrl+Shift+A` | Chrome tab search — **in Firefox this opens the Add-ons manager instead** |
| `C-T` `C-W` | `Ctrl+T` / `Ctrl+W` | new tab / close tab — same column, top opens and bottom closes |

Pointer keys need `CONFIG_ZMK_POINTING=y`, which is set. Note that enabling it
changes the HID report descriptor, so a host you were already paired with may
keep serving the old keyboard-only one and ignore the mouse entirely — clear the
bond with `BTclr` on L4 and re-pair if the pointer does nothing over Bluetooth
but works over USB.

---

## L4 — Adjust

Radios, output routing, reset and the F-row. Reached only by **holding**
position 34 while on L3 — never by tapping — so it cannot be stumbled into.

╭───────┬───────┬───────┬───────┬───────╮   ╭───────┬───────┬───────┬───────┬───────╮
│   ⇥   │   ·   │   ·   │   ·   │  BT4  │   │   F1  │   F2  │   F3  │   F4  │   ⌫   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  Esc  │  BT0  │  BT1  │  BT2  │  BT3  │   │   F5  │   F6  │   F7  │   F8  │   ⏎   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│ BTclr │  USB  │  BLE  │ Reset │  Boot │   │   F9  │  F10  │  F11  │  F12  │   ·   │
╰───────┴───────┴───────┴───────┴───────╯   ╰───────┴───────┴───────┴───────┴───────╯
                ╭───────┬───────┬───────╮   ╭───────┬───────┬───────╮
                │   ·   │   L3  │   ·   │   │ sAltGr│   L0  │   ·   │
                ╰───────┴───────┴───────╯   ╰───────┴───────┴───────╯

---

## Combos

| Keys | Sends | Window | Live on |
| --- | --- | --- | --- |
| `J` + `K` | `Esc` | 50 ms | base only |
| `L` + `;` | `Enter` | 50 ms | base only |
| both layer thumbs | back to base | 50 ms | L1–L4 |

The escape chord is scoped off base on purpose. ZMK picks combo candidates from
the highest active layer at the **first** keypress, so on base it is never a
candidate and the two thumb keys behave normally.

`combo_esc` carries `require-prior-idle-ms = 100`, meaning it will not fire
within 100 ms of another keypress — which is exactly the insert-mode-exit
rhythm. Drop it to 0 if `jk` starts typing itself instead of escaping.
`combo_ret` has no such guard, deliberately: it has to fire right after a letter
for `:w<CR>` to work.

---

## Behaviours and timing

| Behaviour | Used on | Flavor | Term | Notes |
| --- | --- | --- | --- | --- |
| `slm` | base thumb 31 (L1) | `hold-preferred` | 250 ms | `&mo` on hold, `&sl` on tap |
| `slt` | base thumb 34 (L2) | `tap-preferred` | 250 ms | `&tog` on hold, `&sl` on tap |
| `td` | `O`, `,`, `.` on base | `tap-preferred` | 200 ms | `&td <hold> <tap>` |
| `hm` | *unused* | `tap-preferred` | 200 ms | kept so a home-row mod is one word away |
| `&mt` | *unused* | `tap-preferred` | 200 ms | node override only |
| `&mmv` | L3 pointer | — | — | 680 ms to max speed, acceleration exponent 2 |

`tap-preferred` resolves the hold **only** when the term expires — another
keypress cannot force it. That is what stops a fast tap from leaking a toggle
on `slt`, and it is also why a locked layer is not live until 250 ms have passed.

`slm` uses `hold-preferred` instead, so the symbol layer goes live the instant
another key goes down — no dead spot before the first `(`. That flavor normally
risks reading a fast tap-then-key as a hold, but here both branches emit the
same character and only the exit differs, so the misread is invisible.

---

## Known rough edges

- **No `LALT` and no `^` anywhere.** `&sk RALT` is on L1–L3, which is AltGr on
  an international layout rather than a plain Alt. `^` needs `Shift`+`6`, and
  `6` is on L2 where position 32 is `Super` rather than `Shift`.
- **`Super` only exists on L2**, so `Super`+letter needs the tap-then-hold
  sequence described above.
- **`td` sits on three keys that are also Vim commands.** Pausing on `.`, `,`
  or `O` in normal mode emits `!`, `_` or `-`. The term is 200 ms, which covers
  normal typing; push it further if normal-mode pauses still misfire.
- **Tab costs a layer trip** now that Backspace has the thumb.
- **No Shift on L2** — position 32 is `Super` there.

---

## Building

Pushing to `master` triggers the GitHub Actions build defined by
[`build.yaml`](build.yaml); download `firmware.zip` from the run's artifacts and
flash `corne_left` and `corne_right` separately. There is no local toolchain in
this repo, so CI is the real syntax check.

ZMK Studio is enabled (`CONFIG_ZMK_STUDIO=y`, locking off), so the keymap can
also be edited live over USB at [zmk.studio](https://zmk.studio). Studio edits
live in the keyboard's settings, not in this file — write them back by hand if
you want to keep them.

---

## Layer index reference

| Index | Name | Contents | Reached by |
| --- | --- | --- | --- |
| 0 | L0 | QWERTY | default |
| 1 | L1 | symbols | left thumb — tap for one key, hold for as long as you like |
| 2 | L2 | numbers, media, clipboard | right thumb — tap for one key, hold 250 ms to lock |
| 3 | L3 | pointer, arrows, browser | `34` from L2 |
| 4 | L4 | Bluetooth, output, reset, F-row | **hold** `34` from L3 |
