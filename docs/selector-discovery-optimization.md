# Selector discovery optimization (for faster Atlas Catalyst runs)

Yes — your discovery output is useful.

The main value is not just having selectors, but using them in a **ranked registry** so Atlas can execute deterministically instead of thinking through alternatives.

## Why your discovery output helps
- It identifies major UI zones (`contentTree`, `activityEditor`, `taskEditor`, `templates`, `verification`).
- It captures control intent (`controlType`) which is useful for action routing.
- It gives concrete anchors for the three target templates (Multiple Choice, Gapfill, Categorisation).

## What to fix before relying on it in production
1. **Avoid text selectors with hard-coded content IDs/names**
   - Example: `text('Activity ID: 297369')` is brittle.
   - Replace with stable labels/containers where possible.

2. **Avoid generic selectors that match multiple unrelated controls**
   - Example: `input[type='checkbox']`, `input[type='text']`, `select`.
   - Scope them to nearby labels/sections (e.g., task settings panel).

3. **Escape apostrophes safely in selector strings**
   - Example: `text('Adam's playground')` can break parsing.
   - Use double quotes around the outer string or escaped apostrophes.

4. **Split multi-target selectors into single-purpose keys**
   - Prefer one selector per action (e.g., `activityNodeMultipleChoice`, `activityNodeGapfill`).

5. **Add fallback chains and confidence levels**
   - Every key should have: `primary`, optional `fallbacks[]`, `confidence`.

## Recommended selector registry shape

```json
{
  "version": "0.2.0",
  "selectors": {
    "taskEditor.promptInput": {
      "primary": "TODO_STABLE_SELECTOR",
      "fallbacks": ["TODO_FALLBACK_SELECTOR"],
      "controlType": "text-input",
      "confidence": "high"
    }
  }
}
```

## Runtime strategy to reduce Atlas "thinking time"
1. Preload only selectors needed for templates in current payload.
2. For each field write:
   - try `primary`
   - try `fallbacks` in order
   - fail fast with logged blocker after max retries.
3. Disable exploratory discovery unless all selector options fail.
4. Cache successful selector-path per environment and reuse next run.

## Quick acceptance checklist for selector quality
- [ ] Selector is unique in current view.
- [ ] Selector survives refresh and new activity IDs.
- [ ] Selector does not depend on user-generated text.
- [ ] Selector maps to one control/action only.
- [ ] Selector has at least one tested fallback when possible.

## Suggested next action
Take your discovery JSON and convert it into a ranked selector registry with:
- `primary` + `fallbacks`
- `confidence`
- `scope` (content tree / activity editor / task editor)
- `templates` applicability (multiple-select / gapfill / categorization)

Then wire this registry into the operator pack preamble so Atlas executes first and reasons second.
