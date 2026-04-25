# SeasonWars Dev Kit — Example #1: Burn Tracker

> xZod is not just a protocol. It's a real-time decision
> engine you can build on.

Build a live strategy app on top of a DeFi protocol —
in under 30 minutes, using only on-chain data.

Example: burn the wrong token → lose ~20% efficiency instantly
(no way to recover once executed)

→ [Live demo](https://aiorosxz.github.io/seasonwars-burn-tracker-example)
→ Watch live burns update every few seconds using on-chain events
→ Simulate your first strategy

This repo is part of the **SeasonWars Dev Kit**.
SeasonWars is evolving into a developer platform.
This tracker is the first building block.

---

## ⚡ What you can build

* 🤖 **Strategy bots** — optimize burn decisions using
  `getCurrentSigns()` + `quoteBurn()`
* 📊 **Analytics dashboards** — track clans, wallets,
  and performance in real-time
* 🧠 **Decision engines** — simulate outcomes before
  executing transactions
* 🎮 **Alternative frontends** — build your own UI
  for SeasonWars

---

## 🎯 What makes SeasonWars interesting

This is not a simple yield optimization problem.

* Burn returns are partial (~65%) and delayed
* NFTs can outperform short-term losses
* Staking is locked (45 days) — timing matters
* Yield rotates every cycle (HOT → COLD)

There is no obvious optimal strategy.

→ This is why bots, dashboards and simulators matter.

---

## 🧠 Why developers build on SeasonWars

Most DeFi protocols expose data.
SeasonWars creates decision complexity.

→ Not just "what is the yield?"
→ But "what is the best move right now?"

* Yields rotate over time (not fixed)
* Capital is locked (45 days)
* Returns are partial and delayed (~65%)
* Rewards depend on timing, token choice and player behavior

There is no dominant strategy.

→ This creates a new type of opportunity:

* Bots can outperform humans
* Interfaces can influence decisions
* Simulations can create real edge

SeasonWars is not just financial infrastructure.
It's a competitive decision layer.

---

## 🔑 Why `quoteBurn()` matters

Most DeFi protocols tell you what happened.

SeasonWars lets you simulate what will happen
before you act.

→ `quoteBurn()` turns DeFi into a decision engine.
This is the core primitive that everything builds on.

```js
const points = await contract.quoteBurn(
  cycleId,
  userAddress,
  zodSign,  // 0 = Aries, 7 = Scorpio...
  amount
);
// → returns expected burn points before you commit
```

Use this to compare strategies, build bots, or guide
user decisions.

---

## 🔄 What the protocol exposes

**Events (real-time)**

* `ZODBurned(cycleId, player, zodSign, amount, burnPoints)`
* `CycleCreated(cycleId, startTime, endTime, deadline)`
* `CycleFinalized(cycleId, winnerClan, totalBurned, rewardToken)`

**Views (state)**

* `getCurrentSigns()` → hot, cold, opposite
* `getLeaderboard(cycleId)` → burn points per clan
* `getPlayerInfo(cycleId, player)` → full wallet position
* `quoteBurn(cycleId, player, zodSign, amount)` → simulate

---

## 🧩 How this tracker works

| Feature          | Source              |
| ---------------- | ------------------- |
| Live burn feed   | `ZODBurned` event   |
| Clan leaderboard | `getLeaderboard()`  |
| HOT / COLD / OPP | `getCurrentSigns()` |
| Burn simulator   | `quoteBurn()`       |

Everything you see can be reused in your own app.

---

## 🔗 Contracts (Sepolia Testnet)

| Contract        | Address                                                                                            |
| --------------- | -------------------------------------------------------------------------------------------------- |
| SeasonWars V2.3 | [`0x5598...2fc8`](https://sepolia.etherscan.io/address/0x5598778158a66d376bd96243FC6bc27316fD2fc8) |
| xZOD            | [`0x017f...9D2f`](https://sepolia.etherscan.io/address/0x017f4333Aa7e83fA42d119d5489c41e3648c9D2f) |

---

## 🛠 Build your own

Start by modifying this tracker:

* Add wallet connection (wagmi, ethers.js)
* Build a strategy bot
* Create burn alerts
* Add multi-wallet tracking

Or build something entirely new.

→ Read [`INTEGRATION.md`](./INTEGRATION.md) for a
step-by-step guide. Build time: ~30 minutes.

---

## 🧠 Mental model

You are not optimizing yield.
You are competing under constraints.

* short-term losses (burn)
* long-term advantages (NFTs, APY boosts)
* timing constraints (45d staking lock)

SeasonWars is where those decisions play out.

---

Part of the **xZod Network** ecosystem.

SeasonWars is the first step toward a new class of
on-chain strategy applications.

[xzod.io](https://xzod.io) ·
[Whitepaper](https://xzod.io/whitepaper) ·
[github.com/AiorosxZ/xzod-contracts](https://github.com/AiorosxZ/xzod-contracts)
[Live demo](https://aiorosxz.github.io/seasonwars-burn-tracker-example)

*"In Zod we trust."* 🔥
