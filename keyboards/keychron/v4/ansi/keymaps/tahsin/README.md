# Tahsin's Keychron V4 Custom Keymap

Custom keymap for Keychron V4 with Vim-style navigation and Caps Lock remapping.

## Features

- **Caps Lock**: Tap for ESC, hold for Left Control (using `MT(MOD_LCTL, KC_ESC)`)
- **Vim-style arrows**: Ctrl+H/J/K/L -> Left/Down/Up/Right arrows
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
PATH=/opt/homebrew/opt/arm-none-eabi-binutils/bin:/opt/homebrew/opt/arm-none-eabi-gcc@8/bin:/opt/homebrew/opt/avr-binutils/bin:/opt/homebrew/opt/avr-gcc@8/bin:/opt/homebrew/bin:/opt/homebrew/sbin:$PATH qmk compile -kb keychron/v4/ansi -km tahsin
```


## Flashing

### Step-by-step flashing instructions

1. **Compile the firmware** (see compilation section above)
   - The compiled firmware will be at: `.build/keychron_v4_ansi_tahsin.bin`

2. **Prepare the keyboard:**
   - Unplug the keyboard from your computer
   - Remove the **spacebar keycap** (the reset button is underneath)

3. **Enter DFU (bootloader) mode:**
   - Press and hold the **reset button** (under the spacebar)
   - While holding reset, plug in the USB cable
   - Release the reset button after plugging in

4. **Flash with QMK Toolbox:**
   - Open QMK Toolbox application
   - Verify that it shows the keyboard in bootloader mode
   - Click **"Open"** and select the compiled `.bin` file (`.build/keychron_v4_ansi_tahsin.bin`)
   - Click **"Flash"**
   - Wait for the flashing process to complete
   - The keyboard will automatically restart with the new firmware

5. **Test the keyboard:**
   - Replace the spacebar keycap
   - Test Caps Lock: tap for ESC, hold for Control
   - Test key overrides: Ctrl+H/J/K/L for arrows

## Important Notes

If using Karabiner-Elements, disable it for this keyboard to avoid conflicts with firmware-level remapping.

## Keymap Layout

### Base Layer (MAC_BASE / WIN_BASE)
```
+---+---+---+---+---+---+---+---+---+---+---+---+---+-------+
| ` | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 0 | - | = | Bksp  |
+-----+---+---+---+---+---+---+---+---+---+---+---+---+-----+
| Tab | Q | W | E | R | T | Y | U | I | O | P | [ | ] |  \  |
+------+---+---+---+---+---+---+---+---+---+---+---+--------+
|Esc/C | A | S | D | F | G | H | J | K | L | ; | ' | Enter  |
+-------+---+---+---+---+---+---+---+---+---+---+----------+
| Shift | Z | X | C | V | B | N | M | , | . | / |   Shift  |
+----+----+----+-----------------------------+----+----+----+
|Ctrl|Opt |Cmd |           Space             |Cmd |FN1 |FN2 |Ctrl|
+----+----+----+-----------------------------+----+----+----+----+
```

**Esc/C** = Tap for ESC, Hold for Left Control

### Key Overrides (Active on all layers)
- `Ctrl + H` -> Left Arrow
- `Ctrl + J` -> Down Arrow
- `Ctrl + K` -> Up Arrow
- `Ctrl + L` -> Right Arrow

## Files

- `keymap.c` - Main keymap configuration
- `config.h` - QMK settings (`HOLD_ON_OTHER_KEY_PRESS` for responsive mod-tap)
- `rules.mk` - Build rules (enables KEY_OVERRIDE)
- `README.md` - This file

## Troubleshooting

### Key overrides not working
Ensure `KEY_OVERRIDE_ENABLE = yes` is in `rules.mk` and firmware is compiled and flashed.

