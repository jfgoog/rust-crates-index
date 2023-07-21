# crates-index

[![crates-index on Crates.io](https://img.shields.io/crates/v/crates-index.svg)](https://lib.rs/crates/crates-index)

Library for retrieving and interacting with the [crates.io registry git-based index](https://github.com/rust-lang/crates.io-index).

The index contains metadata for all Rust libraries and programs published on crates.io: their versions, dependencies, and feature flags.

[Documentation](https://docs.rs/crates-index/)

## Example

```rust
let index = crates_index::Index::new_cargo_default()?;

for crate_releases in index.crates() {
    let _ = crate_releases.most_recent_version(); // newest version
    let crate_version = crate_releases.highest_version(); // max version by semver
    println!("crate name: {}", crate_version.name());
    println!("crate version: {}", crate_version.version());
}
```

## Migration from 0.19 to 1.0.0

* `git2` is now optional if you use `default-features = false`. If `git-index` feature is enabled, `git2` v0.17 is required. You'll want to enable `https` feature too.
* `SparseIndex.make_cache_request` returns `request::Builder` instead of `Request`. Call `.body(())` on it.

## Migration from 0.18

It should work without any code changes. Only the `git2` and `toml` dependencies were updated.

## Migration from 0.16 and 0.17

* `BareIndex` and `BareIndexRepo` have become the `Index`.
* `Index::new_cargo_default()?` is the preferred way of accessing the index. Use `with_path()` to clone to a different directory.
* There's no need to call `retrieve()` or `exists()`. It's always retrieved and always exists.
* `retrieve_or_update()` is just `update()`.
* `highest_version()` returns crate metadata rather than just the version number. Call `highest_version().version().parse()` to get `semver::Version`.
* There's no `crate_index_paths()`, because there are no files any more. Use `crate_` to get individual crates.

## Similar crates

- [`crates_io_api`](https://github.com/theduke/crates_io_api)

## License

Licensed under version 2 of the Apache License
