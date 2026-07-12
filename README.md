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

## Automated changes

The public `install` entry point clones this repository to `~/.dev-mac` and
runs `script/bootstrap`. The bootstrap performs all of the following actions.

### macOS and developer tools

- Refuses to run on operating systems other than macOS.
- Requests the Xcode Command Line Tools when they are missing and stops so the
  installation can be completed before the bootstrap is rerun.
- Accepts the Xcode license when full Xcode is installed.
- Changes the current account's default login shell to macOS's built-in
  `/bin/bash`.
- Installs Homebrew when it is missing.
- Installs every available macOS software update without automatically
  restarting the Mac. The script reports when a restart is required.

### Homebrew packages and applications

`brew bundle` installs the following formulae declared in `Brewfile`:

- `ack`, `bash-completion`, `cmake`, `colima`, `docker`, `docker-buildx`,
  `docker-compose`, `docker-credential-helper`, `dockerfile-language-server`,
  `dockutil`, `gh`, `git`, `gpg`, `htop`, `inetutils`, `lazygit`, `libpq`,
  `lynx`, `mas`, `mise`, `moreutils`, `neovim`, `nmap`, `opencode`, `p7zip`,
  `rcm`, `ripgrep`, `shellcheck`, `terminal-notifier`, `the_silver_searcher`,
  `tldr`, `tmux`, `tree`, `vim`, `watch`, and `ykman`.

It also installs these casks and fonts:

- 1Password, 1Password CLI, Alfred, Brave Browser, Codex, Dash, Firefox,
  Ghostty, Google Chrome, Google Drive, GPG Suite without Mail, Rectangle,
  ScreenFlow, Slack, Sublime Text, SwitchResX, and Yubico Authenticator.
- DejaVu Sans Mono Nerd Font, Fira Code Nerd Font, and JetBrains Mono.

### Shell, dotfiles, tmux, and language tools

- Clones Bash-it to `~/.bash-it` with a shallow checkout. An existing checkout
  and any local changes in it are preserved.
- Clones the public dotfiles repository to `~/.dotfiles`. Its `script/setup`
  runs `rcup`, which links the repository-managed shell, Git, editor, tmux,
  Ghostty, mise, Neovim, opencode, and other configuration into the home
  directory according to its `rcrc`. Existing dotfiles are handled by `rcm`.
- Clones tmux plugin manager to `~/.tmux/plugins/tpm` and runs its
  `install_plugins` command. This installs the plugins declared by the
  dotfiles-managed `.tmux.conf`.
- Runs `mise install` using the dotfiles-managed global configuration. The
  current configuration installs Go latest, Node 24.3.0, Ruby 4.0.1, and Yarn
  1.22.22.

### Security

- Enables Touch ID authentication for `sudo` in `/etc/pam.d/sudo_local`,
  creating that file from Apple's template when necessary.
- Requires the account password immediately after the screen locks.
- Enables the macOS application firewall.
- Sets the login-window message to `Found this computer? Please contact
  <RECOVERY_CONTACT>.`, prompting for the contact when the environment
  variable is not supplied.
- Enables FileVault for the current user when it is off and verifies that a
  personal recovery key exists. When FileVault is newly enabled, the recovery
  key is written with mode `0600` to
  `~/Desktop/FileVault Recovery Key.txt`; it must then be moved to secure
  storage as described in `MANUAL.md`.

### Keyboard and macOS preferences

- Uses the F1–F12 keys as standard function keys.
- Sets keyboard repeat to the fastest configured value (`KeyRepeat = 1`).
- Enables full keyboard access for controls (`AppleKeyboardUIMode = 3`).
- Maps the built-in keyboard's Caps Lock key to Control, both persistently and
  for the current login session.
- Changes Spotlight search to Control-Option-Space.
- Disables the Mission Control Control-Up and Control-Down shortcuts.
- Prevents Time Machine from offering newly attached disks for backup.
- Displays the menu-bar clock as abbreviated weekday, month, day, time with
  seconds, and AM/PM.
- Shows the input-source menu at the login window.
- Restarts `cfprefsd`, `pbs`, Spotlight, SystemUIServer, and Dock so changed
  preferences are reloaded. It does not restart the Mac.

### Dock

- Removes Maps, Mail, Music, Pages, Numbers, TV, Keynote, Phone, and Games from
  the Dock when present.
- Pins Brave Browser, Ghostty, and 1Password to the Dock.

### Verification and reruns

At the end, `script/verify` checks Homebrew dependencies, the login shell,
Bash-it, TPM, dotfile links, mise discovery, Dock contents, Caps Lock mapping,
security settings, FileVault, macOS defaults, symbolic shortcuts, and pending
software updates. Verification does not repair failed outcomes; it reports
them and exits unsuccessfully.

Existing Git checkouts and their local changes are preserved. The scripts do
not clean packages, reset repositories, overwrite a non-Git path expected to
contain a checkout, or restart the Mac.

## Overrides

- `DOTFILES_REPO_URL` changes the dotfiles clone URL.
- `DOTFILES_DIR` changes the dotfiles checkout location.
- `BASH_IT_REPO_URL` changes the Bash-it clone URL.
- `BASH_IT_DIR` changes the Bash-it checkout location.
- `TPM_REPO_URL` changes the tmux plugin manager clone URL.
- `TPM_DIR` changes the tmux plugin manager checkout location.
- `RECOVERY_CONTACT` supplies the email address or phone shown at login. If it
  is omitted, the security script prompts for it. Never commit the value here.
- `DEV_MAC_REPO_URL` and `DEV_MAC_DIR` can point the public installer at a fork
  or a different checkout directory.

The dotfiles repository owns shell/editor configuration, its `rcm` setup,
Bash-it activation and component choices, and global mise configuration.

This repository installs Bash-it at `~/.bash-it` without modifying local
configs. It also installs the tmux plugin manager and the plugins declared by
the dotfiles-managed tmux configuration.
