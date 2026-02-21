# Issue #21 — New ordering strategy: reverse numerical

## Executive Summary

Add a `ReverseTxHash` ordering strategy that is the mirror of `TxHash` (issue #20):
sorts `SimulatedOrder`s by their `id().fixed_bytes()` (B256) in **descending** order,
so higher hash bytes appear first.

## Files Modified

- `crates/rbuilder/src/building/block_orders/order_priority.rs`: `OrderReverseTxHashCmp` + `OrderReverseTxHashPriority` macro + unit test
- `crates/rbuilder/src/building/mod.rs`: `ReverseTxHash` variant + `REVERSE_TX_HASH_NAME = "reverse_tx_hash"`
- `crates/rbuilder/src/live_builder/config.rs`: import + both match arms wired up

## Implementation Tasks

- [x] Add OrderReverseTxHashCmp + OrderReverseTxHashPriority + unit test
- [x] Add ReverseTxHash variant + constant to mod.rs
- [x] Wire up in config.rs (backtest + live builder)
- [x] cargo check ✅
- [x] cargo clippy --workspace --features="" -- -D warnings ✅
