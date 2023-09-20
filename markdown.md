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
  <li class="fragment">Adding the migration to Asset Hub (~10 min)</li>
  <li class="fragment">Dry-run upgrade with try-runtime-cli (~15 min)</li>
  <li class="fragment">What's next for migration tooling</li>
</ul>

<p class="fragment">Please ask questions!</p>

---

### New FRAME example pallet

<small>(~15 minutes)</small>

```bash
cargo doc \
    --package pallet-example-storage-migrations \
    --features try-runtime \
    --open
```

---

### Add it to Asset Hub

<small>(~10 minutes)</small>

<ol class="fragment">
  <li>Add `pallet-example-storage-migrations` to the Cargo.toml</li>
  <li>Add the example pallet to the runtime</li>
  <li>Implement the example pallet config for our runtime</li>
  <li>Add the versioned migration to the migrations tuple</li>
</ol>

---

### Test it with try-runtime-cli

<small>(~15 minutes)</small>

<ul>
  <li class="fragment">Why we need further testing</li>
  <ul>
    <li class="fragment">Dry-run our runtime upgrade using real storage</li>
    <ul>
      <li class="fragment">PoV size</li>
      <li class="fragment">Execution time</li>
      <li class="fragment">Reality stings</li>
    </ul>
  </ul>
  <li class="fragment">Get Started: <a href="https://github.com/paritytech/try-runtime-cli">https://github.com/paritytech/try-runtime-cli</a></li>
  <li class="fragment">Example of overweight migration</li>
  <li class="fragment">Safety tips</li>
</ul>

---

### Things to be aware of

<ul>
  <li class="fragment">Multi-block migrations: <a href="https://github.com/paritytech/polkadot-sdk/issues/198">https://github.com/paritytech/polkadot-sdk/issues/198</li>
  <li class="fragment">Chopsticks: <a href="https://github.com/AcalaNetwork/chopsticks">https://github.com/AcalaNetwork/chopsticks</li>
  <li class="fragment">Parachain recovery discussion: <a href="https://forum.polkadot.network/t/how-to-recover-a-parachain/673/25">https://forum.polkadot.network/t/how-to-recover-a-parachain/673/25</li>
</ul>

---

### Thank you!

Question time
