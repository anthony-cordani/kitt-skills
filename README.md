# kitt-skills

Public agent skills you can install in any project with [kitt](https://github.com/anthony-cordani/kitt).

## Try it

```sh
brew install anthony-cordani/tap/kitt
cd my-project
kitt init --source kitt-skills=https://github.com/anthony-cordani/kitt-skills.git
kitt install humanizer
```

For an existing kitt project, add this repository with `kitt remote add kitt-skills https://github.com/anthony-cordani/kitt-skills.git`, then install a skill.

| Skill | Purpose |
| --- | --- |
| [humanizer](skills/humanizer/SKILL.md) | Rewrite AI-sounding prose while preserving facts and meaning, with English and French patterns. |
| [spec-first](skills/spec-first/SKILL.md) | Turn an analysed coding task into a scoped spec with tests, gates, and a stop condition. |
| [review](skills/review/SKILL.md) | Independently review a committed diff against its spec before merge. |

The skills work with Claude Code, Codex, Copilot, OpenCode, or another coding agent. They require no private services or project-specific setup.

## Use it as a template for your own skills repository

Put each skill in its own folder:

```text
skills/
  humanizer/
    SKILL.md
    LICENSE
    references/
      french.md
  spec-first/
    SKILL.md
  review/
    SKILL.md
```

The convention is `skills/<name>/SKILL.md`, with optional supporting Markdown files in `skills/<name>/references/`. Link to another skill with `../<name>/SKILL.md` and to a local reference with `references/<file>.md`.

Start each `SKILL.md` with Agent Skills YAML frontmatter:

```yaml
---
name: my-skill
description: Describe what the skill does and when to use it, including trigger phrases such as "prepare a task spec".
license: MIT
metadata:
  version: "1.0.0"
---
```

The `name` matches the folder name. Keep `description` to one paragraph covering purpose and when to use the skill. Set `license: MIT` and keep `metadata.version` a quoted semantic version string. No `metadata.requires` field is needed.

Release a skill with `kitt release <name>`. Releases use tags in the form `<name>/vX.Y.Z`, for example `humanizer/v1.0.0`; the tag and frontmatter version identify the release.

## License

[MIT](LICENSE). Humanizer includes upstream MIT content from [blader/humanizer](https://github.com/blader/humanizer), version 3.1.0, copyright Siqi Chen. Its upstream notice is preserved in [skills/humanizer/LICENSE](skills/humanizer/LICENSE).
