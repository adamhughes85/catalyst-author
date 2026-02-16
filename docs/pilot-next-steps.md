# Pilot next steps: generate content, then enter in Catalyst

## What we now have
- A concrete sample lesson payload covering the three MVP templates:
  - Multiple Select (`multiple-choice` with `allowMultipleSelect=true`)
  - Gap Fill (`gapfill`)
  - Categorization (`categorisation`)
- File: `examples/lesson-plan-pilot-multi-gap-categorization.json`

## Next steps
1. **Lock template field maps in Catalyst UI**
   - Capture exact selectors and input behavior for all fields used by the three templates.
   - Record unsupported controls and any legacy quirks.

2. **Build/validate payload adapter**
   - Add a pre-entry validator that checks:
     - required task wrapper fields,
     - taskType to task-variant consistency,
     - expectedResponse shape for each template.

3. **Create content entry dry-run**
   - Use the content entry agent to create one draft activity in Catalyst from the sample file.
   - Write logs/screenshots for each field write.

4. **QA checklist for first run**
   - Prompt text appears correctly in preview.
   - Multiple Select reflects multiple-correct answer behavior.
   - Gap Fill shows expected options and evaluates correctly.
   - Categorization target-option mapping is correct.

5. **Iterate from failures**
   - If any field fails to populate, patch selector map and rerun idempotently using the same lesson ID.

## Suggested command sequence for your team
- Validate JSON structure and readability.
- Feed `examples/lesson-plan-pilot-multi-gap-categorization.json` to the entry agent in dry-run mode.
- Review generated `entry_log.json` + screenshots.
- Promote to publish only after QA pass.
