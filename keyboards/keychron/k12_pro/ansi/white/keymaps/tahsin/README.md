# Tahsin's Keychron K12 Pro Custom Keymap

Custom keymap for Keychron K12 Pro with Vim-style navigation and Caps Lock remapping.

## Features

- **Caps Lock**: Tap for ESC, hold for Left Control (using `MT(MOD_LCTL, KC_ESC)`)
- **Vim-style arrows**: Ctrl+H/J/K/L → Left/Down/Up/Right arrows
- **Key Override enabled**: Better modifier key handling

## Prerequisites

Install QMK CLI and ARM toolchain via Homebrew:

```fish
brew install qmk/qmk/qmk
brew install arm-none-eabi-binutils arm-none-eabi-gcc@8 avr-binutils avr-gcc@8
```

## Compilation

### Option 1: Using PATH (current method)

```fish
PATH=/opt/homebrew/opt/arm-none-eabi-binutils/bin:/opt/homebrew/opt/arm-none-eabi-gcc@8/bin:/opt/homebrew/opt/avr-binutils/bin:/opt/homebrew/opt/avr-gcc@8/bin:/opt/homebrew/bin:/opt/homebrew/sbin:$PATH qmk compile -kb keychron/k12_pro/ansi/white -km tahsin
```

### Option 2: Add to fish config (recommended)

Add this to `~/.config/fish/config.fish`:

```fish
# QMK toolchain
set -gx PATH /opt/homebrew/opt/arm-none-eabi-binutils/bin $PATH
set -gx PATH /opt/homebrew/opt/arm-none-eabi-gcc@8/bin $PATH
set -gx PATH /opt/homebrew/opt/avr-binutils/bin $PATH
set -gx PATH /opt/homebrew/opt/avr-gcc@8/bin $PATH
```

Then reload config with `source ~/.config/fish/config.fish` and compile with:

```fish
qmk compile -kb keychron/k12_pro/ansi/white -km tahsin
```

### Option 3: Use helper script

Create `compile.sh` in this directory:

```bash
#!/usr/bin/env bash
PATH=/opt/homebrew/opt/arm-none-eabi-binutils/bin:/opt/homebrew/opt/arm-none-eabi-gcc@8/bin:/opt/homebrew/opt/avr-binutils/bin:/opt/homebrew/opt/avr-gcc@8/bin:/opt/homebrew/bin:/opt/homebrew/sbin:$PATH qmk compile -kb keychron/k12_pro/ansi/white -km tahsin
```

Make it executable: `chmod +x compile.sh`, then run `./compile.sh`

## Flashing

1. Compile the firmware (see above)
2. Open QMK Toolbox
3. Load the generated `.bin` file from the build output
4. Put keyboard in bootloader mode (usually Fn + Esc or physical reset button)
5. Click "Flash" in QMK Toolbox

The compiled firmware will be at:
```
.build/keychron_k12_pro_ansi_white_tahsin.bin
```

## Important Notes

### Karabiner-Elements Conflict

**Do NOT use** Karabiner-Elements to remap Caps Lock or ESC, as it will conflict with the firmware-level remapping. If you have Karabiner installed, ensure these simple modifications are NOT enabled:
- ❌ `escape` → `grave_accent_and_tilde`
- ❌ `caps_lock` → anything (let firmware handle it)

The firmware handles Caps Lock remapping at the hardware level, which is more reliable than OS-level remapping.

## Keymap Layout

### Base Layer (MAC_BASE / WIN_BASE)
```
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───────┐
│ ` │ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │ 7 │ 8 │ 9 │ 0 │ - │ = │ Bksp  │
├───┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─────┤
│ Tab │ Q │ W │ E │ R │ T │ Y │ U │ I │ O │ P │ [ │ ] │  \  │
├─────┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴─────┤
│ Esc/C│ A │ S │ D │ F │ G │ H │ J │ K │ L │ ; │ ' │  Enter │
├──────┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴────────┤
│ Shift  │ Z │ X │ C │ V │ B │ N │ M │ , │ . │ / │   Shift  │
├────┬───┴┬──┴─┬─┴───┴───┴───┴───┴───┴──┬┴───┼───┴┬────┬────┤
│Ctrl│Opt │Cmd │        Space           │Cmd │FN1 │ FN2│Ctrl│
└────┴────┴────┴────────────────────────┴────┴────┴────┴────┘
```

**Esc/C** = Tap for ESC, Hold for Left Control

### Key Overrides (Active on all layers)
- `Ctrl + H` → Left Arrow
- `Ctrl + J` → Down Arrow
- `Ctrl + K` → Up Arrow
- `Ctrl + L` → Right Arrow

## Files

- `keymap.c` - Main keymap configuration
- `rules.mk` - Build rules (enables KEY_OVERRIDE)
- `README.md` - This file

## Troubleshooting

### Caps Lock produces backtick (`) instead of ESC
Check Karabiner-Elements configuration and remove any ESC remapping.

### Key overrides not working
Ensure `KEY_OVERRIDE_ENABLE = yes` is in `rules.mk` and firmware is compiled and flashed.

### Compilation fails
Ensure ARM toolchain is in PATH (see Option 2 above).
