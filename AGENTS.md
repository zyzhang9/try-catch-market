# Repository Notes For AI Agents

This repository is primarily a writing workspace. The main working areas are `posts/` and `editorial/`. The `projects/` directory exists as reference material for writing and should usually be treated as source context rather than the main editing target.

Each article under `posts/` should usually be treated as its own dossier. In the normal workflow:

- `diary.md` contains the user's source notes, facts, pasted references, and working record
- `post.md` contains the publishable article written from those materials

The `old/` directory is only a temporary migration holding area for legacy content from the previous structure and should not be treated as the main workspace.

## Git And Network Notes

`git pull` or `git push` may fail here even when the git command itself is correct. The issue is sometimes the local network proxy mode rather than repository state.

If git network operations fail, check whether Clash is currently in `rule` mode. In some cases, git works only after switching Clash to `global` mode first.

Recommended workflow:

1. Try the git command normally first.
2. If the network path looks blocked, switch Clash from `rule` to `global`.
3. Retry the git command.
4. After the git operation succeeds, switch Clash back to `rule`.

Do not leave Clash in `global` mode longer than necessary.

When switching Clash mode for a git network operation, prefer doing the mode switch, git command, and mode restore in one atomic shell script with a `trap` that always restores `rule` on exit. This reduces the chance of leaving Clash in `global` mode if the git command fails or the turn is interrupted.

Preferred pattern:

1. Install a shell `trap` that restores Clash to `rule`.
2. Switch Clash to `global`.
3. Run `git push` or `git pull`.
4. Let the `trap` restore `rule` automatically on exit.

When mentioning this behavior to the user or another agent, be explicit that the problem is often "git traffic pathing through Clash" rather than git itself being broken.

Still check the staged set before committing:

1. Run `git status --short`.
2. Run `git diff --cached --name-only`.
3. Confirm that every staged file belongs to the current task.

## Editing Guidance

- Prefer editing article files under `posts/` unless the user explicitly asks for changes elsewhere.
- Treat untracked or modified files outside the current task as user work unless clearly told otherwise.
- When using `projects/` as reference material, align article claims with the actual code structure before editing prose.
