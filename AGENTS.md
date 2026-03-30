# Repository Notes For AI Agents

This repository is primarily a writing workspace. The main working areas are `posts/` and `editorial/`. The `projects/` directory exists as reference material for writing and should usually be treated as source context rather than the main editing target.

Each article under `posts/` should usually be treated as its own dossier. In the normal workflow:

- `diary.md` contains the user's source notes, facts, pasted references, and working record
- `post.md` contains the publishable article written from those materials

The `old/` directory is only a temporary migration holding area for legacy content from the previous structure and should not be treated as the main workspace.

## Git And Network Notes

Still check the staged set before committing:

1. Run `git status --short`.
2. Run `git diff --cached --name-only`.
3. Confirm that every staged file belongs to the current task.

## Editing Guidance

- Prefer editing article files under `posts/` unless the user explicitly asks for changes elsewhere.
- Treat untracked or modified files outside the current task as user work unless clearly told otherwise.
- When using `projects/` as reference material, align article claims with the actual code structure before editing prose.
