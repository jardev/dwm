# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## CRITICAL: NEVER TOUCH config.h

**DO NOT READ, EDIT, DELETE, OR OVERWRITE `config.h` UNDER ANY CIRCUMSTANCES.**

This file contains years of personal customization (keybindings, rules, appearance settings). It is NOT auto-generated and must NEVER be modified by Claude.

- Only edit `config.def.h` for patches
- NEVER run `rm config.h`
- NEVER run `make clean && make` (this overwrites config.h)
- After patching, tell the user to manually merge changes from config.def.h into their config.h
- If the user needs to rebuild, use `make` only (without removing config.h)

## Build Commands

```bash
make              # Build dwm (safe - preserves config.h)
sudo make install # Install to /usr/local/bin (configurable in config.mk)
```

To apply patches: edit `config.def.h` and `dwm.c`, then tell the user what changes they need to manually add to their `config.h`.

## Architecture

dwm is a dynamic window manager for X11 written in C. The codebase follows suckless philosophy: minimal, configurable through source code.

### Core Files

- **dwm.c** - Main window manager logic, X event handling, client management
- **drw.c/drw.h** - Drawing abstraction layer (fonts, colors, rendering via Xft)
- **util.c/util.h** - Utility functions (die, ecalloc) and macros (MAX, MIN, LENGTH)
- **config.def.h** - Default configuration template; copied to config.h on first build

### Key Data Structures (dwm.c)

- `Client` - Window state: position, size, tags, floating/fullscreen state, linked list pointers
- `Monitor` - Display state: screen geometry, window area, tag state, client list, bar window
- `Layout` - Layout symbol and arrange function pointer

### Configuration

All configuration is done in `config.h` (generated from `config.def.h`):
- Appearance: borders, fonts, colors
- Tagging rules: per-application tag assignment and floating state
- Layouts: tile, floating, monocle
- Key bindings: defined as `Key` structs with modifier, keysym, function, and argument
- Mouse bindings: defined as `Button` structs

### Applied Patches

This build includes:
- **systray** - System tray support (NetSystemTray atoms, systray settings in config)
- **ewmhtags** - EWMH tag/desktop support (NetDesktopNames, NetCurrentDesktop atoms)
- **sticky** - Sticky windows visible on all tags (issticky client flag, togglesticky function)
- **fakefullscreen** - Per-client fake fullscreen (isfakefullscreen client flag)
- **ignore_transient_windows** - Per-client ignore transient hint (ignoretransient in rules)
- **tab** - i3-like tab bar for monocle mode (Mod+w to toggle, click tabs to focus)
- **deck** - Deck layout (stack area shows one window at a time like monocle)
- **cyclelayouts** - Cycle through layouts (Mod+Ctrl+,/.)
- **scratchpads** - Toggle-able floating terminals (Mod+` for terminal, Mod+u for ranger)
- **fullgaps** - Gaps between windows (Mod+- decrease, Mod+= increase, Mod+Shift+= reset)
