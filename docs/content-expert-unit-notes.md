# Content expert unit draft notes (Animals / beginner)

This file describes the expanded sample payload for the content expert/content-entry workflow:
- `examples/content-expert-unit-animals-beginner.json`

## Intent
- Provide a larger rough-draft unit (4 lessons) for Atlas to author in Catalyst.
- Keep template usage constrained to:
  - multiple-select (`multiple-choice` with `allowMultipleSelect=true`)
  - gapfill
  - categorization (`categorisation`)
- Prioritize volume and structure over pedagogical perfection.

## Shape
- One **single unit file** containing:
  - unit metadata,
  - lessons,
  - steps,
  - activities,
  - task/questions + expected responses.
- 4 lessons total.
- Each lesson has 3 steps (within the requested 3–8 range).
- Each lesson step uses one of the three selected task templates.

## Source-of-truth rule
Use `examples/content-expert-unit-animals-beginner.json` as the canonical source for authoring.
Do **not** require Atlas to generate per-lesson JSON files at run time.

## How to use with Atlas (single-file mode)
1. Give Atlas this one file (`examples/content-expert-unit-animals-beginner.json`).
2. Instruct Atlas to author directly from this file into Catalyst, lesson by lesson.
3. For each lesson, Atlas should read the lesson object from the unit file and enter it immediately.
4. Keep logs keyed by `lessonId` and `run_id`, but keep content source in the one unit file.
