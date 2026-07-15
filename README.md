> [!NOTE]
> [`indexable-inc/build-version`](https://github.com/indexable-inc/build-version) is a read-only mirror, generated from [`packages/build-version`](https://github.com/indexable-inc/index/tree/c10de686bbc054cb4a1895e635813494a49ca52b/packages/build-version) in [`indexable-inc/index`](https://github.com/indexable-inc/index) at commit `c10de686bbc0`. The monorepo is the source of truth: please open issues and pull requests [there](https://github.com/indexable-inc/index). This mirror is regenerated automatically; anything pushed directly here will be overwritten.

# build-version

A tiny Rust library that formats a binary's `--version` line from build
metadata a Nix wrapper stamps into the environment, so every tool reports its
revision, commit date, and how long ago it was built in one consistent shape:

```console
$ nwm --version
nix-web-monitor 0.1.0 (7e42ccdb1882, 2026-06-07, 2 days ago)
```

A reproducible build has no wall-clock compile time, so the "when" is the
flake's commit time (`IX_BUILD_EPOCH`, from `self.lastModified`), and "how
long ago" is computed at run time against the current clock — which is why the
metadata rides the wrapper environment (`IX_BUILD_REV`, `IX_BUILD_EPOCH`)
instead of being baked into the binary: a new commit re-stamps a tiny wrapper
without rebuilding the Rust unit.

## Quickstart

Hand the interned `&'static str` straight to clap:

```rust
let cmd = clap::Command::new("mytool")
    .version(build_version::version_static(env!("CARGO_PKG_VERSION")));
```

With the two env vars set, `--version` prints
`0.1.0 (7e42ccdb1882, 2026-06-07, 2 days ago)`; without them (a dev
`cargo run`), it degrades to the bare crate version. In the monorepo the
wrapper sets them from `ix.rev` / `ix.revEpoch` (see
`packages/nix/nix-web-monitor/server/default.nix` for a real example).

## Pointers

- [doc/build-version/overview.md](https://github.com/indexable-inc/index/blob/main/doc/build-version/overview.md)
  — from-source documentation.

## Install

`build-version` is not on crates.io; add it as a git dependency:

```toml
[dependencies]
build-version = { git = "https://github.com/indexable-inc/build-version" }
```

Changes: [CHANGELOG.md](CHANGELOG.md), derived from the [monorepo history](https://github.com/indexable-inc/index/commits/main/packages/build-version) of the package.
