# Repository Notes For AI Agents

This repository is primarily a writing workspace. The main working areas are `posts/` and `editorial/`. The `projects/` directory exists as reference material for writing and should usually be treated as source context rather than the main editing target.

Each article under `posts/` should usually be treated as its own dossier. In the normal workflow:

- `diary.md` contains the team's working record, source facts, pasted references, and production notes
- `post.md` contains the publishable article written from those materials

Important clarification:

- `diary.md` is not a writing-plan file or article-outline scratchpad
- `diary.md` should read like the team's internal working record: context, observations, constraints, incidents, logs, partial interpretations, and engineering responses
- if you need to record writing rules, article-boundary notes, naming debates, or editorial rationale, put that in `editorial/` instead of the article dossier

## Publication Positioning

This publication is positioned as a Drift-narrated engineering memory project.

- Drift is the default narrative surface for the公众号, not an optional decorative add-on.
- Articles should normally preserve a clear separation between the acting team (`他们`) and the observing narrator Drift (`我` when Drift is explicitly present).
- New articles should generally include the closing Drift signature, and should consider 1 to 3 restrained Drift appearances in the body when the piece benefits from narrative continuity.
- If a draft does not use Drift, treat that as an exception that should be consciously justified by the article rather than the default.

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
