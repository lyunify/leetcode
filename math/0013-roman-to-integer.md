# 13. Roman to Integer

**Difficulty**: Easy<br>
**Primary Tag**: math<br>
**Secondary Tags**: hash-table, string<br>
**LeetCode Link**: https://leetcode.com/problems/roman-to-integer/

---

## Problem Summary

Given a Roman numeral string, convert it to an integer. Roman numerals use subtractive notation when a smaller value symbol precedes a larger one (e.g. IV = 4, IX = 9).

## Screenshot

![0013-roman-to-integer](../assets/screenshots/0013-roman-to-integer.png)

---

## My Mistake(s)

- **Off-by-one on the loop.** Forgetting that the loop should stop at `length - 1` (or mishandling the last index) leads to index out of bounds when reading `s.charAt(i + 1)`, or to double-counting / missing the last symbol.
- **Subtracting in the wrong direction.** Trying to subtract the larger symbol (e.g. treating IV as 5 − 1 applied the wrong way). The one-pass rule is: subtract the *left* (smaller) unit when `left < right`, add otherwise—not "subtract the pair as a chunk."
- **Confusing subtractive cases.** IX (9) vs XI (11)—only one smaller symbol is allowed immediately before a larger one in valid numerals; the pairwise rule still handles it: I < X → subtract I, then add X.
- **Forgetting edge-case guards.** LeetCode guarantees valid input, but in interviews a null/empty string check is easy to miss if constraints change.
- **Loop bound errors.** Using `i <= s.length()` or comparing `i` with `length` incorrectly; the correct bound is `i < s.length() - 1`. Testing with I, IV, IX, LVIII, MCMXCIV catches this quickly.

## Key Insight

- **Roman numerals are locally decided by pairs.** Compare each symbol only with its right neighbor. If `value[i] < value[i+1]`, the left symbol is subtractive; otherwise it is additive. You never need to look two steps ahead for standard LeetCode rules.
- **The last character is always additive.** It has no right neighbor, so it can never be the "small half" of a subtractive pair. Many clean solutions loop `0 .. n-2` and then unconditionally add `value[last]`.
- **O(1) lookup with a fixed map.** A `Map<Character, Integer>` (or a switch on char) for the seven symbols gives O(1) per character; the full algorithm is O(n) time, O(1) extra space.
- **Mental model invariant.** Think of the running sum as: "everything processed so far, treating each position by comparing `s[i]` vs `s[i+1]`." The subtraction rule is just: "if I'm about to be cancelled by the next larger symbol, subtract me first."

## Correct Approach

1. Build a map of the seven Roman symbols to their integer values.
2. Walk indices `0` through `n-2`. At each position, if `value[i] < value[i+1]`, subtract `value[i]`; otherwise add `value[i]`.
3. After the loop, always add `value[n-1]` (the last character).

```java
class Solution {
    public int romanToInt(String s) {
        int res = 0;
        Map<Character, Integer> roman = new HashMap<>();
        roman.put('I', 1);
        roman.put('V', 5);
        roman.put('X', 10);
        roman.put('L', 50);
        roman.put('C', 100);
        roman.put('D', 500);
        roman.put('M', 1000);

        for (int i = 0; i < s.length() - 1; i++) {
            if (roman.get(s.charAt(i)) < roman.get(s.charAt(i + 1))) {
                res -= roman.get(s.charAt(i));
            } else {
                res += roman.get(s.charAt(i));
            }
        }
        res += roman.get(s.charAt(s.length() - 1));
        return res;
    }
}
```

**Time Complexity**: O(n)<br>
**Space Complexity**: O(1) (fixed 7-entry map)

---

## Practice History

| Date | Outcome | Notes |
|------|---------|-------|
| 2026-03-30 | Solved after review | Off-by-one on loop bound; subtracted in wrong direction for subtractive pairs |
