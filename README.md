# Termux Clock
[![WIP](https://img.shields.io/badge/%F0%9F%9B%A0-WIP-cyan)](#)
[![GitLab: mirror](https://img.shields.io/badge/GitLab-mirror-blue?labelColor=black&logo=gitlab)](https://gitlab.com/iahmadgad/termux-clock)
[![Rust CI workflow](https://img.shields.io/github/actions/workflow/status/iahmadgad/termux-clock/rust.yml?label=Rust%20CI&logo=rust)](https://github.com/iahmadgad/termux-clock/actions/workflows/rust.yml)
[![Release workflow](https://img.shields.io/github/actions/workflow/status/iahmadgad/termux-clock/release.yml?label=Release&logo=github)](https://github.com/iahmadgad/termux-clock/actions/workflows/release.yml)
[![Lines of code](https://tokei.rs/b1/github/iahmadgad/termux-clock?category=code&label=Lines%20of%20code&style=flat)](#)
[![Latest Tag](https://img.shields.io/github/v/tag/iahmadgad/termux-clock?label=Latest%20tag&sort=semver)](https://github.com/iahmadgad/termux-clock/tags)
[![Downloads](https://img.shields.io/github/downloads/iahmadgad/termux-clock/total?label=Downloads%20(GH))](https://github.com/iahmadgad/termux-clock/releases)
[![PRs: welcome](https://img.shields.io/badge/PRs-welcome-lemon)](https://github.com/iahmadgad/termux-clock/fork)

Termux tool to set alarms & timers headlessly.

# Requirements
- [Termux](https://github.com/termux/termux-app) >= `0.118.1` _(this version solves issue termux/termux-app#3990)_
- [Termux: API](https://github.com/termux/termux-api)
- Termux packages: `termux-api`, `at`, `cronie`

# Install
> [!IMPORTANT]
> both 2 methods install `termux-api` package not the Android app, which needs to be installed manually.
> Read [Installation](https://github.com/termux/termux-api?tab=readme-ov-file#installation) on `termux/termux-api` repo to know how to install the Android app.
## Pre-built Termux packages
- Navigate to [GitHub Releases](https://github.com/iahmadgad/termux-clock/releases), and choose package with your desired version & phone architecture (run `uname -m` in termux to know your phone's architecture).
- copy deb file url & download it using `wget` or `curl`.
- Install the downloaded package with `apt`:
```sh
apt install ./<package>.deb
```
## From source
- Install dependencies
```sh
pkg install termux-api at cronie
```
- Compile and install from the latest commit on `main` branch:
```sh
cargo install --force --git https://github.com/iahmadgad/termux-clock
```
- Or from tag (recommended):
```sh
cargo install --force --git https://github.com/iahmadgad/termux-clock --tag <tag>
```
See [`cargo-install(1)`](https://doc.rust-lang.org/cargo/commands/cargo-install.html) for more `cargo install` options.
# Development
## Requirements 
- [just](https://github.com/casey/just) >= `1.19.0` _(for modules feature)_
- [yq](https://github.com/mikefarah/yq)
- [cross](https://github.com/cross-rs/cross) _(in case you are not building on termux)_
- [termux-create-package](https://github.com/termux/termux-create-package)
- bash

Install just:
```sh
cargo install just
```
Other tools (except bash) will be installed automatically during build process, or when you use `install-tool` recipe.

The `install-tool` recipe is designed to run on an AMD64 Linux machine. If your system does not meet these requirements, it is recommended to install the tools manually.

## Just recipes
List recipes in `justfile`:
```sh
just --list
```
List recipes in module:
```sh
just --list <module>
```
Just recipes are located in `justfile` and `justmodules` directory.

If you don't know just recipes read [just documentation](https://just.systems/man/en/).

# Manpages
Deb packages comes with manpages for `termux-clock`, `termux-clock timer` and `termux-clock alarm`

You can read them by running:
```sh
man termux-clock
man termux-clock-timer
man termux-clock-alarm
```

You can also build these manpages from source with `man` recipe:
```sh
just man
```
Or with `build-man` [xtask](https://github.com/matklad/cargo-xtask) (but ensure you are in the project root directory):
```sh
cargo build-man
```

Generated manpages are located in: `target/manpages`

# Usage
## `timer`
| Option | Description |
| ------ | ----------- |
| `-H, --hours <HOURS>` | add hours to timer length. |
| `-M, --minutes <MINUTES>` | add minutes to timer length. |
| `-S, --seconds <SECONDS>` | add seconds to timer length. |
| `-m, --message <MESSAGE>` | timer message. |
| `-v, --vibrate` | enable vibration. _(not available in android timers, so use it with `--termux` option)_. |
| `-t, --termux` | set timer in termux instead of android alarm clock. |
### Example
```sh
termux-clock timer -M 1 -S 30 -t -m "Timer for 1 minute and 30 seconds in Termux"
```
## `alarm`
| Option | Description |
| ------ | ----------- |
| `-H, --hours <HOURS>` | alarm hour. |
| `-M, --minutes <MINUTES>` | alarm extra minutes. |
| `-d, --days <DAYS>` | days to recurr the alarm, denoted by comma-seperated numbers (e. g. `1,2,3`), where each number corresponds to a weekday, starting from sunday (i. e. sunday is `1`, monday is `2` and so on and so forth). |
| `-m, --message <MESSAGE>` | alarm message. |
| `-v, --vibrate` | enable vibration. |
| `-t, --termux` | set alarm in termux instead of android alarm clock. |
| `--pm` | set alarm hour in pm instead of am. |
### Example
```sh
termux-clock alarm -H 6 -M 30 -d 1,2,3 -v -m "Alarm at 06:30 am 🕡 every Sunday, Monday and Tuesday with vibration enabled 📳"
```
