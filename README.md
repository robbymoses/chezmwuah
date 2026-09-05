# Dotfiles

Personal [chezmoi](https://www.chezmoi.io/) source repository for shell,
terminal, prompt, and desktop configuration. It is intended to remain private.

This repository uses chezmoi's `symlink` mode: ordinary managed files are
symlinked from the checkout, so edits made here take effect immediately after
the affected application or shell is reloaded. Chezmoi templates are the
exception; they are rendered to regular files when `chezmoi apply` runs.

## Included configuration

* Zsh environment and configuration, including shared navigation, chezmoi, and
  Nix helpers.
* Ghostty with the Eldritch theme.
* Starship with Eldritch prompt themes.
* Hyprland configuration on Linux.
* ASUS Duo container and ScreenPad helpers only on the `asus-duo` host.

## Layout

* `dot_config/` installs to `~/.config/`.
* `dot_zshenv` installs to `~/.zshenv`, which sets `ZDOTDIR` to
  `~/.config/zsh`.
* `.chezmoitemplates/` contains reusable template fragments; it is not
  installed itself.
* `docs/` contains repository documentation and is not installed.

Chezmoi source names are deliberate: a `dot_` prefix becomes `.` in the target
path. For example, `dot_config/ghostty/config.ghostty` becomes
`~/.config/ghostty/config.ghostty`.

## Set up a new machine

Install chezmoi with the system package manager, ensure your SSH key has
access to this private repository, then clone it to chezmoi's default source
directory:

```sh
git clone git@github.com:robbymoses/chezmwuah.git ~/.local/share/chezmoi
```

Create `~/.config/chezmoi/chezmoi.toml` with:

```toml
mode = "symlink"
```

The default source directory is `~/.local/share/chezmoi`; if you choose a
different checkout location, set its absolute path as `sourceDir` in that same
local configuration file. See
[`docs/chezmoi.toml.example`](docs/chezmoi.toml.example) for a starting point
that also accommodates non-sensitive per-machine template data.

Preview and install the desired state:

```sh
chezmoi apply --dry-run --verbose
chezmoi apply
```

Before applying later changes, run `chezmoi diff`. For ordinary non-template
files, edit the source file in this checkout and reload the relevant program;
there is no need to re-apply it.

## Platform and host conditions

`.chezmoiignore` is a chezmoi template. Hyprland files are installed only on
Linux. ASUS Duo and ScreenPad helper files are installed only when the hostname
is `asus-duo`; update that value in `.chezmoiignore` if the machine is renamed.

Use `{{ .chezmoi.homeDir }}` only in a `*.tmpl` file when a configuration needs
the literal home path. Prefer `$HOME` or `${HOME}` in shell-compatible files so
they can remain symlinks.

## Sensitive data

This private repository can hold personal aliases and machine-specific
configuration, but never passwords, API tokens, private keys, or recovery
codes. Keep secrets in a password manager or untracked local files.

If a configuration becomes reusable, publish a reviewed and sanitized copy in
a separate repository rather than mirroring this one. Public Git history is
difficult to retract.
