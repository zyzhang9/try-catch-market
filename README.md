# Market Systems Writing

This repository is a publishing workspace for long-form writing on quantitative trading infrastructure, execution systems, and real-time market connectivity.

The emphasis is not on tutorials or implementation guides, but on how production trading systems behave under uncertainty.

The current publication identity is:

- publication name: `漂移在交易场`
- recurring observer persona: `Drift`

Drift is used as a restrained observing voice rather than the main subject of the project.

## Project Purpose

This repository is primarily a writing workspace, not a single runnable trading system.

Its main purpose is to develop, revise, and archive long-form articles about real trading infrastructure, execution behaviour, and production system design.

The `projects/` directory contains snapshots of real code from related trading projects. These codebases are included as reference material for writing: they provide concrete architecture, naming, implementation patterns, and operational context that can be cited or studied while drafting articles.

They should be understood as supporting source material rather than the main object of development in this repository. In other words:

- `posts/` and `editorial/` are the primary working areas.
- `projects/` exists to ground the writing in real systems and real code.
- The repository should be interpreted as a research and writing environment built around real trading code references.

The `old/` directory is only a temporary migration holding area for legacy articles from the previous structure. It is not part of the long-term workflow and can be deleted after the transition.

## Repository Structure

```text
.
├── README.md
├── cover.png
├── editorial/
├── old/
├── posts/
├── projects/
└── publication-archive.md
```

### `editorial/`

Editorial control documents that define the intellectual identity of the project:

- `style.md` for structural writing guidance
- `hard-signature.md` for non-negotiable principles
- `soft-signature.md` for recurring voice patterns
- `topic-seed.md` for future article directions

### `posts/`

Primary writing workspace.

Each article lives in its own folder, typically with:

- `diary.md` for the user's source notes, facts, fragments, and pasted materials
- `post.md` for the article written from those materials

This separates source reality from publishable expression.

### `old/`

Temporary migration area for legacy content from the previous `drafts/` and `published/` structure.

Do not treat this directory as part of the long-term workflow.

### `projects/`

Reference implementations and historical project snapshots related to the themes discussed in the articles.

These are real codebases kept here for context and source grounding. They are not the primary publishing output of this repository.

## Current Focus

Recurring themes include:

- market connectivity and stochastic latency
- execution path dependence
- real-time system instability
- infrastructure constraints in trading environments
- engineering decisions under market pressure

The writing documents observed system behaviour rather than prescriptive design frameworks.

## Working Flow

1. Start from a real incident, anomaly, deployment, or debugging event.
2. Create or update a dossier in `posts/<topic>/`.
3. Record source material in `diary.md`.
4. Write the article in `post.md`.
5. Check tone and structure against the documents in `editorial/`.
6. Update `publication-archive.md` when needed.

## Intent

This repository serves as:

- a long-term archive of system observations
- a structural foundation for future publications
- an evolving investigation into trading system reality

No attempt is made to provide complete solutions.

System understanding is treated as partial, empirical, and continuously evolving.
