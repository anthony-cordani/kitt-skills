---
name: skill-create
description: Guide a developer through creating a new skill meant for a kitt skills repository, up to its release. Not for a skill kept only on the local machine. Use when asked to "create a skill", "write a new skill", or "add a skill to our skills repository". Drives repository selection, naming, branch setup, scoping questions, dependency checks, drafting SKILL.md, opening a merge request, and releasing with kitt.
license: MIT
metadata:
  version: "1.0.0"
---

# Create a new skill

Guide the user through creating a new skill in a skills repository, opening a merge request, and publishing a release. Ask one question at a time, provide a recommendation whenever possible, and wait for the user's answer before proceeding.

The skill needs `kitt` 0.4.0 or later for `kitt show`, checked with `kitt --version`; if older, tell the user to update kitt and stop.

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
- If none exist, ask for the repository URL. Propose to register it with `kitt remote add <alias> <url>` in a project that has a `kitt.toml`, or `kitt remote -g add <alias> <url>` otherwise. If the user declines, skip the later `kitt show` and `kitt install` of this skill and give them the repository URL instead.

Locate or prepare a local clone: ask the user where it is, or where to clone it. A skills repository holds `skills/<name>/SKILL.md`. The clone may be a fork: run `git remote -v` and note which remote points at the chosen repository. Step 7 needs it.

Later `kitt list`, `kitt show`, and `kitt install` calls run from the place that gave the sources (the project directory), with `-g` if the sources came from `kitt remote -g`. `kitt release` and the git commands run in the clone.

## 2. Choose the name and start a branch

Enforce kitt naming rules: lowercase letters, digits, and hyphens only, 1 to 64 characters, no leading, trailing, or double hyphens, and the name must not start with `kitt-`.

The check for an existing skill and the branch start both use the remote state: run `git fetch`, then look for `skills/<name>/` on `<remote>/<default>`, where `<remote>` is the remote pointing at the chosen repository. If `skills/<name>/` already exists on `<remote>/<default>`, stop and offer a different name.

If the branch `skill/<name>` already exists, ask whether to resume it or use another name. Never reset or delete it. Otherwise, create it with `git switch -c skill/<name> <remote>/<default>`. If the clone's working tree is dirty, ask before taking any action: never stash or discard the user's work.

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

Look at the names from `kitt list`, pick at most five plausible candidates from the user's answers, and read each candidate's description with `kitt show <alias>/<name>`, which prints the description and versions, not the skill body. If the description is not enough to decide, read `skills/<name>/SKILL.md` in that repository or ask the user. Propose the ones that fit with a one-line reason, or state that none fit.

Declare dependencies with a space-separated string in `metadata.requires`: `"code-review@^1.0.0 public/security@^1"`, using `name@constraint` for the same source and `alias/name@constraint` for another source. The constraint is mandatory: exact `X.Y.Z` or caret `^X[.Y[.Z]]`. A bare `name@constraint` means the same repository as this skill. A qualified `alias/name@constraint` also needs that alias under `[sources]` in the `kitt.toml` at the root of the skills repository, because `kitt release` reads that file, never the user's: check it now and add the alias in this merge request if it is missing.

Verify each candidate immediately with `kitt show <alias>/<name>@<constraint>`. `kitt release` rejects any requirement without a matching published release, at which point the merge request is already merged.

## 5. Write the skill

Write `skills/<name>/SKILL.md`.
- Frontmatter: `name` matching the folder name, a one-paragraph `description` (at most 1024 characters) stating purpose, when to use it, and trigger phrases, quoted `metadata.version: "1.0.0"`, and `license` if the repository uses one.
- Body: imperative instructions, what to do when something is missing, a stop condition, and an output format if the skill produces artifacts.
- Move long reference material into `skills/<name>/references/`.
- Add a row to the repository's skill table if its README has one.

Follow the repository's own conventions when they differ. Show the user the full text and apply their changes before committing.

## 6. Prepare the merge request

Commit on `skill/<name>`. Ask before pushing.

On yes, run `git push -u origin skill/<name>`. In a fork, push to the user's fork and open the merge request against the chosen repository. Use the tool of the host: `gh pr create` on GitHub, `glab mr create` on GitLab. On any other host, or when that tool is missing, give the URL or the steps to open the merge request by hand.

If the push is refused, report the error and explain the options (request repository access, or fork): never push to the default branch, and never force-push.

Tell the user the next step is theirs: have the merge request reviewed and merged. Until it is released the skill has no version, so a project installing it would follow the default branch instead of a pinned release, and a skill that requires it would fail. Tell them to return after merge and ask to release `<name>`. Stop here; do not wait for the merge.

## 7. Release, once merged

This step also runs standalone when the user asks to "release <name>".

Check the merge request of the branch by name: `gh pr view skill/<name>` or `glab mr view skill/<name>`, or ask the user. If it was closed without merging, say so and stop. If it is not merged, stop.

Run on the default branch of the clone, level with the remote pointing at the chosen repository, on a clean tree:
1. Run `git switch <default>` and `git pull --ff-only`. If the working tree is dirty, ask before switching.
2. Show the exact command in the proposal and run it only on the user's explicit yes. If that remote is not `origin`, the command is `kitt release --remote <remote> <name>`, because kitt pushes the tag to `origin` by default and would otherwise publish to the fork. Otherwise, run `kitt release <name>`. `kitt release` verifies requirements, then creates and pushes the tag `<name>/v1.0.0`.

Report kitt's error as it is. kitt removes the local tag when the push fails. If the error is a refusal or a permission error, explain that tags may be protected in this repository and someone allowed to publish must run the same command. If it is a network or authentication error, fix that first and retry on the user's yes. Do not work around a refusal.

On success, confirm with `kitt show <alias>/<name>` and offer `kitt install <alias>/<name>`.
