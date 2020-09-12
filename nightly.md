# nightlyバージョンのRustインストール
ただRocketの[Quick Start]["https://rocket.rs/v0.4/guide/quickstart/"]を試したかっただけなのに、
[Rocket公式のnightlyバージョンのインストールガイド]["https://rocket.rs/v0.4/guide/getting-started/"]に従ったらエラー出たためメモ。
(macOS 10.15.5)

# 問題
最新のnightlyバージョンのRustがインストールできない。

# 解決策

1.[ここ]["https://rust-lang.github.io/rustup-components-history/x86_64-apple-darwin.html"]でrustupのtoolchainの最新版を確認。
2.`rustup default nightly`の代わりに以下のコマンドを実行。

`rustup toolchain install nightly-20xx-yy-zz`
`rustup default nightly-20xx-yy-zz-x86_64-apple-darwin`

これでOK!

# 記録

公式ドキュメントに従い以下のコマンドでインストール

```
>curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

info: downloading installer

Welcome to Rust!

This will download and install the official compiler for the Rust
programming language, and its package manager, Cargo.

Rustup metadata and toolchains will be installed into the Rustup
home directory, located at:

  /Users/terauchishunsuke/.rustup

This can be modified with the RUSTUP_HOME environment variable.

The Cargo home directory located at:

  /Users/terauchishunsuke/.cargo

This can be modified with the CARGO_HOME environment variable.

The cargo, rustc, rustup and other commands will be added to
Cargo's bin directory, located at:

  /Users/terauchishunsuke/.cargo/bin

This path will then be added to your PATH environment variable by
modifying the profile files located at:

  /Users/terauchishunsuke/.profile
  /Users/terauchishunsuke/.zprofile
  /Users/terauchishunsuke/.bash_profile

You can uninstall at any time with rustup self uninstall and
these changes will be reverted.

Current installation options:


   default host triple: x86_64-apple-darwin
     default toolchain: stable (default)
               profile: default
  modify PATH variable: yes

1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
>

info: profile set to 'default'
info: default host triple is x86_64-apple-darwin
info: syncing channel updates for 'stable-x86_64-apple-darwin'
info: latest update on 2020-08-27, rust version 1.46.0 (04488afe3 2020-08-24)
info: downloading component 'cargo'
info: downloading component 'clippy'
info: downloading component 'rust-docs'
info: downloading component 'rust-std'
info: downloading component 'rustc'
 47.0 MiB /  47.0 MiB (100 %)  30.4 MiB/s in  1s ETA:  0s
info: downloading component 'rustfmt'
info: installing component 'cargo'
info: Defaulting to 500.0 MiB unpack ram
info: installing component 'clippy'
info: installing component 'rust-docs'
 12.6 MiB /  12.6 MiB (100 %)   4.2 MiB/s in  3s ETA:  0s
info: installing component 'rust-std'
 14.7 MiB /  14.7 MiB (100 %)   7.2 MiB/s in  2s ETA:  0s
info: installing component 'rustc'
 47.0 MiB /  47.0 MiB (100 %)   7.5 MiB/s in  6s ETA:  0s
info: installing component 'rustfmt'
info: default toolchain set to 'stable'

  stable installed - rustc 1.46.0 (04488afe3 2020-08-24)


Rust is installed now. Great!

To get started you need Cargo's bin directory ($HOME/.cargo/bin) in your PATH
environment variable. Next time you log in this will be done
automatically.

To configure your current shell run source $HOME/.cargo/env
```

正常にインストールできてそうなので指示通りにパスを通すようにzshrcに追記。

次にRocketを使いたいので、nightlyバージョンをインストールしてみる。
stableはリリース版、nightlyは開発版らしい。

```
>rustup default nightly
info: syncing channel updates for 'nightly-x86_64-apple-darwin'
info: latest update on 2020-09-12, rust version 1.48.0-nightly (99111606f 2020-09-11)
info: default toolchain set to 'nightly-x86_64-apple-darwin'

  nightly-x86_64-apple-darwin unchanged - (toolchain not installed)
```

ここでnightlyバージョンのtoolchainがないといわれる。

toolchainのリストを確認してみると

```
>rustup toolchain list
stable-x86_64-apple-darwing
```
確かにnightly版が入っていない。
githubに同様の[issue]["https://github.com/rust-lang/rust/issues/46391"]が上がっていたので試してみる。
1.exec `rustup self uninstall`

それでもうまくいかない...

色々探していると、ある[issue][https://github.com/rust-lang/rust/issues/55571]を見つける。

[ここ]["https://rust-lang.github.io/rustup-components-history/x86_64-apple-darwin.html"]でrustupのtoolchainを確認できるらしい。

どうやら今日(2020/09/12)のバージョンはmissingでLast availableが昨日(2020/09/11)になっている。

[公式ガイド]["https://doc.rust-lang.org/edition-guide/rust-2018/rustup-for-managing-rust-versions.html"]に従い昨日の日付のtoolchainをインストール。

```
>rustup toolchain install nightly-2020-09-11

info: syncing channel updates for 'nightly-2020-09-11-x86_64-apple-darwin'
info: latest update on 2020-09-11, rust version 1.48.0-nightly (a1947b3f9 2020-09-10)
info: downloading component 'cargo'
info: downloading component 'clippy'
info: downloading component 'rust-docs'
info: downloading component 'rust-std'
 19.3 MiB /  19.3 MiB (100 %)  12.3 MiB/s in  1s ETA:  0s
info: downloading component 'rustc'
 51.6 MiB /  51.6 MiB (100 %)  15.4 MiB/s in  3s ETA:  0s
info: downloading component 'rustfmt'
info: installing component 'cargo'
info: Defaulting to 500.0 MiB unpack ram
info: installing component 'clippy'
info: installing component 'rust-docs'
 13.0 MiB /  13.0 MiB (100 %)   4.0 MiB/s in  3s ETA:  0s
info: installing component 'rust-std'
 19.3 MiB /  19.3 MiB (100 %)   7.3 MiB/s in  2s ETA:  0s
info: installing component 'rustc'
 51.6 MiB /  51.6 MiB (100 %)   6.5 MiB/s in  7s ETA:  0s
info: installing component 'rustfmt'

  nightly-2020-09-11-x86_64-apple-darwin installed - rustc 1.48.0-nightly (a1947b3f9 2020-09-10)

info: checking for self-updates
```

`rustup default nightly`の代わりに以下のコマンドを実行し特定のバージョンを指定。

```
> rustup default nightly-2020-09-11-x86_64-apple-darwin

info: using existing install for 'nightly-2020-09-11-x86_64-apple-darwin'
info: default toolchain set to 'nightly-2020-09-11-x86_64-apple-darwin'

  nightly-2020-09-11-x86_64-apple-darwin unchanged - rustc 1.48.0-nightly (a1947b3f9 2020-09-10)

```

Rocketの[Quick Start]["https://rocket.rs/v0.4/guide/quickstart/"]を試してみる。

```
>git clone https://github.com/SergioBenitez/Rocket
>cd Rocket
>git checkout v0.4.5
>cd examples/hello_world
>cargo run
🚀 Rocket has launched from http://localhost:8000
```
うまくいった!!!!

どうやら、nightlyの最新版がunavalilabeだったのが原因ぽい?
