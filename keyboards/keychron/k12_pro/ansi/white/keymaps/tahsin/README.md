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

### Step-by-step flashing instructions

1. **Compile the firmware** (see compilation section above)
   - The compiled firmware will be at: `.build/keychron_k12_pro_ansi_white_tahsin.bin`

2. **Prepare the keyboard:**
   - Unplug the keyboard from your computer
   - Set the mode switch to **cable mode** (physical switch on the keyboard)
   - Remove the **spacebar keycap** (the reset button is underneath)

3. **Enter DFU (bootloader) mode:**
   - Press and hold the **reset button** (under the spacebar)
   - While holding reset, plug in the USB cable
   - Release the reset button after plugging in

4. **Flash with QMK Toolbox:**
   - Open QMK Toolbox application
   - Verify that it shows **"Put Your Keyboard into DFU (Bootloader) Mode"** message (this confirms the keyboard is in bootloader mode)
   - Click **"Open"** and select the compiled `.bin` file (`.build/keychron_k12_pro_ansi_white_tahsin.bin`)
   - Click **"Flash"**
   - Wait for the flashing process to complete
   - The keyboard will automatically restart with the new firmware

5. **Test the keyboard:**
   - Replace the spacebar keycap
   - Test Caps Lock: tap for ESC, hold for Control
   - Test key overrides: Ctrl+H/J/K/L for arrows

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

### Top-left key produces § (section sign) instead of ` (backtick)

**Problem**: The keyboard produces § and ± instead of ` and ~

**Cause**: macOS has incorrectly identified the keyboard as ISO layout (European) instead of ANSI layout (U.S.). This causes macOS to interpret key scancodes differently.

**Solution**: Delete macOS keyboard type cache to force re-detection

1. Disconnect the keyboard from your Mac
2. Open Terminal and run:
   ```bash
   sudo rm /Library/Preferences/com.apple.keyboardtype.plist
   ```
3. Restart your Mac
4. Plug in the keyboard
5. If "Keyboard Setup Assistant" appears, follow the prompts
6. Test that ` and ~ now work correctly

**Alternative**: Manually run Keyboard Setup Assistant:
```bash
/System/Library/CoreServices/KeyboardSetupAssistant.app/Contents/MacOS/KeyboardSetupAssistant
```

This issue typically occurs when moving the keyboard between different Macs.
