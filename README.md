# Dotfiles

Personal [chezmoi](https://www.chezmoi.io/) source state for a Zsh terminal
environment and a Linux Hyprland desktop. The local chezmoi configuration uses
`mode = "symlink"`, so ordinary source files are live through their managed
home-directory symlinks; reload the affected application after changing one.

## Configuration index

Detailed explanations, commands, and keybindings live beside the configuration
they describe. Documentation is excluded from the desired home-directory state.

| Area | Managed target | Documentation |
| --- | --- | --- |
| Zsh | `~/.zshenv`, `~/.config/zsh/` | [`dot_config/zsh`](dot_config/zsh/README.md) |
| Starship | `~/.config/starship/` | [`dot_config/starship`](dot_config/starship/README.md) |
| Ghostty | `~/.config/ghostty/` | [`dot_config/ghostty`](dot_config/ghostty/README.md) |
| Hyprland | `~/.config/hypr/hyprland.lua` | [`dot_config/hypr`](dot_config/hypr/README.md) |
| Chezmoi setup and templates | local configuration only | [`docs`](docs/README.md), [`.chezmoitemplates`](.chezmoitemplates/README.md) |

[`dot_config/README.md`](dot_config/README.md) describes the shared
`~/.config` source layout. In particular, the Zsh document owns the explanation
of the home-level `dot_zshenv`: it sets `ZDOTDIR`, which makes Zsh load the
configuration in `~/.config/zsh/`.

## Install on a new machine

Install [chezmoi](https://www.chezmoi.io/install/), ensure the machine's SSH
key can read this private repository, then clone it into chezmoi's default
source directory:

```sh
git clone git@github.com:robbymoses/chezmwuah.git ~/.local/share/chezmoi
```

Create `~/.config/chezmoi/chezmoi.toml` with:

```toml
mode = "symlink"
```

[`docs/chezmoi.toml.example`](docs/chezmoi.toml.example) provides the same
setting plus a place for non-sensitive per-machine template data. If the source
checkout lives elsewhere, set its absolute path with `sourceDir` in that local
file.

Preview and apply the desired state:

```sh
chezmoi apply --dry-run --verbose
chezmoi apply
```

Use `chezmoi diff` before later applies. Never commit passwords, tokens,
private keys, recovery codes, or other secrets.

## Platform and host rules

`.chezmoiignore` excludes Hyprland on non-Linux systems and excludes the ASUS
Duo and ScreenPad helpers unless the hostname is `asus-duo`. Rename that value
if the laptop hostname changes. Repository documentation is excluded on every
host.
