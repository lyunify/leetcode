# 122. Best Time to Buy and Sell Stock II

**Difficulty**: Medium<br>
**Primary Tag**: greedy<br>
**Secondary Tags**: array, dynamic-programming<br>
**LeetCode Link**: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/

---

## Problem Summary

Given an array `prices` where `prices[i]` is the stock price on day `i`, return the maximum profit you can achieve with unlimited transactions (but you may not hold more than one share at a time).

## Screenshot

![0122-best-time-to-buy-and-sell-stock-ii](../assets/screenshots/0122-best-time-to-buy-and-sell-stock-ii.png)

---

## My Mistake(s)

- **Mixed up "unlimited transactions" with hard DP**: this problem has a simple greedy solution; overbuilding state for day/holding hides the observation that every positive daily climb should be captured.
- **Forgot the cooling/fee variants**: II has no fee and no cooldown; the template changes for 714 (with fee) or 309 (with cooldown) — reusing II's code blindly fails those.
- **Wrong anchor update**: profit should use `start` before bumping it, then always set `start = prices[i]` (including when we do not sell), so the next segment starts from the latest valley.
- **Off-by-one on empty/single day**: with `len < 2`, max profit is 0; accessing `prices[0]` only is fine, but mentally confusing "need at least two days for any trade."

## Key Insight

- **Local peaks and valleys**: any time `prices[i] > prices[i-1]`, you could buy at `i-1` and sell at `i`; summing those differences equals the maximum profit under unlimited trades with no simultaneous holdings.
- **One-pass form**: keep `start` as the last reset price; if `prices[i] > start`, add `prices[i] - start`, then always `start = prices[i]` — after a drop, `start` tracks the new low for the next uptick.
- **Equivalence**: `max += Math.max(0, prices[i] - prices[i-1])` in a loop from `i=1` is the usual textbook version; same optimum, different state variable.
- **Complexity**: O(n) time, O(1) extra space — one scan, no aux structure needed.

## Correct Approach

Scan left to right. Whenever today's price exceeds our anchor (`start`), lock in that gain and move the anchor forward. Always update the anchor to today's price so the next comparison starts from the most recent level.

```java
class Solution {
    public int maxProfit(int[] prices) {
        int max = 0;
        int start = prices[0];
        int len = prices.length;

        for (int i = 1; i < len; i++) {
            if (start < prices[i]) {
                max += prices[i] - start;
            }
            start = prices[i];
        }

        return max;
    }
}

// Textbook one-liner equivalent
// for (int i = 1; i < prices.length; i++)
//     max += Math.max(0, prices[i] - prices[i-1]);
```

**Time Complexity**: O(n)<br>
**Space Complexity**: O(1)

---

## Practice History

| Date | Outcome | Notes |
|------|---------|-------|
| 2026-04-01 | ✅ Solved after review | Over-engineered with DP; wrong anchor update; confused with fee/cooldown variants |
