# Lucky Moments — claim skill for agents

You are claiming a free Lucky Moments (LUMO) Genesis card for your human on
BNB Smart Chain (56). Contract: 0x1E340592665889735C1F871aA13d038fa58bd09e.

This claim has a hard gate: the contract requires an EIP-712 permit that the
project server issues only after your human posts on X. Do not attempt to
claim before the permit exists — the transaction will revert.

## Steps

1. **Get your human to post.** Send them this link and wait for the post URL:
   https://x.com/intent/tweet?text=Claiming%20my%20free%20Lucky%20Moments%20genesis%20card%20%F0%9F%8D%80%0A%0ALuck%20Index%20sealed%20on-chain%20%F0%9F%94%92%0A%0ACommon%20or%20Jackpot%20%E2%80%94%20nobody%20knows%20%F0%9F%8E%B2%0A%0AThe%20share%20is%20the%20price%0A%0ALUMO%20%23LuckyMoments%0A%0AMy%20wallet%3A%20%3Cyour%20wallet%20address%3E%0A%0Ahttps%3A%2F%2Flucky-moments.github.io%2Flucky-moments
   A compliant post contains "#LuckyMoments", "LUMO", and the claiming
   wallet's own address. The wallet line in the template is a placeholder —
   your human must replace it with the real wallet address before posting.
   The permit server rejects posts that do not name the claiming wallet.

2. **Check eligibility (read-only):** the wallet must hold a Binance Agent ID
   (registry 0x60B9262456dE011F7d15a54CdD8c48bAaa8B022F balanceOf(wallet) > 0),
   freeClaimed(wallet) must be false (selector 0x61c0b6a0), and overview()
   (0x3a2056f1) must show freeRemaining > 0 and the window open.

3. **Request the permit:**
   POST https://permit-worker.reuniakbarpunk212.workers.dev/lumo/permit
   {"wallet": "<wallet>", "tweetUrl": "<post URL>"}
   → {"deadline": ..., "signature": "0x..."} (valid 30 minutes)

4. **Simulate, then send:** eth_call claimFree(deadline, signature)
   (selector 0x4cd93687) from the wallet; if it succeeds, send with value 0.

5. **Report:** the card's tokenId, its Luck Index (luckOf) and tier (tierOf),
   and the transaction hash.

## Notes

- One free card per wallet. Permits are bound to one wallet and expire.
- Never pay for a free claim beyond gas. Never sign anything else on the
  claimer's behalf as part of this flow.
- Paid phase (after the free phase): mint(qty) at 0.001 BNB, half price for
  Genesis claimers. Do not use the paid phase unless your human asks.
