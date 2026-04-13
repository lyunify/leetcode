# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repository stores personal LeetCode mistake notes, organized by problem tag. Each note documents what went wrong, the key insight, the correct solution, and a running practice history.

## Repository Structure

- **Tag folders** (`array/`, `hash-table/`, `two-pointers/`, etc.) — one Markdown note per problem, stored under the problem's primary tag.
- **`assets/screenshots/`** — all problem screenshots, named `number-title.png`.
- **`TEMPLATE.md`** — the template for every new note.

## Naming Convention

| Type | Format | Example |
|------|--------|---------|
| Problem note | `number-title.md` | `0001-two-sum.md` |
| Screenshot | `number-title.png` | `0001-two-sum.png` |

Use lowercase, hyphen-separated slugs. Zero-pad the problem number to 4 digits.

## Screenshot Workflow

When a problem screenshot is sent:

1. **Run `git pull`** before making any changes.
2. Determine the problem number, title, and primary tag from the screenshot.
3. Save the screenshot to `assets/screenshots/number-title.png`. If a screenshot already exists for this problem, **replace it** with the new one.
4. Check whether `<primary-tag>/number-title.md` already exists:
   - **Does not exist** — create it from `TEMPLATE.md`, filling in all known fields.
   - **Already exists** — append a new row to the Practice History table with today's date and outcome. Do not overwrite existing content.
5. If the problem has multiple tags, store the note under the **primary** tag folder and list all secondary tags in the note's `Secondary Tags` field.
6. Stage, commit, and push:
   ```
   git add .
   git commit -m "note: number-title (YYYY-MM-DD)"
   git push
   ```

## Multi-Practice Tracking

Every time the same problem is practiced, add a new row to the **Practice History** table in the existing note. Keep all prior rows — never delete practice history.

## Available Tag Folders

`array` · `hash-table` · `two-pointers` · `binary-search` · `sliding-window` · `dynamic-programming` · `tree` · `graph` · `backtracking` · `greedy` · `stack` · `queue` · `linked-list` · `string` · `math`

If a problem's primary tag is not in this list, create the new folder and add it here.
