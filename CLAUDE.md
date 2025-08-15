# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a ZMK keyboard firmware configuration for a Corne 3x5+3 (36-key) keyboard using nice_nano_v2 controllers with nice_view displays.

## Key Files and Structure

- `config/corne.keymap` - Main keymap definition with 4 layers (DEFAULT, NUM, SYM, FUNC)
- `config/corne.conf` - Configuration options for display, sleep, and Bluetooth
- `config/west.yml` - West manifest for ZMK dependencies
- `build.yaml` - GitHub Actions build matrix defining hardware combinations
- `docs/assets/keymap.png` - Visual representation of the keymap layout

## Build Process

Builds are handled automatically via GitHub Actions when code is pushed. The workflow uses `zmkfirmware/zmk/.github/workflows/build-user-config.yml@main` and builds firmware for:
- Left half: nice_nano_v2 + corne_left + nice_view_adapter + nice_view
- Right half: nice_nano_v2 + corne_right + nice_view_adapter + nice_view

Built firmware files are available as artifacts from GitHub Actions runs.

## Keymap Architecture

The keymap uses a 4-layer system:
- **DEFAULT (0)**: Base QWERTY with home row mods (Alt/Ctrl/Shift on ASDF/JKL;)
- **NUM (1)**: Navigation arrows, page up/down, numbers with numpad layout
- **SYM (2)**: Symbols and special characters
- **FUNC (3)**: Function keys, media controls, Bluetooth management, system controls

### Custom Behaviors
- Home row mods (`hm`) with 200ms tapping term and tap-preferred flavor
- Tap dance (`td1`) for Ctrl+Tab/Escape
- Combos for caps word (16+19 keys) and vim quit (1+2 keys)
- Custom vim quit macro (ESC + :wq)

### Layer Access
- NUM layer: Hold backspace key
- SYM layer: Hold delete key  
- FUNC layer: Access via NUM+SYM layer combination

## Configuration Options

In `corne.conf`:
- Display enabled (`CONFIG_ZMK_DISPLAY=y`)
- Deep sleep after 1 hour (`CONFIG_ZMK_IDLE_SLEEP_TIMEOUT=3600000`)
- Enhanced Bluetooth transmission power (`CONFIG_BT_CTLR_TX_PWR_PLUS_8=y`)
- RGB underglow disabled by default (commented out)

## Development Workflow

1. Modify `config/corne.keymap` for layout changes
2. Adjust `config/corne.conf` for firmware features
3. Update `build.yaml` if hardware configuration changes
4. Push changes to trigger automatic firmware builds
5. Download firmware from GitHub Actions artifacts
6. Flash firmware to keyboard halves using appropriate tools

## Key Customization Areas

- Timing values (tapping-term-ms) in behaviors section
- Layer definitions in keymap section
- Combo key positions and timeouts
- Macro definitions for custom key sequences
- Configuration flags in corne.conf for hardware features