# Issue #20 — New ordering strategy: numerical (tx hash)

## Executive Summary

Add a `TxHash` ordering strategy that sorts `SimulatedOrder`s by their identifier's
`fixed_bytes()` (B256) in ascending order. For mempool txs this is the transaction hash;
for bundles it's the UUID bytes padded to 32 bytes. This gives a deterministic,
hash-based numerical ordering.

## Files to Modify

### `crates/rbuilder/src/building/block_orders/order_priority.rs`
- Add `OrderTxHashCmp` struct implementing `eq`/`cmp` via `order.id().fixed_bytes()`
- Register `OrderTxHashPriority` with `create_order_priority!` macro
  (`simulation_too_low` reuses `simulation_too_low_profit` for degradation checks)

### `crates/rbuilder/src/building/mod.rs`
- Add `TxHash` variant to `Sorting` enum (with doc comment)
- Add `TX_HASH_NAME: &str = "tx_hash"` constant
- Add `TxHash` case in `FromStr` and `Display`

### `crates/rbuilder/src/live_builder/config.rs`
- Import `OrderTxHashPriority`
- Add `Sorting::TxHash` arm in both `match config.sorting` blocks
  (`backtest_simulate_block` and `create_ordering_builder`)

## Implementation Tasks

- [x] Write plan
- [x] Add OrderTxHashCmp + OrderTxHashPriority to order_priority.rs
- [x] Add TxHash variant + constant to mod.rs
- [x] Wire up in config.rs
- [x] cargo check
- [x] cargo clippy --workspace --features="" -- -D warnings
- [ ] cargo test --features="" -p rbuilder -- --test-threads=10 (build timeout in CI env)

## Test Strategy

- Crate: `rbuilder`
- Command: `cargo test --features="" -p rbuilder -- --test-threads=10`
- Add a unit test to `order_priority.rs` verifying lower hash < higher hash ordering.
