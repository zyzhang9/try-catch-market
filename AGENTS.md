# Repository Notes For AI Agents

This repository is primarily a writing workspace. The main working areas are `drafts/`, `published/`, and `editorial/`. The `projects/` directory exists as reference material for writing and should usually be treated as source context rather than the main editing target.

## Git Safety

Be careful when committing or pushing from this repository.

A previous agent workflow exposed an important issue: there may already be unrelated files staged in the index before you start your own work. If you run `git commit` without checking the staged set carefully, your commit may include files that were not part of your intended change.

Before every commit or push:

1. Run `git status --short`.
2. Run `git diff --cached --name-only`.
3. Confirm that every staged file belongs to the current task.
4. If unrelated files are already staged, do not include them accidentally. Stage only the files for the current task, or pause and surface the ambiguity to the user.

Do not assume that "files I just added" is the same as "everything currently staged".

## Editing Guidance

- Prefer editing article files under `drafts/` unless the user explicitly asks for changes elsewhere.
- Treat untracked or modified files outside the current task as user work unless clearly told otherwise.
- When using `projects/` as reference material, align article claims with the actual code structure before editing prose.
