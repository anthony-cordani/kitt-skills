---
name: review
description: Independently review a committed diff before merge using a different model or agent from the author. Use after implementation against a spec, or when asked to "review this change", "check this before merge", or "review base..head". Checks scope, conventions, edge cases, and acceptance criteria, reports evidence-backed findings, and blocks approval while HIGH or CRITICAL issues remain.
license: MIT
metadata:
  version: "1.0.0"
---

# Independent pre-merge review

Have a different model or agent than the one that wrote the change review it. Give the reviewer the original spec, allowed file list, project rules, acceptance criteria, gate evidence, and committed base and head revisions. A fresh review by the author does not provide independence.

Review the committed range `base..head`. Uncommitted work is invisible to this review: a working-tree edit cannot resolve a finding until it is committed and included in a new head. Do not commit work implicitly to make it visible.

If the spec, revisions, diff, or independent reviewer is unavailable, report what is missing and return BLOCKED. An empty diff is not a completed review.

## 1. Check scope mechanically

Resolve the supplied base and head to exact commit IDs and use those same IDs for every check and the reviewer. Run:

```sh
git diff --name-status --no-renames "$base..$head"
git diff --no-ext-diff --no-renames "$base..$head"
```

Compare every changed path against the spec's allowed list, including new and deleted files and both paths of a rename. Using `--no-renames` exposes a rename as a deletion and an addition. A file outside the allowed list is a HIGH scope finding. Report command errors as a blocked check, never as an empty diff.

## 2. Review the change against the spec

Read the relevant source around each change, not only the patch. Cover these angles in one independent pass:

- Conventions: project rules, naming, structure, architecture boundaries, and maintainability.
- Edge cases: invalid or empty inputs, failures, security, concurrency, compatibility, and performance where applicable.
- Acceptance: each requirement and required test in the spec, including negative constraints and unrelated changes.

Do not modify application code during the review. Do not invent defects: support each finding with source evidence and a concrete failure scenario. Do not downgrade a severity to make a merge easier; a correction needs evidence.

## 3. Classify and report findings

Use these severities:

- CRITICAL: an immediate blocker such as severe security exposure, data loss, or a crash in an essential path.
- HIGH: a functional bug, major architecture failure, unmet essential requirement, or out-of-scope change.
- MEDIUM: a limited-impact defect or maintainability problem that does not block acceptance.
- LOW: a cosmetic or minor clarity issue.

Every finding must include severity, angle, `file:line` at the reviewed head (or base for deleted code), the problem, source evidence, a concrete trigger and failure scenario, and a precise corrective recommendation. State the affected requirement when applicable. Keep preferences separate from defects.

Return this report:

```text
RANGE: exact base..head commit IDs
REVIEWER: independent model or agent, and author identity
SCOPE: allowed-list comparison and command results
FINDINGS: severity | angle | file:line | problem | evidence | failure scenario | recommendation
COUNTS: CRITICAL, HIGH, MEDIUM, LOW
VERDICT: APPROVED or BLOCKED
NEXT: ordered fixes or remaining verification
```

The verdict is binary: BLOCKED whenever any unresolved CRITICAL or HIGH finding exists across the scope check and independent review. Missing or incomplete review also means BLOCKED. APPROVED requires a completed independent review with zero unresolved CRITICAL and HIGH findings.

## 4. Verify findings, fix, and re-review

The author verifies every finding against the source before fixing it. Record each as CONFIRMED or REFUTED with the source line, test, or reproduction that decides it. Keep refuted findings and their proof in the report. Do not silently discard a finding or dismiss it by preference; unresolved disagreements remain open.

Fix confirmed findings within the spec's scope, rerun the relevant gates, and include the fixes in a new committed head through the project's authorized workflow. Send the updated `base..head` range and finding dispositions back to the independent reviewer. Recheck scope and review the whole updated range for regressions until the review converges.

Stop after two consecutive review passes that keep producing new HIGH findings. Change the design or escalate to the human before continuing; repeated patching is not convergence. Return BLOCKED with the remaining findings. Unresolved CRITICAL findings also keep the verdict BLOCKED.

If the fixes need a new design decision or a wider scope, return to [spec-first](../spec-first/SKILL.md) and resolve the spec before implementation. An APPROVED review is evidence for the project's merge process, not permission to merge or publish.
