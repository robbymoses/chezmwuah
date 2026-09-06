# Hyprland

`hyprland.lua` maps to `~/.config/hypr/hyprland.lua` and is installed only on
Linux. It uses the dwindle layout, starts Noctalia, configures `eDP-1` as the
primary display, and leaves the ASUS Duo `eDP-2` ScreenPad disabled by default.
The ScreenPad helpers are described in the [Zsh documentation](../zsh/README.md).

Required commands include `kitty`, `dolphin`, `noctalia`, `wpctl`,
`brightnessctl`, and `playerctl`; replace any of these in the Lua configuration
to suit the machine.

## Keybindings

| Binding | Action |
| --- | --- |
| `Super + Q` / `C` / `E` | open terminal / close focused window / open file manager |
| `Super + M` | run `hyprshutdown` when available, otherwise exit Hyprland |
| `Super + Space` | toggle the Noctalia launcher |
| `Super + V` / `P` / `J` | toggle floating / pseudo-tiled / dwindle split |
| `Super + Arrow` | move focus |
| `Super + 0–9` | select workspace 10 or 1–9 |
| `Super + Shift + 0–9` | move active window to workspace 10 or 1–9 |
| `Super + S` / `Super + Shift + S` | toggle / move a window to special workspace `magic` |
| `Super + mouse wheel` | move through workspaces |
| `Super + left/right mouse drag` | move / resize a window |
| media, volume, and brightness keys | control playback, PipeWire volume, and display brightness |

Three-finger horizontal swipes change workspaces. Restart Hyprland after
changing settings that do not apply dynamically, notably permissions.
