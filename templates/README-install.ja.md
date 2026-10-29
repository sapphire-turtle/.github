<!--
アプリの README.ja.md のインストールの節。<app> を短い名前（journal）に置き換え、
このコメントは消す。英語版は README-install.md。
「デスクトップ版」の節は Velopack のパッケージ（sapphire-turtle/corundum-framework#51）が前提。
それを出すまでは、デスクトップ版のバイナリを案内する。
-->
## インストール

### デスクトップ版

[最新のリリース](https://github.com/sapphire-turtle/sapphire-<app>/releases/latest)から、
お使いの OS のインストーラーをダウンロードしてください。CLI・app server・MCP サーバーも含まれ、
自動で更新され、`sapphire-<app>` が `PATH` に入ります。下の手順を併用する必要はありません。

### CLI 版のみ（サーバー機・ヘッドレス）

```sh
# Linux, macOS
curl -fsSL https://raw.githubusercontent.com/sapphire-turtle/sapphire-framework/main/install/install.sh | sh -s -- <app>
```

```powershell
# Windows
& ([scriptblock]::Create((irm https://raw.githubusercontent.com/sapphire-turtle/sapphire-framework/main/install/install.ps1))) <app>
```

`sapphire-<app>` と、同期に使う `sapphire-bridge` が `~/.local/bin` に入ります。オプションは
[インストールスクリプトの README](https://github.com/sapphire-turtle/sapphire-framework/tree/main/install)
を見てください。

[cargo-binstall](https://github.com/cargo-bins/cargo-binstall) でも、同じリリースのファイルを入れられます。

```sh
cargo binstall sapphire-<app> sapphire-bridge
```

### ソースから

```sh
cargo install sapphire-<app> sapphire-bridge
```
