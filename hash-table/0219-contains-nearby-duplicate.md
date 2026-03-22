# 219. Contains Nearby Duplicate

**Difficulty**: Easy
**Primary Tag**: hash-table
**Secondary Tags**: array, sliding-window
**LeetCode Link**: https://leetcode.com/problems/contains-duplicate-ii/

---

## Problem Summary

Given an integer array `nums` and an integer `k`, return `true` if there exist two distinct indices `i` and `j` such that `nums[i] == nums[j]` and `abs(i - j) <= k`.

## Screenshot

![0219-contains-nearby-duplicate](../assets/screenshots/0219-contains-nearby-duplicate.png)

---

## My Mistake(s)

- Placed `records[value] = index` in the wrong scope, so new values were never added to the map.
- Missed that the dictionary should store the **latest** index of each number (overwrite on each visit).
- Forgot to update the current value's index after each iteration.

## Key Insight

Record `value → latest index` in a dictionary. On each visit, if the value already exists, check whether `index - records[value] <= k`. Always update the stored index afterwards so it stays current for future comparisons.

## Correct Approach

1. Initialize an empty hash map `records`.
2. Iterate with `enumerate(nums)`.
3. If `value` is already in `records` and `index - records[value] <= k`, return `True`.
4. Update `records[value] = index` (outside the inner `if`, always at the end of the loop body).
5. If the loop ends without finding a match, return `False`.

```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        records = {}
        for index, value in enumerate(nums):
            if value in records:
                if index - records[value] <= k:
                    return True
            records[value] = index
        return False
```

**Time Complexity**: O(n)
**Space Complexity**: O(n)

---

## Practice History

| Date | Outcome | Notes |
|------|---------|-------|
| 2026-03-21 | ✅ | Accepted — 20 ms, beats 99.22% runtime; 23.81 MB, beats 89.70% memory |
