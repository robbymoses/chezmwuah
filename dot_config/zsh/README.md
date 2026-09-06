# Zsh

`dot_zshenv` maps to `~/.zshenv` and belongs to this configuration area even
though it is a home-level file. It sets `ZDOTDIR=$HOME/.config/zsh` plus the
standard XDG paths, so Zsh subsequently reads `dot_zshrc` as
`~/.config/zsh/.zshrc`.

## Shell behavior

The `.zshrc` enables completion, shared history, automatic `cd`, and
Emacs-style keybindings. Up and down arrows search history substrings. It
sources every `*.aliases` and `*.functions` file in this directory; an empty
set is safe, which keeps the setup portable. Antidote and Starship are loaded
only when installed.

## Commands

| File | Availability | Provides |
| --- | --- | --- |
| `common.aliases` | every host | `..`, `...`, `....` directory navigation |
| `chezmoi.aliases` | every host | `chzm` to enter the source checkout; `chzm-a` to apply chezmoi |
| `nix.functions` | every host | `rebuild`, which validates and switches the NixOS flake containing the current directory |
| `asus-duo.aliases` / `asus-duo.functions` | hostname `asus-duo` | NixOS rebuild and client-container helpers |
| `screenpad.aliases` / `screenpad.functions` | hostname `asus-duo` | enable, disable, inspect, toggle, and position the `eDP-2` ScreenPad |

The ASUS Duo rebuild aliases expect a flake in `$HOME/blueprint`. `rebuild`
instead operates on the current Git checkout and refuses to continue when it
contains untracked files needed by the flake.

The host-specific helper files are excluded on other hosts by
[`.chezmoiignore`](../../.chezmoiignore).
