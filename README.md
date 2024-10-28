# `git-lfs-agent-scp`

A custom transfer agent for [`git-lfs`](https://git-lfs.github.com/) that uses [`scp`](https://www.openssh.com/) to transfer files.
This transfer agent makes it possible to use `git-lfs` in situations where the remote only speaks `ssh`.
This is useful if you do not want to install a `git-lfs` server.

## Usage

Configure your local git repository as follows

```sh
$ git config lfs.standalonetransferagent scp # <1>
$ git config lfs.customtransfer.scp.path git-lfs-agent-scp # <2>
$ git config lfs.customtransfer.scp.args $DESTINATION # <3>
```
1. tell `git-lfs` to use the transfer agent named "scp"
2. tell `git-lfs` what the name of the program is of the transfer agent
3. `$DESTINATION` is the destination to which `scp` will copy files tracked by `git-lfs` when running `$ git pull` and the place it will store files when running `$ git push`

- **NOTE**: `$DESTINATION` can be set to anything `scp` understands.
      As an example, `server.example.com:/home/git/my-lfs-files` ships files to a remote server over `ssh`.

## Install

Download pre-built executable binary from [Releases page](https://github.com/funatsufumiya/git-lfs-agent-scp/releases) (unzip it and move to `/usr/local/bin` or `C:¥Windows¥System32`).

## Build

Clone the source and run:

```bash
$ cargo build --release # just build
$ cargo install --path . # build and install
```

## Cross build

- from mac (arm64), build mac x64

```bash
$ rustup target add x86_64-apple-darwin
$ cargo build --release --target=x86_64-apple-darwin

# aarch64
# rustup target add aarch64-apple-darwin
# cargo build --release --target=aarch64-apple-darwin
```

- from mac, build linux

see https://github.com/rust-lang/rust/issues/34282#issuecomment-796182029

```bash
$ brew tap SergioBenitez/osxct
$ brew install x86_64-unknown-linux-gnu
$ brew tap messense/macos-cross-toolchains
$ brew install aarch64-unknown-linux-gnu
$ rustup target add x86_64-unknown-linux-gnu
$ rustup target add aarch64-unknown-linux-gnu
$ CARGO_TARGET_X86_64_UNKNOWN_LINUX_GNU_LINKER=x86_64-unknown-linux-gnu-gcc cargo build --release --target=x86_64-unknown-linux-gnu

$ CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_LINKER=aarch64-unknown-linux-gnu-gcc cargo build --release --target=aarch64-unknown-linux-gnu
```

- from mac, build windows

```bash
$ brew install mingw-w64
$ rustup target add x86_64-pc-windows-gnu
$ CARGO_TARGET_X86_64_UNKNOWN_LINUX_GNU_LINKER=x86_64-w64-mingw32-gcc cargo build --release --target=x86_64-pc-windows-gnu
```

## Acknowledgements

This project was created with fork from https://github.com/tdons/git-lfs-agent-scp
