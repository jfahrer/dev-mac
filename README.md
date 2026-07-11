# dev-mac

Bootstrap a personal Mac with Homebrew, public dotfiles, `rcm`, and `mise`.
Scripts are safe to rerun: they do not clean packages, overwrite conflicting
files, reset Git changes, or restart the Mac.

The bootstrap uses macOS's built-in `/bin/bash` as the default login shell.

## Install

Run the public entry point on a new Mac:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/jfahrer/dev-mac/main/install)"
```

To inspect everything before running it:

```sh
git clone https://github.com/jfahrer/dev-mac.git ~/.dev-mac
less ~/.dev-mac/script/bootstrap
~/.dev-mac/script/bootstrap
```

The bootstrap may stop for the Xcode Command Line Tools dialog. Let it finish,
then rerun the same command. Homebrew, `sudo`, FileVault, and the recovery
contact also require visible interaction.

After bootstrap, follow [MANUAL.md](MANUAL.md). You can run the read-only
verification again at any time with `~/.dev-mac/script/verify`.

## Overrides

- `DOTFILES_REPO_URL` changes the dotfiles clone URL.
- `DOTFILES_DIR` changes the dotfiles checkout location.
- `RECOVERY_CONTACT` supplies the email address or phone shown at login. If it
  is omitted, the security script prompts for it. Never commit the value here.
- `DEV_MAC_REPO_URL` and `DEV_MAC_DIR` can point the public installer at a fork
  or a different checkout directory.

The dotfiles repository owns shell/editor configuration, its `rcm` setup, and
`~/.config/mise/config.toml`. Project tool versions belong in project-local
`mise.toml` or `.tool-versions` files. This repository never runs Homebrew
cleanup automatically.
