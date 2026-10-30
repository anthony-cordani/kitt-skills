---
name: spec-first
description: Turn an already-analysed coding task into a tight specification with exact paths, bounded changes, required tests, and gates. Use before handing work to a coding agent or subagent, or when asked to "write a spec", "make this task gateable", or "prepare this for delegation". Surfaces blind spots before dispatch and verifies the resulting diff against the spec.
license: MIT
metadata:
  version: "1.0.0"
---

# Spec first

Resolve the problem and design before handing execution to a coding agent. Produce a self-contained spec that makes the expected changes and the evidence for completion explicit.

## 1. Establish context

Read the relevant source and project instructions. Restate the already-analysed task, intended behavior, and acceptance criteria. Include the repository location, starting revision, relevant architecture, existing APIs, naming conventions, and any context the agent needs without access to the earlier conversation.

If essential design decisions remain open, resolve them with the human before dispatch. If success has no observable verification path, define one or report the blocker.

## 2. Enumerate the changes

For each numbered change, specify:

- Exact file paths and edit sites, with the current behavior and required replacement behavior; include exact content when a mechanical edit needs it.
- The allowed files, including any new files, and the relevant symbols or APIs confirmed in the source.
- Error and edge cases, expected inputs and outputs, and acceptance criteria that a command or observation can decide.
- Any bounded implementation choice left to the agent and the criterion that decides whether it is acceptable.

State negative constraints explicitly: files not to touch, no unrelated refactoring, no new dependencies, and any task-specific bans. Any exception must be named in the spec. Do not rely on the agent to infer scope from the goal.

## 3. Hunt blind spots and loop back

Check the draft against the source:

- Is every edit site located, and does every referenced symbol or API exist?
- Are empty inputs, missing values, failures, timeouts, concurrency, and compatibility addressed where relevant, or explicitly outside scope?
- Are conventions and acceptance criteria concrete enough to prevent invented behavior?
- Are assumptions, prerequisites, and test data available and stated?
- Could the spec be satisfied mechanically while still missing the user's intended behavior?

For each material open question, ask the human a specific question before dispatch. Update the spec with the answers and repeat this checklist until no material blind spot remains. Do not hide unknowns behind an implementation choice.

## 4. Define tests and gates

Name the required tests and exact commands, working directories, prerequisites, and expected exit codes. Cover changed behavior and relevant regressions. For a bug fix, require a regression test that fails without the fix and passes with it when practical; otherwise state the verification limit.

Select the project's applicable build, type, lint, and test checks. Gates must expose real exit codes: do not mask a failed command with a successful trailing command or a pipeline that loses its status. If a successful check expects a nonzero exit code, say so explicitly. For behavior needing human inspection, define what the human must observe and leave acceptance pending until that happens.

## 5. State the stop condition and output

Tell the coding agent when to stop: after the enumerated changes and gates, or immediately on a blocker or a design decision beyond the spec. Prohibit opportunistic cleanup after completion. State whether commits or other repository actions are authorized; do not imply authorization to merge or publish.

Require this evidence-shaped output:

```text
FILES CHANGED: literal git status --porcelain --untracked-files=all
GATES: each command, its real exit code, and relevant output
UNCERTAIN: anything not verified, including gates that did not run and why
```

The agent reports facts rather than approving its own work. A gate that did not run must say "did not run".

## 6. Dispatch and verify

Hand the spec to your coding agent or subagent with the required context and project rules. Keep each task bounded by its enumerated scope.

When the result returns, verify the diff, not the agent's summary. Inspect changed and new files against the allowed list, acceptance criteria, and bans. Independently run the gate plan against the actual candidate. A reported success is not evidence that a write or check occurred. On failure, return the concrete evidence for a bounded fix; stop and revisit the spec if the failure exposes a design gap.

Once the candidate is committed through the project's authorized workflow, use [review](../review/SKILL.md) for an independent pre-merge review of the committed range. Gate success alone does not replace that review.
