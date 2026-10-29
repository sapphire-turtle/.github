<!--
The installation section of an application's README. Replace <app> with the
short name (journal) and drop this comment. README-install.ja.md is the Japanese one. The "Desktop app" section
describes the Velopack package (sapphire-turtle/corundum-framework#51): until an
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
curl -fsSL https://raw.githubusercontent.com/sapphire-turtle/sapphire-framework/main/install/install.sh | sh -s -- <app>
```

```powershell
# Windows
& ([scriptblock]::Create((irm https://raw.githubusercontent.com/sapphire-turtle/sapphire-framework/main/install/install.ps1))) <app>
```

This installs `sapphire-<app>` together with `sapphire-bridge`, which it syncs through, into
`~/.local/bin`. See the
[install script's README](https://github.com/sapphire-turtle/sapphire-framework/tree/main/install)
for the options.

Or, with [cargo-binstall](https://github.com/cargo-bins/cargo-binstall), which downloads the
same release files:

```sh
cargo binstall sapphire-<app> sapphire-bridge
```

### From source

```sh
cargo install sapphire-<app> sapphire-bridge
```
