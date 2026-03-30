# OpenWrite Render Guide

This document defines the minimal formatting adjustments allowed when an
approved `post.md` is prepared for OpenWrite.

The purpose is not to rewrite the article or create a second article file.
It is to keep `post.md` as the single source of truth while allowing a narrow
set of platform-facing formatting adjustments.

---

## Position

`post.md` remains the source of truth for the article itself.

If an idea, claim, paragraph order, or conclusion needs to change,
change `post.md` first.

OpenWrite preparation should usually happen without creating a separate file.

---

## Core Principle

OpenWrite adaptation should preserve:

- argument order
- factual content
- tone
- uncertainty level
- Drift usage

OpenWrite adaptation may adjust:

- separator style
- inline emphasis style such as backticks, italics, or bold
- code block style

The goal is:

Same article. Better platform fit.

---

## Allowed Transformations

Typical allowed changes include:

- replacing long visual separators such as `——` with Markdown separators such as `---`
- adjusting inline style such as `` `xxx` ``, `*xxx*`, or `_xxx_`
- adjusting fenced code block style without changing code meaning

These are render moves, not conceptual edits.

---

## Not Allowed

Do not use OpenWrite preparation to:

- add new claims not present in `post.md`
- turn the piece into a tutorial
- add listicles, summaries, or CTA-style endings
- make the tone more motivational, promotional, or persuasive
- expand Drift into a stronger narrative role
- replace uncertainty with stronger causal closure
- add decorative formatting that changes the article's character

If a change alters the article's intellectual position, it belongs in
`post.md`.

---

## Default Practice

Default OpenWrite preparation should usually follow this shape:

1. Preserve the original title.
2. Convert large section turns into clean separators.
3. Optionally adjust inline emphasis style.
4. Optionally adjust code block style.
5. Keep the closing Drift signature if it exists in `post.md`.

This should remain lightweight, and it usually does not require a second file.

---

## Publication Archive

`publication-archive.md` should follow the current repository structure.

For published articles:

- point to the canonical source article under `posts/<slug>/post.md`
- include the final public WeChat link when it exists

Avoid maintaining links to legacy `published/...` article paths when the
canonical source already lives under `posts/`.

The archive is a publishing index, not a second content store.

---

## Related Reading Footer

Published or near-published articles may include a lightweight related-reading
footer at the bottom of `post.md`.

Default shape:

1. Keep the main article ending intact.
2. Keep the Drift signature if present.
3. Add a simple separator.
4. Add a short `## 相关文章` block.
5. Prefer linking only to already published articles.

If a nearby article is planned but not yet published, it may be listed in a
low-key placeholder form such as plain text or `（待发布）`.

Keep this footer minimal:

- no summaries
- no CTA language
- no marketing phrasing
- no long reading lists

The goal is to make the publication feel like an accumulating微信公众号 series,
not a blog sidebar.

---

## Tone Requirements

Even after adaptation, the article should still feel:

- calm
- observational
- technically grounded
- non-didactic
- slightly restrained

If the OpenWrite version becomes more clickable but less credible,
the adaptation has gone too far.

---

## Workflow

Recommended workflow:

1. Finish and approve `post.md`.
2. Apply only narrow render-layer adjustments if the platform needs them.
3. Keep `post.md` unchanged unless a real article issue is discovered.

---

## Quick Test

Before publishing to OpenWrite, check:

- Does it still read like the same article?
- Did we avoid changing wording, structure, and argument flow?
- Did we preserve the original uncertainty and restraint?
- Are all conceptual changes absent or already reflected in `post.md`?

If the answer is yes, the render is likely good enough.
