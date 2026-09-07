# Noctalia

`config.toml` maps to `~/.config/noctalia/config.toml`. Hyprland starts
Noctalia; see the [Hyprland configuration](../hypr/README.md) for its launcher
binding and dependency list.

It defines a compact, floating top bar that reserves space for tiled windows:

| Area | Widgets |
| --- | --- |
| Left | clock, volume, Bluetooth, and audio visualizer |
| Center | workspaces |
| Right | network, brightness, battery, Control Center, notifications, and session controls |

The audio visualizer remains visible while idle and opens the media page in
Control Center when clicked. Network labels are hidden; battery uses a glyph
and percentage label.

Noctalia hot-reloads most changes. Validate this source file before applying it:

```sh
noctalia config validate dot_config/noctalia/config.toml
```

The Settings UI writes its own overrides to
`~/.local/state/noctalia/settings.toml`, which take precedence over this
dotfiles-managed configuration. If a GUI change appears to override a value
here, inspect that state file or reset the matching setting in Noctalia.

The bar uses Noctalia's built-in widgets. To customize its layout, change the
`start`, `center`, or `end` arrays; use named `[widget.*]` sections for
widget-specific options. Middle-clicking a bar widget opens its Settings page.
