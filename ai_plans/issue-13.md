# Issue #13 — Remove extra data config flag

## Executive Summary

Since extra_data is now hardcoded to `"vibe builder 420 69"` (issue #11), the TOML config field
`extra_data` in `BaseConfig` is no longer needed. Remove it and its supporting deserializer.

## Files to Modify

- `crates/rbuilder/src/live_builder/base_config.rs`
  - Add `const EXTRA_DATA: &[u8] = b"vibe builder 420 69";`
  - Remove `Deserializer` from serde import (only used by `deserialize_extra_data`)
  - Remove `pub extra_data: Vec<u8>` field + `#[serde(...)]` attribute from `BaseConfig`
  - Change `extra_data: self.extra_data.clone()` → `extra_data: EXTRA_DATA.to_vec()`
  - Remove `extra_data: b"vibe builder 420 69".to_vec()` from `Default` impl
  - Remove `deserialize_extra_data` function entirely

## Implementation Tasks

- [x] Identify all extra_data references in base_config.rs
- [x] Write plan
- [x] Add EXTRA_DATA constant
- [x] Remove Deserializer import
- [x] Remove extra_data field from BaseConfig struct
- [x] Replace self.extra_data.clone() with EXTRA_DATA.to_vec()
- [x] Remove default value
- [x] Remove deserialize_extra_data function
- [x] cargo check
- [x] cargo clippy --workspace --features="" -- -D warnings
- [ ] cargo test --features="" -p rbuilder -- --test-threads=10

## Test Strategy

- Crate: `rbuilder`
- Command: `cargo test --features="" -p rbuilder -- --test-threads=10`
- No new tests needed — this is a field removal with no behavioral logic change.
