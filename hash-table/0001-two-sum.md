# 1. Two Sum

**Difficulty**: Easy<br>
**Primary Tag**: hash-table<br>
**Secondary Tags**: array<br>
**LeetCode Link**: https://leetcode.com/problems/two-sum/

---

## Problem Summary

Given an array of integers `nums` and an integer `target`, return the indices of the two numbers that add up to `target`.

## Screenshot

![0001-two-sum](../assets/screenshots/0001-two-sum.png)

---

## My Mistake(s)

### 2026-04-15

- **Updated the map before checking the complement.** Storing `nums[i]` first and then looking up the complement can pair the element with itself when `target == 2 * nums[i]`, violating the "no same element twice" rule. Always check first, then store.
- **Overthought duplicates.** Overwriting `value -> index` is safe here — a valid pair only needs one prior index, and the problem guarantees a unique solution.
- **Added unnecessary edge-case code.** The problem guarantees exactly one answer, so extra null-return branches or "no solution" handling only complicate the solution.

### 2026-03-27

- **Used the same index twice.** With a single hash map updated as you scan, you must only accept a complement stored at a different index. Putting `nums[i]` in the map before checking, or initializing everything upfront, can pair an element with itself (invalid when "you may not use the same element twice"). Fix: check first, then store.
- **Returned values instead of indices.** The problem asks for indices, not the numbers; returning `{nums[i], complement}` or confusing array values with positions is a common slip.
- **Two-pointer on an unsorted array without care.** Sorting with indices requires tracking original indices (pairs of (value, index)), extra work, and O(n log n). Naive two-pointer on unsorted nums is wrong.
- **Off-by-one / wrong loop bounds.** Nested brute force (O(n²)) has pitfalls: inner loop must start at `j = i + 1` so you do not reuse the same index.
- **Map overwrite confusion.** When duplicates exist (e.g. `nums = [3,3], target = 6`), storing value → index and overwriting earlier indices is fine: you only need one prior index for a valid pair, and the unique-solution guarantee handles the rest.
- **Assumed sorted input or built-in search on values.** Binary search on sorted values does not directly give two distinct indices in the original array unless you carry index metadata.

## Key Insight

Use a hash map to store each number and its index as you iterate. For each new number, check if its complement (`target - value`) already exists in the map. If so, you've found the answer in a single pass.

### 2026-04-15

- **Reduce to complement lookup.** The problem reduces to: has `target - nums[i]` been seen at an earlier index? A hash map answers this in O(1) on average.
- **One-pass is sufficient.** At index `i`, only earlier indices matter — check the map first, then store `nums[i] -> i`. This naturally enforces using two distinct indices.
- **"No same element twice" is free.** Because we only store past indices, the current index `i` is never in the map when we check, so self-pairing is impossible.

### 2026-03-27

- **Complement formulation:** For each index `i`, you need another index `j` with `nums[j] == target - nums[i]` and `j ≠ i`. The hash map answers "have I seen this complement before?" in O(1).
- **One-pass hash map:** Scan left to right; for each `nums[i]`, if `target - nums[i]` is already in the map, return `[storedIndex, i]`; else store `(nums[i], i)`. Time O(n), space O(n).
- **Why sorting + two-pointer is secondary:** It works if you preserve original indices, but the map avoids sorting entirely and is the favored linear solution for classic Two Sum.
- **Problem guarantee:** Exactly one solution exists, so no need to collect all pairs or handle "no answer" on LeetCode.

## Correct Approach

1. Initialize an empty hash map `records`.
2. Iterate through `nums` with `enumerate`.
3. Compute `remain = target - value`.
4. If `remain` is in `records`, return `[records[remain], index]`.
5. Otherwise, store `records[value] = index` and continue.

```python
class Solution(object):
    def twoSum(self, nums, target):
        records = {}
        for index, value in enumerate(nums):
            remain = target - value
            if remain in records:
                return [records[remain], index]
            records[value] = index
```

**Time Complexity**: O(n)<br>
**Space Complexity**: O(n)<br>

---

## Practice History

| Date | Outcome | Notes |
|------|---------|-------|
| 2026-03-21 | ✅ | Accepted — 2 ms, beats 57.26% runtime; 12.88 MB, beats 97.57% memory |
| 2026-03-27 | Solved after review | Self-pairing bug (check before store); returned values not indices; one-pass complement hash map is the canonical O(n) solution |
| 2026-04-15 | Solved after review | Updated map before checking (self-pair risk); overthought duplicates; added unnecessary edge-case code |
