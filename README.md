# Dotfiles

Personal [chezmoi](https://www.chezmoi.io/) source state for a Zsh-based
terminal environment and a Linux [Hyprland](https://hypr.land/) desktop. This
repository is private and uses chezmoi's `symlink` mode: managed, non-template
files in the home directory point back to this checkout.

That means changes to an ordinary source file are live immediately; reload the
shell or affected application to use them. Templates, if added, are rendered
as regular files when `chezmoi apply` runs.

## Included applications

| Application | Managed target | Source format | Notes |
| --- | --- | --- | --- |
| [Zsh](https://www.zsh.org/) | `~/.zshenv`, `~/.config/zsh/` | shell | Sets XDG paths, loads aliases/functions, completion, and optional plugins. |
| [Starship](https://starship.rs/) | `~/.config/starship/starship.toml` | TOML | Eldritch-colored prompt initialized automatically when `starship` is installed. |
| [Ghostty](https://ghostty.org/) | `~/.config/ghostty/` | Ghostty config | Selects the local `Eldritch` terminal theme. |
| [Hyprland](https://wiki.hypr.land/) | `~/.config/hypr/hyprland.lua` | Lua | Linux-only compositor settings, bindings, and laptop controls. |
| [NixOS](https://nixos.org/) | Zsh helper only | shell | Provides rebuild and ASUS Duo container shortcuts; it does not manage a NixOS system configuration. |

Several prompt glyphs expect a [Nerd Font](https://www.nerdfonts.com/). The
Hyprland configuration invokes `kitty`, `dolphin`, `noctalia`, `wpctl`,
`brightnessctl`, and `playerctl`; install or replace those commands to suit the
machine.

## Repository layout

```text
.
├── dot_zshenv                         -> ~/.zshenv
├── dot_config/                        -> ~/.config/
│   ├── zsh/
│   │   ├── dot_zshrc                  -> ~/.config/zsh/.zshrc
│   │   ├── *.aliases                  # shell aliases loaded automatically
│   │   └── *.functions                # shell functions loaded automatically
│   ├── starship/
│   │   ├── starship.toml              # active prompt configuration
│   │   └── themes/                    # alternative Eldritch TOML themes
│   ├── ghostty/
│   │   ├── config.ghostty             # selects the Eldritch theme
│   │   └── themes/Eldritch            # local Ghostty theme definition
│   └── hypr/hyprland.lua              # Linux-only Hyprland configuration
├── .chezmoitemplates/                 # reusable template fragments; never installed
├── .chezmoiignore                     # OS- and hostname-dependent exclusions
└── docs/chezmoi.toml.example          # local chezmoi configuration example
```

Chezmoi source names encode their destination: `dot_` becomes `.`. For
example, `dot_config/ghostty/config.ghostty` installs as
`~/.config/ghostty/config.ghostty`, and `dot_config/zsh/dot_zshrc` installs as
`~/.config/zsh/.zshrc`.

## Install on a new machine

Install [chezmoi](https://www.chezmoi.io/install/) with the system package
manager, ensure the machine's SSH key can read this private repository, then
clone it into chezmoi's default source directory:

```sh
git clone git@github.com:robbymoses/chezmwuah.git ~/.local/share/chezmoi
```

Create `~/.config/chezmoi/chezmoi.toml` with the following local setting:

```toml
mode = "symlink"
```

[`docs/chezmoi.toml.example`](docs/chezmoi.toml.example) includes the same
setting and a place for non-sensitive per-machine template data. If the source
checkout is somewhere else, set its absolute path with `sourceDir` in that
local file.

Preview, then apply the desired state:

```sh
chezmoi apply --dry-run --verbose
chezmoi apply
```

For later updates, edit this repository and use `chezmoi diff` to inspect the
desired changes before applying. Reload Zsh (`exec zsh`) or restart the desktop
application after modifying its configuration.

## Shell configuration

`~/.zshenv` sets `ZDOTDIR=$HOME/.config/zsh` and the standard XDG data, cache,
and configuration directories. Zsh then reads `~/.config/zsh/.zshrc`.

The `.zshrc` enables completion and history sharing, uses Emacs-style key
bindings, and sources every `*.aliases` and `*.functions` file in its
directory. Missing optional files are harmless, which keeps the configuration
portable across macOS and Linux. It initializes [Starship](https://starship.rs/)
only when the executable is present, and loads Antidote only when
`~/.config/zsh/lib/antidote.zsh` exists.

| File | Scope | Provides |
| --- | --- | --- |
| `common.aliases` | all hosts | `..`, `...`, and `....` navigation aliases |
| `chezmoi.aliases` | all hosts | `chzm` (open source checkout) and `chzm-a` (`chezmoi apply`) |
| `nix.functions` | all hosts | `rebuild`, which validates the current Git-backed NixOS flake before switching to the current hostname |
| `asus-duo.aliases` | `asus-duo` only | NixOS rebuild and client-container aliases, including Evereve shortcuts |
| `asus-duo.functions` | `asus-duo` only | container listing, status, restart, command execution, and update functions |
| `screenpad.aliases` / `screenpad.functions` | `asus-duo` only | enable, disable, toggle, inspect, and reposition the `eDP-2` ScreenPad |

The ASUS Duo rebuild aliases expect a flake at `$HOME/blueprint`. The generic
`rebuild` function instead operates on the Git repository containing the
current directory and refuses to run while required flake files are untracked.

## Terminal and prompt

Ghostty uses the local `Eldritch` theme. Its palette is also the basis for the
active Starship configuration, which shows directory, user and hostname, Git
state, common language/runtime versions, containers, package version, command
duration, and time.

Alternative complete Starship themes live under
`dot_config/starship/themes/`:

| File | Style |
| --- | --- |
| `colors.toml` | shared Eldritch color palette |
| `eldritch-spaceship.toml` | icon-rich, two-line Spaceship-style prompt |
| `eldritch-pure.toml` | minimal Pure-inspired prompt |
| `eldritch-powerline.toml` | segmented Powerline-style prompt |

`starship.toml` is the active file. Treat the files under `themes/` as source
examples for a deliberate configuration change; do not edit the generated
home-directory path independently in symlink mode.

## Hyprland configuration

`hyprland.lua` is installed only on Linux. It configures `eDP-1` as the primary
display and defines `eDP-2` (the ASUS Duo ScreenPad Plus) disabled by default.
It starts Noctalia, uses the dwindle layout, enables animations and blur, and
defines keyboard, pointer, gesture, multimedia, and window rules.

| Binding | Action |
| --- | --- |
| `Super + Q` | open terminal (`kitty`) |
| `Super + C` | close active window |
| `Super + E` | open file manager (`dolphin`) |
| `Super + Space` | toggle Noctalia launcher |
| `Super + V` | toggle active window floating |
| `Super + P` | toggle pseudo-tiled mode |
| `Super + J` | toggle dwindle split |
| `Super + Arrow` | move focus |
| `Super + 0–9` | select workspace 10 or 1–9 |
| `Super + Shift + 0–9` | move active window to workspace 10 or 1–9 |
| `Super + S` / `Super + Shift + S` | toggle / move window to the `magic` special workspace |
| `Super + mouse wheel` | move through workspaces |
| `Super + left/right mouse drag` | move / resize a window |

The media keys control PipeWire volume through `wpctl`, brightness through
`brightnessctl`, and playback through `playerctl`. Three-finger horizontal
swipes change workspaces.

## Platform, host, and template rules

`.chezmoiignore` is a chezmoi template that controls what is installed:

| Condition | Excluded files |
| --- | --- |
| non-Linux | `.config/hypr/**` |
| hostname is not `asus-duo` | ASUS Duo and ScreenPad Zsh aliases/functions |
| every host | repository `README.md` and `docs/**` |

If the ASUS laptop hostname changes, update `asus-duo` in `.chezmoiignore`.
Check the current name with `hostname`.

Use `{{ .chezmoi.homeDir }}` only in `*.tmpl` files that need a rendered,
literal home path. Prefer `$HOME` or `${HOME}` in shell-compatible files so
they remain portable symlinks.

## Sensitive data

Do not commit passwords, API tokens, private keys, recovery codes, or other
secrets. Keep them in a password manager or untracked local files. Reusable
configuration should be reviewed and sanitized before being published in a
separate repository; public Git history is difficult to retract.
