# LiteRT-LM-prebuilts

Mirrors Google's official [LiteRT-LM](https://github.com/google-ai-edge/LiteRT-LM)
C API prebuilt shared libraries as individual GitHub Releases assets, so
[`litertlm-sys`](https://github.com/kiamazi/LiteRT-LM-rs) can download just
the one file it needs for its target — not Google's single all-platforms
`.zip`.

## What's here

Each release tag (e.g. `v0.16.0`) matches a LiteRT-LM version. A GitHub
Actions workflow downloads Google's official release `.zip` for that
version — which bundles the shared libraries for every supported platform
together — extracts each one, and re-uploads them as separate release
assets:

| Asset                                | Platform      |
| ------------------------------------- | ------------- |
| `linux_x86_64_liblitert-lm.so`        | Linux x86_64  |
| `linux_arm64_liblitert-lm.so`         | Linux ARM64   |
| `macos_arm64_liblitert-lm.dylib`      | macOS ARM64   |
| `windows_x86_64_litert-lm.dll`        | Windows x86_64|
| `windows_x86_64_litert-lm.lib`        | Windows x86_64|
| `android_arm64_liblitert-lm.so`       | android ARM64 |
| `android_x86_64_liblitert-lm.so`      | android x86_64|


## Why this exists

Google publishes a single `.zip` per LiteRT-LM release containing the
shared libraries for *every* supported platform bundled together.
Downloading and unzipping that whole archive just to pull out the one
`.so`/`.dylib`/`.dll` a given build actually needs is wasteful — especially
inside a `build.rs` that might run on every fresh `cargo build`/CI job.
This repo does that unzip-and-split step once (via CI), so `litertlm-sys`'s
build script can fetch the bare library file for its own target directly
over a stable URL, with no zip handling and no unrelated platforms'
binaries downloaded on the consumer's end.

## Used by

[`litertlm-sys`](https://crates.io/crates/litertlm-sys) / [`litertlm-rs`](https://crates.io/crates/litertlm-rs) — Rust bindings for LiteRT-LM.

## Not affiliated with Google

This is an independent mirror, not an official Google project.
