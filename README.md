# Market Systems Writing

This repository is a publishing workspace for long-form writing on quantitative trading infrastructure, execution systems, and real-time market connectivity.

The emphasis is not on tutorials or implementation guides, but on how production trading systems behave under uncertainty.

## Project Purpose

This repository is primarily a writing workspace, not a single runnable trading system.

Its main purpose is to develop, revise, and archive long-form articles about real trading infrastructure, execution behaviour, and production system design.

The `projects/` directory contains snapshots of real code from related trading projects. These codebases are included as reference material for writing: they provide concrete architecture, naming, implementation patterns, and operational context that can be cited or studied while drafting articles.

They should be understood as supporting source material rather than the main object of development in this repository. In other words:

- `drafts/`, `published/`, and `editorial/` are the primary working areas.
- `projects/` exists to ground the writing in real systems and real code.
- The repository should be interpreted as a research and writing environment built around real trading code references.

## Repository Structure

```text
.
├── README.md
├── cover.png
├── publication-archive.md
├── drafts/
├── editorial/
├── projects/
└── published/
```

### `editorial/`

Editorial control documents that define the intellectual identity of the project:

- `style.md` for structural writing guidance
- `hard-signature.md` for non-negotiable principles
- `soft-signature.md` for recurring voice patterns
- `topic-seed.md` for future article directions

### `drafts/`

Work-in-progress articles that are not yet published.

Use this folder for:

- early article drafts
- partial rewrites
- exploratory notes that are already article-shaped

### `published/`

Finalized articles that are ready to archive as published pieces.

This folder is the stable record of finished writing.

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
2. Draft the article in `drafts/`.
3. Check tone and structure against the documents in `editorial/`.
4. Move finalized pieces into `published/`.
5. Update `publication-archive.md`.

## Intent

This repository serves as:

- a long-term archive of system observations
- a structural foundation for future publications
- an evolving investigation into trading system reality

No attempt is made to provide complete solutions.

System understanding is treated as partial, empirical, and continuously evolving.
