# 242. Valid Anagram

**Difficulty**: Easy<br>
**Primary Tag**: hash-table<br>
**Secondary Tags**: string<br>
**LeetCode Link**: https://leetcode.com/problems/valid-anagram/

---

## Problem Summary

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

## Screenshot

![0242-valid-anagram](../assets/screenshots/0242-valid-anagram.png)

---

## My Mistake(s)

### 2026-04-15

- **Delayed the length check.** Forgetting or postponing `s.length() == t.length()` can incorrectly accept cases where `t` is a strict superset of `s`.
- **Used a heavier approach (sorting or HashMap) unnecessarily.** The constraint of lowercase English letters allows a constant-size `int[26]` array — sorting is O(n log n) and a map adds boxing overhead.
- **Missed the early-negative guard.** Returning false as soon as any count goes negative would catch non-anagrams sooner and simplifies correctness reasoning.

### 2026-03-27

- **Forgot the length check.** Anagrams must use the same multiset of characters, so `|s| == |t|` is necessary. An early length mismatch should return false immediately.
- **Used sorting but compared wrong things.** Sorting both strings and comparing works, but mixing up references (only sorting one copy, comparing original to sorted, or using `==` on strings after sort) causes failures. Also O(n log n) vs O(n) counting.
- **Off-by-one / wrong bucket for `char - 'a'`.** Assuming lowercase English only: index must be `c - 'a'` in [0, 25]. Non-lowercase input in another variant would break a size-26 array without a different map.
- **Counting only one string.** Building counts from `s` and never reconciling with `t` is wrong — you need equality of frequencies for every character, not "s fits inside t."
- **Early return on first mismatch in a fragile way.** For the "increment for s, decrement for t" pattern, returning false only when counts go negative is correct only if lengths are equal first; otherwise also ensure all buckets end at zero.
- **Used `HashMap<Character, Integer>` unnecessarily.** Not a logical error, but overkill when the alphabet is fixed at 26; a plain `int[26]` is simpler and avoids boxing/autoboxing mistakes.
- **Confused anagram with "same unique letters".** Same set of distinct characters is not enough; multiplicities must match (`"aab"` vs `"abb"` is NOT an anagram).

- Mixed up which dictionary to update and which key to use: wrote `countT[s[i]]` and `countS.get(t[i], 0)` instead of updating `countT` with `t[i]` and reading from `countT` for `t`.
- Flipped the final check: used `if countS[c] == countT.get(c, 0): return False` instead of `!=`.
- Did not see `countS[s[i]] = 1 + countS.get(s[i], 0)` as "read old count → add 1 → write back"; the left side is the slot (key = letter), the right side is the new value.
- Confused `[]` on a string (`s[i]` = index) with `[]` on a dict (`countS[letter]` = key); same brackets, different meaning.
- Wondered why it must be `countS.get(...)` not `get(...)` — `.get` is a method on that specific dict; you must say which map you are reading.
- Thought `countT[c]` was fine after the loop, but `c` is only guaranteed to be a key in `countS`; if `t` never had that letter, `countT[c]` raises `KeyError` — need `countT.get(c, 0)`.
- Was unclear what `c` is in `for c in countS`: in Python, iterating a dict gives keys — each `c` is a letter that appeared in `s`.
- Overloaded on names (`letter_s`, `old_count_s`, `i`) until separating: `i` = position, `s[i]` / `letter_s` = character, `countS[...]` = frequency table.

## Key Insight

### 2026-04-15

- **Invariant is frequency, not position.** An anagram means the same multiset of letters — matching character frequencies, not positions.
- **`int[26]` is the ideal structure here.** For lowercase English letters, count up while scanning `s`, subtract while scanning `t`, and fail early if any count goes negative.
- **Length check upfront.** `s.length() == t.length()` prevents wasted work and false positives.

### 2026-03-27

- **Formal condition:** s and t are anagrams iff they have identical length and identical per-character frequencies (multiset equality).
- **Linear-time recipe:** a single `int[26]` — pass over s (increment) and t (decrement) yields all zeros iff anagram; with equal length, any negative midway is impossible.
- **Alternate view:** sorting both strings to the same canonical form and comparing — correct but O(n log n).
- **Complexity:** Time O(n); Space O(1) because alphabet size is constant (26).

- Anagram ⟺ same multiset of characters ⟺ same length and same per-letter counts.
- Build `countS` and `countT` in one pass (same index `i` for `s[i]` and `t[i]`).
- To compare, loop `for c in countS` and check `countS[c] != countT.get(c, 0)`: `.get(c, 0)` means "zero occurrences in `t` if the key is missing," avoiding `KeyError` and catching different letter sets.
- `dict.get(key, default)` is for safe reads when the key might not exist yet.

## Correct Approach

1. If `len(s) != len(t)`, return `False` immediately.
2. Initialize two empty dicts `countS` and `countT`.
3. Loop `for i in range(len(s))`: increment `countS[s[i]]` and `countT[t[i]]` using `.get(..., 0) + 1`.
4. Loop `for c in countS`: if `countS[c] != countT.get(c, 0)`, return `False`.
5. Return `True`.

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        countS, countT = {}, {}

        for i in range(len(s)):
            countS[s[i]] = 1 + countS.get(s[i], 0)
            countT[t[i]] = 1 + countT.get(t[i], 0)

        for c in countS:
            if countS[c] != countT.get(c, 0):
                return False

        return True
```

**Time Complexity**: O(n)<br>
**Space Complexity**: O(1) (at most 26 keys for lowercase English letters)

---

## Practice History

| Date | Outcome | Notes |
|------|---------|-------|
| 2026-03-23 | Solved after review | Mixed up dict/key assignments, flipped != check, confused KeyError on countT[c] |
| 2026-03-27 | Solved after review | Forgot length check; confused anagram with same unique letters; int[26] approach cleaner than HashMap; multiset equality is the formal condition |
| 2026-04-15 | Solved after review | Delayed length check; used sorting/map instead of int[26]; missed early-negative guard |
