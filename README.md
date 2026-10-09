# herat-contract

A workspace of [Soroban](https://soroban.stellar.org/) smart contracts for
on-chain identity bonds, delegated attestations, dispute resolution, treasury
custody, and governance. Every contract is written in Rust and compiles to a
`wasm32-unknown-unknown` target for deployment on Stellar.

The repo previously shipped as `Credence-Contracts`; it is now `herat-contract`.
Crate/package names (for example `credence_bond`) are intentionally left
unchanged to preserve on-chain and wire compatibility.

## Workspace layout

| Path | Package | Purpose |
|---|---|---|
| `contracts/credence_bond` | `credence_bond` | Core identity bond: attestations, slashing, tiers, rolling bonds, fees |
| `contracts/credence_delegation` | `credence_delegation` | Delegated attestation and management rights |
| `contracts/credence_registry` | `credence_registry` | Identity ↔ bond-contract address mapping |
| `contracts/credence_treasury` | `credence_treasury` | Fee accounting and multi-sig withdrawal |
| `contracts/dispute_resolution` | `dispute_resolution` | Stake-backed slash disputes with arbitrator voting |
| `contracts/arbitration` | `arbitration` | Weighted-vote dispute resolution |
| `contracts/admin` | `admin` | Hierarchical role management (SuperAdmin / Admin / Operator) |
| `contracts/credence_multisig` | `credence_multisig` | Generic M-of-N multi-signature proposals |
| `contracts/timelock` | `timelock` | Time-delayed operation execution |
| `contracts/fixed_duration_bond` | `fixed_duration_bond` | Fixed-term bond with optional early-exit penalty |
| `contracts/bounty-escrow` | `bounty-escrow` | Escrowed bounty payouts |
| `contracts/credence_errors` | `credence_errors` | Shared, wire-stable `ContractError` enum |
| `contracts/credence_math` | `credence_math` | Overflow-safe arithmetic and time helpers |
| `crates/testutils` | `testutils` | Shared test harness (mock auth, helpers) |
| `crates/interfaces` | `interfaces` | Common trait/interface definitions |
| `crates/credence_admin_cli` | `credence_admin_cli` | Off-chain CLI for admin operations |

See [`docs/crates.md`](docs/crates.md) for the dependency graph and
[`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) for per-crate internals.

## Requirements

- Rust **1.89.0** (see `rust-toolchain.toml`; installed automatically by `rustup`)
- Components: `rustfmt`, `clippy`, `llvm-tools-preview`
- Target: `wasm32-unknown-unknown`

```bash
rustup target add wasm32-unknown-unknown
```

## Build, test, lint

```bash
# Build the whole workspace
cargo build --workspace

# Release build (optimized, wasm-friendly profile)
cargo build --workspace --release

# Run all tests
cargo test --workspace

# Formatting check
cargo fmt --all -- --check

# Lints
cargo clippy --workspace --all-targets --all-features -- -D warnings
```

Per-crate coverage is enforced at **95%** lines with `cargo-llvm-cov`:

```bash
cargo llvm-cov --package credence_bond --fail-under-lines 95
cargo llvm-cov --package credence_delegation --fail-under-lines 95
cargo llvm-cov --package timelock --fail-under-lines 95
```

Full contributor expectations live in [`docs/CI.md`](docs/CI.md) and
[`docs/testing.md`](docs/testing.md).

## Documentation

- [`docs/README.md`](docs/README.md) — complete documentation index
- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — components, storage, events
- [`docs/crates.md`](docs/crates.md) — crate dependency graph
- [`docs/DEPLOYMENT.md`](docs/DEPLOYMENT.md) — deployment runbook
- [`docs/THREAT_MODEL.md`](docs/THREAT_MODEL.md) — STRIDE threat model
- [`CHANGELOG.md`](CHANGELOG.md) — release history

## License

No license file is currently checked in. Contact the maintainers before
redistributing or reusing this code.
