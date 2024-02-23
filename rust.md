# Rust

Installation of `rust`

```bash
λ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
info: downloading installer

Welcome to Rust!

This will download and install the official compiler for the Rust
programming language, and its package manager, Cargo.

Rustup metadata and toolchains will be installed into the Rustup
home directory, located at:

  ~/.rustup

This can be modified with the RUSTUP_HOME environment variable.

The Cargo home directory is located at:

  ~/.cargo

This can be modified with the CARGO_HOME environment variable.

The commands cargo, rustc, rustup and other commands will be added to
Cargo's bin directory, located at:

  ~/.cargo/bin

This path will then be added to your PATH environment variable by
modifying the profile files located at:

  ~/.profile
  ~/.bash_profile
  ~/.zshenv

You can uninstall at any time with rustup self uninstall and
these changes will be reverted.

Current installation options:


   default host triple: aarch64-apple-darwin
     default toolchain: stable (default)
               profile: default
  modify PATH variable: yes

1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
>

info: profile set to 'default'
info: default host triple is aarch64-apple-darwin
info: syncing channel updates for 'stable-aarch64-apple-darwin'
info: latest update on 2022-11-03, rust version 1.65.0 (897e37553 2022-11-02)
info: downloading component 'cargo'
info: downloading component 'clippy'
info: downloading component 'rust-docs'
 18.9 MiB /  18.9 MiB (100 %)   8.8 MiB/s in  2s ETA:  0s
info: downloading component 'rust-std'
 27.6 MiB /  27.6 MiB (100 %)   8.8 MiB/s in  3s ETA:  0s
info: downloading component 'rustc'
 52.2 MiB /  52.2 MiB (100 %)   8.5 MiB/s in  6s ETA:  0s
info: downloading component 'rustfmt'
info: installing component 'cargo'
info: installing component 'clippy'
info: installing component 'rust-docs'
 18.9 MiB /  18.9 MiB (100 %)   8.4 MiB/s in  2s ETA:  0s
info: installing component 'rust-std'
 27.6 MiB /  27.6 MiB (100 %)  15.4 MiB/s in  1s ETA:  0s
info: installing component 'rustc'
 52.2 MiB /  52.2 MiB (100 %)  17.9 MiB/s in  2s ETA:  0s
info: installing component 'rustfmt'
info: default toolchain set to 'stable-aarch64-apple-darwin'

  stable-aarch64-apple-darwin installed - rustc 1.65.0 (897e37553 2022-11-02)


Rust is installed now. Great!

To get started you may need to restart your current shell.
This would reload your PATH environment variable to include
Cargo's bin directory ($HOME/.cargo/bin).

To configure your current shell, run:
source "$HOME/.cargo/env"
```

## Verification
Verify the installation setup:
```sh
$ rustup -V
rustup 1.26.0 (5af9b9484 2023-04-05)
info: This is the version for the rustup toolchain manager, not the rustc compiler.
info: The currently active `rustc` version is `rustc 1.73.0 (cc66ad468 2023-10-03)`
```

Check for new versions:
```sh
$ rustup check
stable-aarch64-apple-darwin - Update available : 1.73.0 (cc66ad468 2023-10-03) -> 1.74.0 (79e9716c9 2023-11-13)
rustup - Up to date : 1.26.0
```

## Installing rust source

```bash
rustup component add rust-src
```

## Rust analyser

```bash
rustup component add rust-analyzer

rustup which --toolchain stable rust-analyzer
~/.rustup/toolchains/stable-aarch64-apple-darwin/bin/rust-analyzer

ln -s ~/.rustup/toolchains/stable-aarch64-apple-darwin/bin/rust-analyzer rust-analyzer
```
## List installed components

```bash
❯ rustup component list --installed
cargo-aarch64-apple-darwin
clippy-aarch64-apple-darwin
rust-analyzer-aarch64-apple-darwin
rust-docs-aarch64-apple-darwin
rust-src
rust-std-aarch64-apple-darwin
rustc-aarch64-apple-darwin
rustfmt-aarch64-apple-darwin
```

## Testing

The `#[cfg(test)]` annotation on the tests module tells Rust to compile and run the test code only when you run cargo test, not when you run cargo build. This saves compile time when you only want to build the library and saves space in the resulting compiled artifact because the tests are not included. You’ll see that because integration tests go in a different directory, they don’t need the `#[cfg(test)]` annotation. However, because unit tests go in the same files as the code, you’ll use `#[cfg(test)]` to specify that they shouldn’t be included in the compiled result.

# Tutorials & References
The following are very good resources to start and follow:

- [This][comprehensive-rust] is the Rust course used by the Android team at Google. It provides you the material to quickly teach Rust.

- [Rust Book][rust-book] provides a very good detailed tutorials and information

- [Rust By Example][rust-byexample] provides a collection of runnable examples that illustrate various Rust concepts and standard libraries.

[comprehensive-rust]: https://github.com/google/comprehensive-rust
[rust-book]: https://doc.rust-lang.org/book/
[rust-byexample]: https://doc.rust-lang.org/stable/rust-by-example/
