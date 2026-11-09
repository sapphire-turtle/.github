<!--
The installation section of an application's README. Replace <app> with the
short name (journal) and drop this comment. README-install.ja.md is the Japanese one. The "Desktop app" section
describes the cargo-packager package (sapphire-turtle/corundum-framework#103): until an
application ships one, keep pointing at its bare desktop binary instead.
-->
## Installation

### Desktop app

Download the installer for your platform from the
[latest release](https://github.com/sapphire-turtle/sapphire-<app>/releases/latest). It
includes the CLI, the app server and the MCP server, keeps itself up to date, and puts
`sapphire-<app>` on your `PATH`; nothing below is needed alongside it.

### CLI only (servers, headless machines)

```sh
# Linux, macOS
curl -fsSL https://raw.githubusercontent.com/sapphire-turtle/sapphire-<app>/main/install/install.sh | sh
```

```powershell
# Windows
& ([scriptblock]::Create((irm https://raw.githubusercontent.com/sapphire-turtle/sapphire-<app>/main/install/install.ps1)))
```

This installs `sapphire-<app>` into `~/.local/bin`, after checking the download against its
SHA-256 and, when [minisign](https://jedisct1.github.io/minisign/) is on `PATH`, its signature.
It does not install `sapphire-bridge`, which the app syncs through: on a host that syncs,
install it with
[sapphire-framework's script](https://github.com/sapphire-turtle/sapphire-framework/tree/main/install)
as well. The options are the same as that script's.

Or, with [cargo-binstall](https://github.com/cargo-bins/cargo-binstall), which downloads the
same release files:

```sh
cargo binstall sapphire-<app>-headless sapphire-bridge-headless
```

### From source

```sh
cargo install sapphire-<app>-headless sapphire-bridge-headless
```
