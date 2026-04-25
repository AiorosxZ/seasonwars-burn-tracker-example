# INTEGRATION.md — SeasonWars API Guide

> Connect to SeasonWars in 30 minutes.  
> No backend. No indexer. Just on-chain data.

---

## 1. Connect to the contract

```js
import { ethers } from 'ethers';

const SEASON_WARS_ADDRESS = '0x5598778158a66d376bd96243FC6bc27316fD2fc8';
const RPC = 'https://rpc.sepolia.org';

const provider = new ethers.JsonRpcProvider(RPC);

const ABI = [
  'function getCurrentSigns() view returns (uint8 hot, uint8 cold, uint8 opposite)',
  'function getLeaderboard(uint256 cycleId) view returns (uint256[12])',
  'function getPlayerInfo(uint256 cycleId, address player) view returns (tuple(uint8 clanSign, bool registered, uint256 burnPoints, uint256 rawBurned, uint256 playerRewardBucket, bool playerRewardClaimed, bool clanRewardClaimed))',
  'function quoteBurn(uint256 cycleId, address player, uint8 zodSign, uint256 amount) view returns (uint256)',
  'function currentCycleId() view returns (uint256)',
  'event ZODBurned(uint256 indexed cycleId, address indexed player, uint8 zodSign, uint256 amount, uint256 burnPoints)',
];

const contract = new ethers.Contract(SEASON_WARS_ADDRESS, ABI, provider);
```

---

## 2. Read current signs

```js
const { hot, cold, opposite } = await contract.getCurrentSigns();

// hot = 1 (Taurus), cold = 0 (Aries), opposite = 7 (Scorpio)
// Signs: 0=Aries 1=Taurus 2=Gemini 3=Cancer 4=Leo 5=Virgo
//        6=Libra 7=Scorpio 8=Sagittarius 9=Capricorn 10=Aquarius 11=Pisces

console.log(`HOT: ${hot} · COLD: ${cold} · OPPOSITE: ${opposite}`);
```

Call this every 60 seconds to stay in sync with the protocol.

---

## 3. Read the leaderboard

```js
const cycleId = await contract.currentCycleId();
const points = await contract.getLeaderboard(cycleId);

// points[0] = Aries burn points
// points[7] = Scorpio burn points
// etc.

const leaderboard = points
  .map((pts, i) => ({ sign: i, pts: pts.toString() }))
  .sort((a, b) => Number(b.pts) - Number(a.pts));
```

---

## 4. Simulate a burn — `quoteBurn()`

This is the core primitive. Use it before any burn decision.

```js
const cycleId = await contract.currentCycleId();
const zodSign = 0;  // Aries (COLD) — best efficiency this cycle
const amount = ethers.parseEther('1000');

const expectedPoints = await contract.quoteBurn(
  cycleId,
  userAddress,
  zodSign,
  amount
);

console.log(`Expected burn points: ${expectedPoints}`);
// Aries (COLD) → 1000 tokens → 1200 points (×1.2 multiplier)
```

**Multipliers:**
- COLD token → ×1.2 (+20%)
- OPPOSITE token → ×1.1 (+10%)
- HOT or neutral → ×1.0 (no bonus)

---

## 5. Listen to live burns

```js
contract.on('ZODBurned', (cycleId, player, zodSign, amount, burnPoints) => {
  console.log({
    cycle: cycleId.toString(),
    player,
    sign: zodSign,
    amount: ethers.formatEther(amount),
    points: burnPoints.toString(),
  });
});

// Don't forget to clean up
// contract.off('ZODBurned', handler);
```

Use this to power live feeds, alerts, or leaderboard updates.

---

## 6. Read a wallet position

```js
const info = await contract.getPlayerInfo(cycleId, walletAddress);

console.log({
  clan: info.clanSign,
  registered: info.registered,
  burnPoints: info.burnPoints.toString(),
  rawBurned: ethers.formatEther(info.rawBurned),
  pendingReward: ethers.formatEther(info.playerRewardBucket),
});
```

---

## What you can build with this

| Use case | Functions needed |
|---|---|
| Strategy bot | `getCurrentSigns()` + `quoteBurn()` |
| Live dashboard | `ZODBurned` event + `getLeaderboard()` |
| Wallet analyzer | `getPlayerInfo()` + `getLeaderboard()` |
| Burn optimizer | `quoteBurn()` across all 12 signs |
| Alert system | `ZODBurned` event filter by address |

---

## Contracts (Sepolia Testnet)

| Contract | Address |
|---|---|
| SeasonWars V2.3 | `0x5598778158a66d376bd96243FC6bc27316fD2fc8` |
| xZOD | `0x017f4333Aa7e83fA42d119d5489c41e3648c9D2f` |

Full contract list → [xzod-contracts](https://github.com/AiorosxZ/xzod-contracts)

---

Part of the **xZod Network** ecosystem.  
[xzod.io](https://xzod.io) · [Whitepaper](https://xzod.io/whitepaper)

*"In Zod we trust."* 🔥
