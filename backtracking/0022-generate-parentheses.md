# 0022. Generate Parentheses

**Difficulty**: Medium<br>
**Primary Tag**: backtracking<br>
**Secondary Tags**: string<br>
**LeetCode Link**: https://leetcode.com/problems/generate-parentheses/

---

## Problem Summary

Given `n` pairs of parentheses, generate all combinations of well-formed parentheses strings.

## Screenshot

![0022-generate-parentheses](../assets/screenshots/0022-generate-parentheses.png)

---

## My Mistake(s)

- Using only `s.length() == 2 * n` as the base case without enforcing validity — this can include invalid strings that happen to reach the right length.
- Allowing `')'` when `closeP == openP` (or greater) breaks the prefix invariant and produces dead/invalid branches.
- Forgetting to import `List` and `ArrayList` causes compile errors even when the recursion logic is correct.

## Key Insight

The key invariant is `closeP <= openP` at every step, which guarantees we never build an invalid prefix. Building by counts (`openP`, `closeP`) is cleaner than validating full strings afterward because invalid branches are pruned immediately. The two decisions are independent but constrained: add `'('` when `openP < n`; add `')'` when `closeP < openP`. These two rules fully cover all valid constructions.

## Correct Approach

1. Start with an empty string and counts `openP = 0`, `closeP = 0`.
2. Base case: when `s.length() == 2 * n`, add `s` to results (validity is guaranteed by construction).
3. If `openP < n`, recurse adding `'('`, incrementing `openP`.
4. If `closeP < openP`, recurse adding `')'`, incrementing `closeP`.

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<>();
        backtrack(res, "", 0, 0, n);
        return res;
    }

    private void backtrack(List<String> res, String s, int openP, int closeP, int n) {
        if (s.length() == 2 * n) {
            res.add(s);
            return;
        }
        if (openP < n) {
            backtrack(res, s + "(", openP + 1, closeP, n);
        }
        if (closeP < openP) {
            backtrack(res, s + ")", openP, closeP + 1, n);
        }
    }
}
```

**Time Complexity**: O(4^n / √n) — the nth Catalan number<br>
**Space Complexity**: O(n) recursion depth

---

## Practice History

| Date | Outcome | Notes |
|------|---------|-------|
| 2026-04-28 | Solved after review | Missed closeP < openP guard; forgot validity isn't implied by length alone |
