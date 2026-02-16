# Content expert unit draft notes (Animals / beginner)

This file describes the expanded sample payload for the content expert agent:
- `examples/content-expert-unit-animals-beginner.json`

## Intent
- Provide a larger rough-draft unit (4 lessons) for Atlas to author in Catalyst.
- Keep template usage constrained to:
  - multiple-select (`multiple-choice` with `allowMultipleSelect=true`)
  - gapfill
  - categorization (`categorisation`)
- Prioritize volume and structure over pedagogical perfection.

## Shape
- One unit object containing `lessons[]`.
- 4 lessons total.
- Each lesson has 3 steps (within the requested 3–8 range).
- Each lesson step uses one of the three selected task templates.

## How to use with Atlas
1. Ask Atlas content expert to generate/refine from this file (topic: beginner animals).
2. Have Atlas output per-lesson lesson-plan payloads for the content entry agent.
3. Feed each lesson payload into the existing fast-run process.
