<div align="center">

# Molfi

### Private prediction markets on Stellar

**Trade real-world outcomes on a CLOB prediction market — with confidential, zero-knowledge positions.**

`molfi.fun` · Built on Stellar / Soroban · Live on Testnet

</div>

---

## What is Molfi?

Molfi is a Polymarket-style prediction market built natively on **Stellar/Soroban**, with one thing the incumbents don't have: **privacy**. You can prove you hold a valid, in-the-money position and withdraw your winnings **without revealing your identity or your full holdings** — using a Privacy-Pools design with a real on-chain **Groth16** zero-knowledge verifier.

- 🎯 **CLOB prediction market** — off-chain matching, on-chain settlement
- 💵 **Real money at stake** — bets escrow **mUSDC** on-chain; winners are paid pari-mutuel, a 2% fee funds an LP vault
- 🔮 **Trustless resolution** — markets settle on-chain from the **live Reflector** SEP-40 price oracle ("Will BTC be ≥ $100k?"), proven end-to-end ([tx](https://stellar.expert/explorer/testnet/tx/17748f6db4423e3d730c87b099ba362e9de3651d3987db3467691b184adfc497))
- 🔒 **ZK-verified bets** — *every* live bet submits a BLS12-381 **Groth16** proof that the on-chain `verifier` checks before the escrow accepts it, burning a nullifier (anti-replay). Proven live ([tx](https://stellar.expert/explorer/testnet/tx/82ec3633d883e7d5bb0735ecdb6c06a574dc2cf95d3c066e4f9509e575f632ce))
- 🤖 **Tradable by AI agents** — the `molfi-predict-sdk` + [`SKILL.md`](molfi-predict-sdk/SKILL.md) let any agent generate a wallet, faucet itself, and place real on-chain bets autonomously
- ✅ **Live on Stellar testnet** — contracts deployed, real on-chain proof verification + a full agent bet→resolve→redeem flow

> **New in this build:** an mUSDC faucet token, an LP vault, a `predict-escrow`
> contract (real-money pari-mutuel betting + on-chain ZK-gated bets), a SEP-40
> mock oracle, a MongoDB market engine serving live rolling markets, and a full
> SDK + agent skill. Jump to [🤖 Autonomous agents](#-autonomous-agents) and
> [💵 Real on-chain bets](#-real-on-chain-bets-musdc--zk--oracle).

---

## Live on Stellar Testnet

All five contracts are deployed and verified on Stellar testnet (network `Test SDF Network ; September 2015`).

| Contract | ID | Explorer |
|----------|----|----------|
| **verifier** (Groth16 / BLS12-381) | `CCTJUV6I5WVOLF7DQUZ2NX6ZJD2AWCGDBCEIN2SBFOWNLZP7MYV2ZXEC` | [view](https://stellar.expert/explorer/testnet/contract/CCTJUV6I5WVOLF7DQUZ2NX6ZJD2AWCGDBCEIN2SBFOWNLZP7MYV2ZXEC) |
| **privacy-pool** | `CAIMKBEKKLAT2CQ2T3QY6RQLKTHGLIHIUHYDDQ4NXULTZWEGZM2OMPNN` | [view](https://stellar.expert/explorer/testnet/contract/CAIMKBEKKLAT2CQ2T3QY6RQLKTHGLIHIUHYDDQ4NXULTZWEGZM2OMPNN) |
| **market** | `CDDX7ELEU2XBQWYYS72BFKZN5M642EBLEA6N2X22WZTHNGXPF7YPAXP3` | [view](https://stellar.expert/explorer/testnet/contract/CDDX7ELEU2XBQWYYS72BFKZN5M642EBLEA6N2X22WZTHNGXPF7YPAXP3) |
| **clob-settlement** | `CCW46B3JPRS3PRKQMQ5CT2XE623QCISXSOC2O7UVHKFQZW77KK7ZHZKQ` | [view](https://stellar.expert/explorer/testnet/contract/CCW46B3JPRS3PRKQMQ5CT2XE623QCISXSOC2O7UVHKFQZW77KK7ZHZKQ) |
| **policy** (ASP / compliance) | `CCIQSVQN3KONZZZEVXG6JMJM5IVZYWN54ZENVBQ6VHO3OO4Q7CTCIYOI` | [view](https://stellar.expert/explorer/testnet/contract/CCIQSVQN3KONZZZEVXG6JMJM5IVZYWN54ZENVBQ6VHO3OO4Q7CTCIYOI) |
| **mUSDC** (SEP-41 token + open faucet) | `CD4J6V73L5LBHDPCDITB2SMZQK5URUFBDED5IGTEU4G6XOUYXYUBJYST` | [view](https://stellar.expert/explorer/testnet/contract/CD4J6V73L5LBHDPCDITB2SMZQK5URUFBDED5IGTEU4G6XOUYXYUBJYST) |
| **predict-escrow** (real-money pari-mutuel + ZK bets) | `CCMR7AL3QT57B7KRZQ47AH34E4OQH42JXUK6BE7SQKRVOIJKAVILURL7` | [view](https://stellar.expert/explorer/testnet/contract/CCMR7AL3QT57B7KRZQ47AH34E4OQH42JXUK6BE7SQKRVOIJKAVILURL7) |
| **vault** (LP vault, earns trading fees) | `CBZSLDILDHVFVZ5E54Y4Z33H6AQANZYQLCB2MKOADD63BLP7VYA7VHDB` | [view](https://stellar.expert/explorer/testnet/contract/CBZSLDILDHVFVZ5E54Y4Z33H6AQANZYQLCB2MKOADD63BLP7VYA7VHDB) |
| **mock-oracle** (SEP-40, *deterministic-demo fallback only*) | `CCDFXRXQ3KDIH44QDLRCFE5IFGS3CRINOWR5LMB3GNYJQ3PPE3IRXMQG` | [view](https://stellar.expert/explorer/testnet/contract/CCDFXRXQ3KDIH44QDLRCFE5IFGS3CRINOWR5LMB3GNYJQ3PPE3IRXMQG) |

**Price oracle:** the live **Reflector** SEP-40 feed `CCYOZJCOPG34LLQQ7N24YXBM7LL62R7ONMZ3G6WZAAYPB5OYKOMJRN63` ([view](https://stellar.expert/explorer/testnet/contract/CCYOZJCOPG34LLQQ7N24YXBM7LL62R7ONMZ3G6WZAAYPB5OYKOMJRN63)) — the markets in the app + SDK settle from it on-chain. The `mock-oracle` is byte-compatible and used only to make scripted demos resolve deterministically.

Collateral assets: **mUSDC** (the prediction-market unit, with an open faucet) and native XLM via its SAC `CDLZFC3SYJYDZT7K67VZ75HPJVIEUVNIXF47ZG2FB2RMQQVU2HHGCYSC`.

### On-chain demo — real transactions

These are real testnet transactions produced by the contracts. Click through to the explorer.

**🔐 Zero-knowledge confidential withdrawal** (the headline)

| Step | What it proves | Transaction |
|------|----------------|-------------|
| `verifier.verify` | A real Groth16 proof verifies **on-chain** → `true` | [`ed9a59c3…`](https://stellar.expert/explorer/testnet/tx/ed9a59c3d855269c2736497b7e6f8a1bf612f137f64c979ab5c6488aabeec221) |
| `privacy_pool.deposit` | 10 XLM collateral escrowed into the pool | [`28625d34…`](https://stellar.expert/explorer/testnet/tx/28625d34973129ca6059f92d70156e3ae0319ede152c2910f81940886a1fc44b) |
| `privacy_pool.register_root` | Commitment Merkle root checkpointed | [`afcb8a5a…`](https://stellar.expert/explorer/testnet/tx/afcb8a5a02b3e994240074271dae404b848dbe158d1b6a7d7fad493174b27331) |
| `privacy_pool.withdraw` | **Withdrawal unlocked by a genuine ZK proof**; nullifier burned, paid out | [`8b192ff1…`](https://stellar.expert/explorer/testnet/tx/8b192ff100e37f08959155bcb4d4c4e20437a5d5e96280c8ca98f602f358f585) |

**📈 Prediction market lifecycle**

| Step | Transaction |
|------|-------------|
| `market.create` ("Will ETH be ≥ $5k at close?") | [`f43b9d8f…`](https://stellar.expert/explorer/testnet/tx/f43b9d8f872c62ffbd3a66ed80f8da132c4727dddab00f0135960676cf6fb7a0) |
| `market.begin_resolution` | [`5d6b33d6…`](https://stellar.expert/explorer/testnet/tx/5d6b33d6e5fdf493f5c90cf3220158a7e9ae89ead9f10ed11f3e92210d76675a) |
| `market.resolve` (→ YES) | [`98abdd79…`](https://stellar.expert/explorer/testnet/tx/98abdd79de13f337290f5d3d95ab0c2506e8c3f3d1c8ee69afbd37aeaed3965c) |

**💸 A real bet: Alice (YES) vs Bob (NO) on "Will BTC be ≥ $100k?"**

Two funded testnet accounts post opposite sides of a 100-share match. Orders are
**ed25519-signed off-chain** and submitted by the relayer; the market resolves YES,
so Alice wins the pot and Bob loses his stake.

| Step | Transaction |
|------|-------------|
| Alice funds 6 XLM (`deposit`) | [`0ee64798…`](https://stellar.expert/explorer/testnet/tx/0ee64798bc2fdaa2831dbf0a01ff93c7f83620053ee8081cc56305c84b371928) |
| Bob funds 4 XLM (`deposit`) | [`4582c362…`](https://stellar.expert/explorer/testnet/tx/4582c3628b3b5420a022be3f7cf4def99dabb397293d154b6e3d8a04ab95b440) |
| `clob.settle` — both signatures verified, positions opened, 10 XLM escrowed | [`f995b4d2…`](https://stellar.expert/explorer/testnet/tx/f995b4d257711c5ff9fca4386f68679c888c068931aff0b9a02c721a2cbb35e9) |
| `market.resolve` → **YES** | [`d1fb6d46…`](https://stellar.expert/explorer/testnet/tx/d1fb6d46cf4c8a221c2731342f5158fe9c614fef4a6a216d9b37155f922d17fa) |
| `alice.redeem` — **WINS the 10 XLM pot** | [`00592f6e…`](https://stellar.expert/explorer/testnet/tx/00592f6ebefc4dccc0d898d9e7228f21790b9f10fdc8fd63b5cee2669a45adb6) |
| `bob.redeem` | rejected — holds NO, not the winning YES |

**Result on-chain:** Alice `≈ 10003.9 XLM` (staked 6, won 10 → **+4 profit**); Bob `≈ 9995.99 XLM` (**−4, lost his stake**); escrow drained to `0`.

> The withdrawal at `8b192ff1…` is the privacy thesis in one transaction: the pool calls the on-chain verifier, the **real Groth16 proof passes**, the nullifier is burned, and collateral is released — without the proof revealing which deposit it came from.

---

## 💵 Real on-chain bets (mUSDC · ZK · oracle)

The `predict-escrow` contract makes a bet **real money on-chain**: it pulls `mUSDC`
into escrow, settles the outcome from the `market` contract's SEP-40 oracle, and
pays winners pari-mutuel (`stake × total_pool ÷ winning_pool`), routing a 2% fee
to the LP vault. One script runs the whole thing end-to-end on testnet —
generate a wallet, friendbot, faucet, bet, oracle-resolve, redeem:

```bash
cd molfi-contracts && bash scripts/agent_onchain_bet.sh
```

**Full agent bet → oracle settle → redeem** (real testnet tx; agent went `10000 → 10960` mUSDC):

| Step | Transaction |
|------|-------------|
| `musdc.faucet` (agent claims 10,000 mUSDC) | [`3d24c5a2…`](https://stellar.expert/explorer/testnet/tx/3d24c5a29a16fcd7e1d1b4162464e0869d9468ea2128ee9f6da679f984bab475) |
| `market.create_price_market` (oracle market) | [`1437b48a…`](https://stellar.expert/explorer/testnet/tx/1437b48adc0c7d41fb26f8a2af4fd309a21923dcc1d25e68938e782dc3c66cb1) |
| `escrow.bet` YES — 1000 mUSDC escrowed | [`c2574041…`](https://stellar.expert/explorer/testnet/tx/c2574041d44cbb01421221a3d592b1c6c1fa476ddf3c92f6f9a5f4dc95b3269e) |
| `escrow.bet` NO — counterparty 1000 mUSDC | [`16b11092…`](https://stellar.expert/explorer/testnet/tx/16b1109212a9194be428ebbecf8937e1ef9778408f379c514e9e4dd62439c224) |
| `market.resolve_from_oracle` → **YES** | [`4e72d91a…`](https://stellar.expert/explorer/testnet/tx/4e72d91a4fef918a41d9eaed92b0adec928deb071d403958911c9f0d96ed733c) |
| `escrow.redeem` — winner paid the pot, 2% → vault | [`45ba4457…`](https://stellar.expert/explorer/testnet/tx/45ba445757f220dc8e99a42045931ec2e4a231fdca4ac5e6bfc13d5964641001) |

**ZK-gated private bet** — the bet only escrows if a BLS12-381 Groth16 proof
verifies **on-chain** (and its nullifier is burned). `bash scripts/zk_onchain_bet.sh`:

| Step | Transaction |
|------|-------------|
| `escrow.bet_zk` — proof verified on-chain + nullifier burned + mUSDC escrowed | [`080e431e…`](https://stellar.expert/explorer/testnet/tx/080e431e58e6c496a26f51f867d1128a5080bbea6afa687af3f1286b409a644b) |
| `market.resolve_from_oracle` | [`c852d0b8…`](https://stellar.expert/explorer/testnet/tx/c852d0b84ab1f3e1edb76d1e3b878bec4a0d3d58f8ddeaeb2702cbac8a3181d9) |
| `escrow.redeem` | [`b612e116…`](https://stellar.expert/explorer/testnet/tx/b612e1163dc3c9bcf3601eccd83b11066ea377824b05a7f732d84381f9ab3db0) |

**Settled by the REAL Reflector oracle** — not a mock. `market.resolve_from_oracle`
reads the live [Reflector](https://reflector.network) SEP-40 feed
(`CCYOZJCO…`) on-chain and settles. `bash scripts/reflector_market.sh BTC`:

| Step | Transaction |
|------|-------------|
| `market.create_price_market` wired to Reflector ("Will BTC ≥ $55k?") | [`931b62cd…`](https://stellar.expert/explorer/testnet/tx/931b62cd8935f2bed221f543585906b349473f360c3761f7d3ca96ee874d1f91) |
| `market.resolve_from_oracle` → **YES** (Reflector reported BTC ≈ $59.7k, on-chain) | [`17748f6d…`](https://stellar.expert/explorer/testnet/tx/17748f6db4423e3d730c87b099ba362e9de3651d3987db3467691b184adfc497) |

> The web app's **⛓️ On-chain** markets and the SDK agent now settle from the
> live Reflector feed. The bundled `mock-oracle` is SEP-40-identical and used only
> for deterministic scripted demos — swapping real⇄mock is one constructor arg.

---

## 🤖 Autonomous agents

Molfi is built so an **AI agent trades through the same rails as a human**. The
[`molfi-predict-sdk`](molfi-predict-sdk/) exposes one `MolfiAgent` class, and
[`molfi-predict-sdk/SKILL.md`](molfi-predict-sdk/SKILL.md) is a drop-in agent skill
(also registered at `.claude/skills/molfi-trade/`).

```ts
import { MolfiAgent, OUTCOME_YES } from "molfi-predict-sdk";

const agent = MolfiAgent.create();              // fresh Stellar wallet
await agent.onboard();                           // friendbot XLM + mUSDC faucet
const [m] = await agent.onChainMarkets();        // a market it can bet on
await agent.bet(m.marketId, OUTCOME_YES, 100);   // escrow 100 mUSDC on YES — real tx
// after resolution: await agent.redeem(m.marketId)
```

```bash
cd molfi-predict-sdk && npm install && npm run build && node examples/agent-trade.mjs
```

An agent can **generate its own wallet, faucet itself, read live odds/order books,
place real on-chain bets, and redeem winnings** — no human in the loop. The market
engine bridges on-chain market ids at `GET /api/onchain/markets`.

---

## Architecture

```
                          molfi-predict-landing (Next.js)
                                       │  "Launch app"
                                       ▼
              molfi-predict-app (React + Vite + Stellar Wallets Kit)
                          │ signs orders / proofs via             │ Soroban RPC
                          ▼                                        ▼
                 @molfi/predict-sdk                         Stellar Testnet
        (canonical CLOB order signing, ed25519)                   │
                          │                                        ▼
                          │              ┌──────────── molfi-contracts (Soroban) ───────────┐
          off-chain CLOB / matching ───► │  market          lifecycle + oracle resolution   │
                                         │  clob-settlement  deposit → settle → redeem        │
                                         │  verifier         Groth16 / BLS12-381 (crypto only)│
                                         │  privacy-pool     commitments + nullifiers + ZK    │
                                         │  policy           ASP allow-list + limits          │
                                         └────────────────────────────────────────────────────┘
                                                       ▲
                              molfi-circuits (Circom + snarkjs, BLS12-381 Groth16)
                              withdraw · solvency · proofs verified on-chain
```

| Directory | What it is |
|-----------|-----------|
| [`molfi-contracts/`](molfi-contracts/) | Soroban (Rust) contracts — market, clob-settlement, verifier, privacy-pool, policy, **mUSDC**, **vault**, **predict-escrow**, **mock-oracle**. `soroban-sdk` v25. |
| [`molfi-circuits/`](molfi-circuits/) | Circom circuits + BLS12-381 Groth16 tooling (snarkjs → Soroban). |
| [`molfi-predict-sdk/`](molfi-predict-sdk/) | TypeScript SDK — `MolfiAgent` (wallet/faucet/on-chain bet) + CLOB order signing. Ships [`SKILL.md`](molfi-predict-sdk/SKILL.md) for AI agents. |
| [`molfi-app/`](molfi-app/) | The trading dApp (React + Vite + Tailwind + Stellar Wallets Kit) — markets, trade terminal, portfolio, vault, leaderboard. |
| [`molfi-backend/`](molfi-backend/) | MongoDB market engine — live spot feed, auto-rolling 15/30-min markets, settlement, leaderboard, vault stats, on-chain market bridge. |
| [`molfi-predict-landing/`](molfi-predict-landing/) | Marketing site (Next.js). |

Design follows Stellar's recommended **verifier / policy / application** split — cryptography, compliance, and state logic are separate, independently-auditable contracts.

---

## The privacy hook

```
deposit (public) ──► trade ──► market resolves ──► withdraw (private)
   commitment                    oracle / admin       Groth16 proof + nullifier
   into the tree                                       reveals only a nullifier
```

1. **Deposit** collateral and insert a commitment `C = H(secret, nullifier, amount)` into a Merkle tree.
2. **Trade / resolve** — markets settle from a SEP-40 oracle, a ZK-attested result, or admin.
3. **Withdraw** — submit a Groth16 proof of (a) membership in the tree and (b) an unspent nullifier. The `verifier` checks it on-chain; the `privacy-pool` burns the nullifier and pays out. The link between deposit and withdrawal is never revealed.

**ZK stack:** Circom circuits → snarkjs Groth16 over **BLS12-381** → on-chain verification via `soroban-sdk` BLS12-381 host functions. The `withdraw` circuit is a Poseidon Merkle-membership + nullifier proof; `solvency` proves "balance ≥ threshold" without revealing the balance.

---

## Tests

**20 automated tests pass** (`cargo test --workspace`), spanning unit and cross-contract integration. All five contracts build to WASM (`wasm32v1-none`).

```
$ cargo test --workspace
   verifier .............. 2 passed     (admin/auth, VK arity)
   privacy-pool .......... 1 passed
   market ................ 3 passed     (lifecycle, dup guard, resolved-gate)
   policy ................ 2 passed     (limits, Merkle membership)
   clob-settlement ....... 1 passed
   integration (e2e) ..... 7 passed
   real_proof ............ 4 passed
   ───────────────────────────────
   total ................ 20 passed; 0 failed
```

**Integration tests (`integration-tests/tests/e2e.rs`)** — full cross-contract flows with a SAC token, real ed25519 order signatures, and a SEP-40 mock oracle:

| Test | What it verifies |
|------|------------------|
| `bet_on_market_settle_and_win` | deposit → settle (ed25519 + nonce) → resolve → redeem; winner is paid the pot |
| `btc_market_resolves_via_oracle_and_pays_winner` | "BTC ≥ $100k" settles from the oracle; YES bettor paid |
| `oracle_resolves_no_when_below_threshold` | price below threshold → NO |
| `oracle_rejects_stale_price` | stale oracle price is rejected |
| `settle_rejects_replayed_nonce` | order-nonce replay protection |
| `privacy_pool_confidential_round_trip` | deposit → proof-gated withdraw → nullifier burned; double-spend rejected |
| `confidential_withdraw_with_real_proof` | the **real Groth16 proof** unlocks a withdrawal; wrong amount + replay rejected |

**Real-proof tests (`integration-tests/tests/real_proof.rs`)** — genuine Groth16 proofs (generated by `molfi-circuits/`) verified on the actual verifier contract:

| Test | What it verifies |
|------|------------------|
| `real_groth16_proof_verifies_on_chain` | a real proof verifies |
| `real_solvency_proof_verifies_on_chain` | "balance ≥ threshold" (balance hidden) |
| `real_membership_proof_verifies_on_chain` | Merkle membership + nullifier (the pool exit statement) |
| `tampered_public_input_is_rejected` | a tampered public input is rejected |

Run them:

```bash
cd molfi-contracts && cargo test --workspace      # 20 tests
cd molfi-circuits  && npm install && bash scripts/build.sh withdraw 14   # generate a real proof
```

---

## Reproduce the testnet deployment

```bash
# 1. Build contracts (soroban-sdk v25 → wasm32v1-none)
cd molfi-contracts && stellar contract build

# 2. Fund a testnet identity
stellar keys generate molfi --network testnet --fund

# 3. Generate the verifying key + proof from the circuit
cd ../molfi-circuits && npm install
bash scripts/build.sh withdraw 14
node scripts/emit_cli.mjs withdraw            # → build/withdraw/cli/{vk,proof,public}.json

# 4. Deploy + run the flows (contract IDs in molfi-contracts/deploy/testnet.env)
cd ../molfi-contracts
stellar contract deploy --wasm target/wasm32v1-none/release/molfi_verifier.wasm \
  --source molfi --network testnet -- --admin <ADDR> --vk "$(cat ../molfi-circuits/build/withdraw/cli/vk.json)"
bash scripts/zk_flow_testnet.sh        # ZK withdrawal flow
bash scripts/market_clean.sh           # market lifecycle
```

---

## Tech stack

- **Contracts:** Rust, `soroban-sdk` v25, BLS12-381 host functions
- **ZK:** Circom 2, snarkjs, Groth16 over BLS12-381, Poseidon
- **Oracle:** SEP-40 (Reflector-compatible) price feeds
- **SDK:** TypeScript, `@stellar/stellar-sdk`, ed25519 order signing
- **App:** React 18 + Vite + Tailwind, Stellar Wallets Kit (Freighter / xBull / Albedo / Lobstr / Hana)

---

## Try the app

```bash
cd molfi-predict-app && npm install && npm run dev   # http://localhost:8080/demo
```

- **`/markets`** lists **real markets read live from the `market` contract** (via
  on-chain `markets()` enumeration + `get_market`) — currently two open for
  trading (BTC, ETH) and two resolved (RAIN → YES, ELEC → NO). No mock data.
- **`/demo`** connects a Stellar wallet (Stellar Wallets Kit — Freighter / xBull /
  Albedo / Lobstr / Hana), reads a resolved market's outcome from the live
  `market` contract, and **submits a real deposit transaction to the
  `privacy-pool` contract** signed by your wallet — the full read + write path.

## Status

**Working & on-chain:**
- **9 contracts** deployed to testnet; all build to WASM; contract tests green (incl. the new `predict-escrow` pari-mutuel + ZK-nullifier tests).
- **Real-money bet, end-to-end:** an agent generates a wallet, friendbots itself, claims mUSDC, bets (real escrow), the market resolves from its SEP-40 oracle, and the winner redeems the pot (2% fee → LP vault) — `scripts/agent_onchain_bet.sh` (agent `10000 → 10960` mUSDC on-chain).
- **ZK-gated private bet:** `escrow.bet_zk` verifies a BLS12-381 Groth16 proof **on-chain** before escrowing and burns the nullifier (replay rejected) — `scripts/zk_onchain_bet.sh`.
- **Confidential withdrawal** unlocked by a real Groth16 proof verified on-chain; nullifier burned.
- **AI-agent native:** `molfi-predict-sdk` (`MolfiAgent`) + `SKILL.md` let an agent trade autonomously; verified live (`node examples/agent-trade.mjs` placed a real on-chain bet).
- **Market engine + app:** MongoDB backend serves live auto-rolling 15/30-min markets across 7 tokens; the app surfaces markets, a trade terminal, portfolio with indicative P&L, vault and leaderboard.
- **Real on-chain leaderboard + vault (no seed data):** the backend indexes `predict-escrow` `bet`/`redeem` events from Soroban RPC, so the **leaderboard is genuine on-chain trader PnL**; the **vault dashboard reads the live vault contract** (TVL / NAV / fees), and vault deposits are wallet-signed on-chain. The only intentionally-synthetic surface is the order book ("indicative" depth — a privacy market has no public resting orders).
- **Self-sustaining on-chain venue:** a backend **keeper** signs as the market admin to keep a fresh, **Reflector-settled** on-chain market open per token (BTC/ETH/SOL/XLM/LINK/AVAX) and `resolve_from_oracle`s them at close — so the markets, leaderboard, and vault stay live with zero manual upkeep.
- **Crypto = the real on-chain markets (unified):** the app's **Crypto** tab *is* these keeper markets, shown in the premium grid; the detail page is the full trade terminal (live chart, real on-chain escrow-pool book, bet ticket) wired to `predict-escrow` + Reflector. Betting escrows real mUSDC. (Pyth has no Stellar/Soroban deployment — Reflector is the Stellar-native SEP-40 oracle, so it's the real, no-mock choice.)
- Oracle (SEP-40) + ZK + admin market-resolution paths.

**Roadmap (documented, not blocking the demo):** on-chain Poseidon tree once `CAP-0075` ships (retires the off-chain root checkpoint) · client-side proof generation in the SDK (`generateExitProof`) · binding the withdrawal recipient in-circuit. See [`ROADMAP.md`](ROADMAP.md) and [`TODO.md`](TODO.md).

---

## License

MIT
