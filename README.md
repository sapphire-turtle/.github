# sapphire-turtle/.github

The CI, release and repository setup every sapphire-turtle Rust repository shares: reusable
workflows in [`.github/workflows/`](.github/workflows/), the caller workflows in
[`workflow-templates/`](workflow-templates/) (also offered under **Actions → New workflow** in
every repository of the organization), and the other files a repository copies in under
[`templates/`](templates/).

It is its own repository, rather than part of sapphire-framework, so that repositories that
do not build on sapphire (asteria, corundum-framework, grain-id) do not depend on it for CI.

## Reusable workflows

Call them as `sapphire-turtle/.github/.github/workflows/<file>@v1`.

| Workflow | What it does |
|---|---|
| [`rust-ci.yml`](.github/workflows/rust-ci.yml) | Pull request CI: `fmt`, then clippy and the tests on Linux x86_64 (`test`) and on Linux aarch64, macOS and Windows (`test-platforms`) |
| [`release-plz.yml`](.github/workflows/release-plz.yml) | release-plz `release` and `release-pr`, with `RELEASE_PLZ_TOKEN`; outputs which packages were released and their tags |
| [`release-binary.yml`](.github/workflows/release-binary.yml) | Builds one binary for the four release platforms and attaches `<asset>-<os>-<arch>[.exe]` to a release |
| [`cargo-deny.yml`](.github/workflows/cargo-deny.yml) | `cargo deny check` against the repository's `deny.toml` |

Each one's inputs are documented at the top of its file.

## Versions

Callers pin a major version tag, `@v1`, so a change here reaches a repository only when it
is tagged, and a breaking one only when that repository moves to the next major.

- `v1.x.y` tags each release; `v1` is moved to the latest `v1.x.y`.
- A change that keeps every caller working (a new input with a default that preserves
  today's behaviour, a fix, an action bump) is a new `v1.x.y`; move `v1` to it.
- A change that does not (a removed or renamed input, a changed release file name) is
  `v2.0.0` and a new `v2` tag. Leave `v1` where it is; each repository moves when it is
  ready. Update `workflow-templates/` to the new major in the same change.

```sh
git tag v1.2.0 && git tag -f v1 v1.2.0
git push origin v1.2.0 && git push -f origin v1
```

## Conventions

- **Toolchain**: every repository pins the same `rust-toolchain.toml` channel and they are
  bumped together. CI installs nothing else (a repository whose `rustfmt.toml` needs nightly
  options sets `rustfmt-toolchain: nightly`).
- **Required checks**: each repository's `main` ruleset requires `ci / fmt` and `ci / test`
  (the caller job is named `ci`).
- **Tags**: an application's CLI makes the `v{version}` tag and GitHub release; its desktop
  app is in the same version group and is released with it, onto the same release
  ([`templates/release-plz.toml`](templates/release-plz.toml)). Libraries others depend on
  (`-core`, `-api`, `-mcp`) keep their own semver and make no tags.
- **Release files**: `<binary>-linux-x86_64`, `-linux-aarch64`, `-macos-aarch64`,
  `-windows-x86_64.exe`. This is a contract with `sapphire-framework-update` (the `update`
  command), sapphire-framework's `install/install.sh` and `install.ps1`, and
  `cargo binstall`: change it only as a new major of `release-binary.yml`, together with them.
- **Distribution**: the CLI (headless use) is installed with the install scripts or
  `cargo binstall`; the desktop app ships as a Velopack package
  (sapphire-turtle/corundum-framework#51). An application's README follows
  [`templates/README-install.md`](templates/README-install.md).
- **cargo binstall**: every crate with a released binary carries
  `[package.metadata.binstall]` pointing at the release files, so `cargo binstall` installs
  the release instead of building it: copy the block from sapphire-framework's
  `bridge/Cargo.toml`.
- **Licenses**: libraries, CLIs and MCP servers are `MIT OR Apache-2.0`. An application's
  desktop and mobile apps (`<app>-desktop`, `<app>-mobile`) and the GUI library they share
  (`<app>-gui`) are `GPL-3.0-or-later` with an app store exception (sapphire-journal's
  `desktop/LICENSE.App-Store-Exception` is the model): the mobile apps are sold, and the
  desktop app shares much of their code. `sapphire-framework-gui` is a framework library and
  stays `MIT OR Apache-2.0`. So an application repository has no
  license file at its root; each component's directory carries its own, and the README lists
  them. A repository that is all libraries keeps `LICENSE-MIT` / `LICENSE-APACHE` at the root.
- **Secrets**: `RELEASE_PLZ_TOKEN` and, for a repository that publishes to crates.io,
  `CARGO_REGISTRY_TOKEN`.
- **Dependabot**: Cargo patch updates are grouped into one `cargo-patch` pull request.
  Dependabot pull requests are reviewed and merged by hand; nothing auto-merges them.
- **Labels**: `release` (release PRs), and `dependencies`, `github_actions`, `rust`
  (Dependabot).

## Setting up a repository

Add the workflows from **Actions → New workflow** (or copy them from
[`workflow-templates/`](workflow-templates/)), and the rest from [`templates/`](templates/):

| File | Notes |
|---|---|
| `.github/workflows/ci.yml` | Set `linux-packages`, and `packages` / `post-test` if needed |
| `.github/workflows/cargo-deny.yml` | As is |
| `.github/workflows/release-plz.yml`, `release-binaries.yml` | For an application with a CLI and a desktop app; replace `<app>` |
| `release-plz.toml` | Likewise; list the repository's library crates |
| `.github/dependabot.yml` | As is |
| `rust-toolchain.toml` | As is |
| `deny.toml` | Add the repository's own `ignore` and `exceptions` entries, including the desktop app's GPL one |
| `.gitignore` | Append repository-specific entries below the marker |

Then create the `main` ruleset (no deletion, no force push, pull request required, the
required checks above).

## License

MIT OR Apache-2.0.
