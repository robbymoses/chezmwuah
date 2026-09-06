# Starship

`starship.toml` is the active prompt configuration at
`~/.config/starship/starship.toml`. Zsh initializes it only when the `starship`
executable is available.

The active Eldritch prompt shows the current directory, user and host, Git
state, supported language and runtime versions, containers, package version,
command duration, and time. Some glyphs require a
[Nerd Font](https://www.nerdfonts.com/).

Alternative Eldritch prompt designs and their palette are documented in
[`themes/README.md`](themes/README.md). Treat those files as complete examples:
copy the desired configuration into `starship.toml` deliberately rather than
editing the managed target outside this source checkout.
