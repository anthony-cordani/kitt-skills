---
name: skill-create
description: Guide a developer through creating a new skill in a skills repository, up to its release. Use when asked to "create a skill", "write a new skill", or "add a skill to our skills repository". Drives repository selection, naming, branch setup, scoping questions, dependency checks, drafting SKILL.md, opening a merge request, and releasing with kitt.
license: MIT
metadata:
  version: "1.0.0"
---

# Create a new skill

Guide the user through creating a new skill in a skills repository, opening a merge request, and publishing a release. Ask one question at a time, provide a recommendation whenever possible, and wait for the user's answer before proceeding.

Hard rules:
- Never create a release tag by hand.
- Never push directly to the default branch.
- Never use `--force` or `--no-verify`.
- Never publish on the user's behalf without their explicit yes.

Updating an existing skill is not covered here: bump `metadata.version`, open a merge request, and run the same release steps once merged.

## 1. Choose the repository

Run `kitt remote` (and `kitt remote -g` if the project has none) to list configured skill sources.
- If one source is configured, confirm it with the user.
- If several sources exist, ask which one to use.
- If none exist, ask for the repository URL.

Locate or prepare a local clone: ask the user where it is, or where to clone it. A skills repository holds `skills/<name>/SKILL.md`.

## 2. Choose the name and start a branch

Enforce kitt naming rules: lowercase letters, digits, and hyphens only, 1 to 64 characters, no leading, trailing, or double hyphens, and the name must not start with `kitt-`. If `skills/<name>/` already exists on the default branch, stop and offer a different name.

Update the default branch with `git fetch` and create the branch `skill/<name>` from it. If the clone's working tree is dirty, ask before taking any action: never stash or discard the user's work.

## 3. Ask the questions that make a solid skill

Ask at most six questions, skipping anything the user already answered:
1. What the skill does.
2. What it must not do.
3. The phrases or situations that trigger it.
4. One concrete example of input and expected result.
5. What it produces.
6. When it must stop or ask a human.

## 4. Check dependencies

The default answer is none. A dependency is justified only when this skill actually invokes the other skill.

Look at the names from `kitt list`, pick at most five plausible candidates from the user's answers, and read each with `kitt show <alias>/<name>`. Propose the ones that fit with a one-line reason, or state that none fit.

Declare dependencies with a space-separated string in `metadata.requires`: `"code-review@^1.0.0 public/security@^1"`, using `name@constraint` for the same source and `alias/name@constraint` for another source. The constraint is mandatory: exact `X.Y.Z` or caret `^X[.Y[.Z]]`.

Verify each candidate immediately with `kitt show <alias>/<name>@<constraint>`. `kitt release` rejects any requirement without a matching published release, at which point the merge request is already merged.

## 5. Write the skill

Write `skills/<name>/SKILL.md`.
- Frontmatter: `name` matching the folder name, a one-paragraph `description` stating purpose, when to use it, and trigger phrases, quoted `metadata.version: "1.0.0"`, and `license` if the repository uses one.
- Body: imperative instructions, what to do when something is missing, a stop condition, and an output format if the skill produces artifacts.
- Move long reference material into `skills/<name>/references/`.
- Add a row to the repository's skill table if its README has one.

Follow the repository's own conventions when they differ. Show the user the full text and apply their changes before committing.

## 6. Prepare the merge request

Commit on `skill/<name>`. Ask before pushing.

On yes, run `git push -u origin skill/<name>`, then `gh pr create` or `glab mr create` depending on the host. If neither CLI is available, provide the URL or steps to open the merge request manually.

If the push is refused, report the error and explain the options (request repository access, or fork): never push to the default branch, and never force-push.

Tell the user the next step is theirs: have the merge request reviewed and merged. State plainly that the skill is not installable until it is released, and tell them to return after merge and ask to release `<name>`. Stop here; do not wait for the merge.

## 7. Release, once merged

This step also runs standalone when the user asks to "release <name>".

Check that the merge request is merged using `gh pr view`, `glab mr view`, or by asking the user. If it is not merged, stop.

Propose `kitt release <name>` and run it only on the user's explicit yes. It must run in the clone, on the default branch, level with the remote, on a clean tree:
1. Run `git switch <default>` and `git pull --ff-only`. If the working tree is dirty, ask before switching.
2. Run `kitt release <name>`. `kitt release` verifies requirements, then creates and pushes the tag `<name>/v1.0.0`.

If tag pushing is refused, kitt removes the local tag. Explain that tags are protected in this repository and that someone allowed to publish must run the same command. Do not retry and do not work around it.

On success, confirm with `kitt show <alias>/<name>` and offer `kitt install <alias>/<name>`.
