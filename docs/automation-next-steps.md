# Automation next steps for faster Atlas runs

1. Use `examples/atlas-system-instructions-template.md` as a fixed preamble in every Atlas run.
2. Pass `examples/catalyst-selector-registry.json` as selector context (filled with real selectors).
3. Pass the payload (e.g., `examples/lesson-plan-pilot-multi-gap-categorization.json`).
4. Require structured output (`preflight_result`, `ui_actions_log`, `verification_result`, `blockers`, `final_status`).
5. Save each run with a `run_id` and compare timing/retries to improve speed over time.

6. Convert discovery-mode output into the ranked registry format in `docs/selector-discovery-optimization.md` and keep it versioned.

7. Use the fixed baseline fast prompt template in `docs/atlas-fast-run-prompt.md` and only swap payload path + run_id.
