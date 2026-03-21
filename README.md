# LeetCode Mistake Notes

A personal repository for tracking LeetCode problem-solving mistakes, patterns, and progress over time.

## Structure

```
leetcode/
├── array/
├── hash-table/
├── two-pointers/
├── binary-search/
├── dynamic-programming/
├── tree/
├── graph/
├── backtracking/
├── greedy/
├── stack/
├── queue/
├── linked-list/
├── string/
├── math/
├── assets/
│   └── screenshots/       # Problem screenshots, named number-title.png
├── TEMPLATE.md            # Template for new problem notes
└── CLAUDE.md              # Workflow rules for Claude Code
```

## Naming Convention

| Type | Format | Example |
|------|--------|---------|
| Problem note | `number-title.md` | `0001-two-sum.md` |
| Screenshot | `number-title.png` | `0001-two-sum.png` |

## Workflow

1. Send a problem screenshot to Claude Code.
2. Claude creates or updates the note under the problem's primary tag folder.
3. Changes are committed and pushed automatically.

## Tags

Each problem is stored under its **primary** tag folder. Secondary tags are listed inside the note itself.
