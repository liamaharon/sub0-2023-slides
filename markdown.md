## Best Practices for Safe Runtime Upgrades

Liam Aharon

Rust Developer - FRAME - Parity Technologies

<br />

[sub0-2023-liam-slides.netlify.app](https://sub0-2023-liam-slides.netlify.app/)

---

## Compiling can take a while

### Let's do it in advance

<div class="fragment">
Asset Hub Runtime

```bash
git clone git@github.com:paritytech/polkadot-sdk.git
cd polkadot-sdk
# contains example migration we will add to the asset hub runtime
git checkout liam-migrations-reference-docs
cargo b -r --package asset-hub-polkadot-runtime --features try-runtime
```

Tool for dry-running runtime upgrades

```bash
cargo install \
    --git https://github.com/paritytech/try-runtime-cli \
    --locked
```

</div>

---

## Game: Who here has 🙋

<ul>
  <li class="fragment">Who here has written a "hello world" program?</li>
  <li class="fragment">Who here has written a FRAME pallet?</li>
  <li class="fragment">Who here has configured a FRAME runtime?</li>
  <li class="fragment">Who here has been involved in writing runtime upgrade code?</li>
  <li class="fragment">Who here has been involved in writing runtime upgrade code that didn't quite execute the way you thought it would, leading to you having to have an awkward conversation with your CEO about how it kind of messed up your chain?</li>
  <ul>
    <li class="fragment">You're not alone</li>
  </ul>
</ul>

---

## The intended outcome of this workshop

<p class="fragment">Reduce the amount of awkward conversations and bricked chains</p>

---

## Agenda

<ul>
  <li class="fragment">Reading a new FRAME example pallet (~10 min)</li>
  <li class="fragment">Reading a migration module for the pallet (~10 min)</li>
  <li class="fragment">Adding the migration to Asset Hub (~5 min)</li>
  <li class="fragment">Dry-run upgrade with try-runtime-cli (~10 min)</li>
  <li class="fragment">Final thoughts</li>
</ul>

<p class="fragment">Please ask questions!</p>

---

### FRAME example pallet and migration

```bash
cargo doc \
    --package pallet-example-storage-migrations \
    --features try-runtime \
    --open
```

---

### Add example pallet and migration to Asset Hub

```toml
# 1. Add dependency and update feature propagation
pallet-example-storage-migrations = { path = "../../../../../substrate/frame/examples/storage-migrations", default-features = false }
```

```rust
// 2. Add pallet to runtime and implement config
construct_runtime!(
  pub enum Runtime
  {
    // other pallets...
    ExamplePallet: pallet_example_storage_migrations::{Pallet, Call, Storage} = 123,
  }
)
// 3. Implement config
impl pallet_example_storage_migrations::Config for Runtime {}
// 4. Add migrations to Migraitons tuple
pub type Migrations =
  (
    // other migrations...
    pallet_example_storage_migrations::migrations::v1::versioned::MigrateV0ToV1<Runtime>
  );
```

---

### Why can't we just deploy now?

<ul>
  <li class="fragment">Dry-run our runtime upgrade using real storage</li>
  <ul class="fragment">
    <li>Estimate PoV size</li>
    <li>Estimate Execution Time</li>
    <li>Sometimes, Reality Bites</li>
  </ul>
</ul>

---

### Introducing try-runtime-cli

<small>
Project URL: <a href="https://github.com/paritytech/try-runtime-cli">https://github.com/paritytech/try-runtime-cli</a>
</small>

---

### Dry-running with try-runtime-cli

Build Asset Hub Runtime

```bash
cargo b -r -p asset-hub-polkadot-runtime --features try-runtime
```

Dry-run a Runtime Upgrade

```bash
try-runtime \
    --runtime ./target/release/wbuild/asset-hub-polkadot-runtime/asset_hub_polkadot_runtime.wasm \
    on-runtime-upgrade \
    live --uri wss://polkadot-asset-hub-rpc.polkadot.io:443
```

<p class="fragment">Finish with examples of overweight and failed migrations</p>

---

### Final thoughts

<ul>
  <li class="fragment">Safety tips</li>
  <li class="fragment">Multi-block migrations: <a href="https://github.com/paritytech/polkadot-sdk/issues/198">https://github.com/paritytech/polkadot-sdk/issues/198</a></li>
  <li class="fragment">Chopsticks: <a href="https://github.com/AcalaNetwork/chopsticks">https://github.com/AcalaNetwork/chopsticks</a></li>
  <li class="fragment">Parachain recovery discussion: <a href="https://forum.polkadot.network/t/how-to-recover-a-parachain/673/25">https://forum.polkadot.network/t/how-to-recover-a-parachain/673/25</a></li>
</ul>

---

### That's all folks

Questions?
