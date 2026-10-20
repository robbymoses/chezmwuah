---
name: code-review
description: Review code changes for correctness, regressions, test coverage, type safety, error and null handling, maintainability, and security. Use when asked to review a diff, pull request, patch, or implementation.
---

# Code Review

Review the requested changes without modifying them unless the user also asks for fixes.

Understand the change's intent and surrounding code before judging it. Inspect the diff, relevant callers and contracts, repository guidance, and nearby tests. Run focused checks when practical, and distinguish verified behavior from inference.

Prioritize issues that can cause incorrect behavior or make the change unsafe to ship:

- Incorrect logic, regressions, broken invariants, and unhandled edge cases
- Missing or weak tests for changed behavior, failure paths, boundaries, and regressions
- Unsound types, unsafe casts, incorrect narrowing, and mismatches across API boundaries
- Missing handling for errors, null or absent values, partial data, timeouts, and cleanup
- Security, privacy, authorization, injection, and unsafe input-handling problems
- Compatibility issues involving public APIs, persisted data, configuration, or migrations
- Concurrency, ordering, retry, idempotency, performance, and resource-lifecycle risks where relevant
- Duplication that can realistically drift or obscure intent; do not demand abstraction merely to remove small or coincidental repetition
- Excessive complexity, unclear ownership, or departures from established project patterns that raise maintenance risk

Evaluate tests by the behaviors and risks they cover, not raw coverage percentage. Prefer focused regression tests that would fail before the change and pass afterward. Do not require tests for trivial declarations or behavior already covered at the appropriate level.

## Reporting Findings

Report concrete, actionable findings first, ordered by severity. For each finding:

1. State the problem and likely impact.
2. Cite the smallest relevant file and line range.
3. Explain the conditions that trigger it.
4. Suggest the direction of a fix when it is not obvious.

Avoid presenting style preferences, speculative concerns, or unrelated pre-existing problems as defects. Group repeated instances when they share one cause. If no findings remain, say so plainly and mention any meaningful test or verification gaps.

After the findings, briefly note open questions or assumptions and summarize the checks performed. Never claim a test or command passed unless it was actually run.
