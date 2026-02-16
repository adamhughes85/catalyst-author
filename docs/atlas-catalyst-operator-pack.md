# Atlas Catalyst Operator Pack (Reusable Instructions)

Use this pack to make Atlas faster and more consistent when entering lesson content in Catalyst UI.

## 1) What to automate
For every lesson-entry run, provide Atlas with 3 fixed inputs:
1. **System instruction block** (stable operating rules)
2. **UI playbook** (step-by-step Catalyst interaction strategy)
3. **Payload** (`lesson-plan` JSON to enter)

This prevents Atlas from rediscovering UI behavior each run.

## 2) Recommended architecture

### A. Persistent instruction template
Store one instruction template and inject it automatically before each run.

- Source of truth: `examples/atlas-system-instructions-template.md`
- Include:
  - strict scope (only edit target lesson/activity)
  - deterministic entry order
  - retry and fallback behavior
  - logging requirements

### B. Template-specific UI playbooks
Keep one playbook per template:
- Multiple Select
- Gap Fill
- Categorization

For each playbook, include:
- exact field order
- selector hints
- expected control type (text, checkbox, dropdown, option row)
- validation checks after save

### C. Preflight + postflight hooks
Run lightweight checks before and after Atlas UI automation:
- **Preflight:** validate JSON shape and required fields.
- **Postflight:** compare authored UI preview vs expectedResponse mapping.

## 3) Turn this into a repeatable pipeline

## Step 1: Build a single “run envelope”
For each run, generate:
- `run_id`
- `target_template_set` = `[multiple-select, gap-fill, categorization]`
- payload path
- instruction template version
- selector map version

## Step 2: Auto-compose Atlas prompt
Compose Atlas prompt with this order:
1. System Instructions (template)
2. UI Playbook snippets for templates used in payload
3. Payload JSON
4. Output contract (`entry_log.json`, screenshots, issues)

## Step 3: Require structured progress output
Ask Atlas to return progress in fixed phases:
- `phase: preflight`
- `phase: ui-entry`
- `phase: verification`
- `phase: done`

This helps detect stalls and reduces ambiguity.

## 4) Performance tuning for slower runs
- Keep instructions short and deterministic (avoid prose-heavy guidance).
- Force task-level ordering (one task at a time, save checkpoints).
- Reuse known selectors from a selector registry.
- Disable exploratory behavior (“do not search globally; follow provided selectors first”).
- Require quick fail + retry limits (e.g., 2 retries per field group).

## 5) Suggested operator prompt contract
Use this exact pattern for each run:

1. **Role and objective**
   - “You are a Catalyst content-entry operator. Enter payload exactly as provided.”
2. **Rules**
   - No schema changes.
   - No extra activities/tasks.
   - Keep IDs and option text exact.
3. **Execution order**
   - lesson metadata → activity shell → task 1 → task 2 → task 3 → preview checks.
4. **Verification**
   - verify prompt text, options, correct answers, and template behavior.
5. **Return format**
   - structured log with successes/failures + blockers.

## 6) Immediate implementation checklist
- [ ] Add and maintain `atlas-system-instructions-template.md`.
- [ ] Add and maintain `catalyst-selector-registry.json`.
- [ ] Add a small script in your orchestrator to auto-compose prompt from template + payload.
- [ ] Add postflight assertion checks against expectedResponse.
- [ ] Capture run metrics (duration, retries, failures per template).

## 7) Minimal fields to track per run
- run_id
- payload_id
- template_types
- started_at / ended_at
- retries_count
- manual_interventions
- final_status
- blocker_reason (if failed)
