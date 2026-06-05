# Replicating: Percepta `transformer-vm` — "Can LLMs Be Computers?"

**Target:** [Percepta-Core/transformer-vm](https://github.com/Percepta-Core/transformer-vm)
· Blog: ["Can LLMs Be Computers?"](https://www.percepta.ai/blog/can-llms-be-computers)
· [Medium write-up](https://medium.com/data-science-collective/i-built-a-tiny-computer-inside-a-transformer-e3000a0019b3)
**No arXiv** — this work is published as code + blog, not a paper.

> Note: this repo was scaffolded against the arXiv paper *"Neural Computers"*
> (arXiv:2604.06425) because the scaffolder requires an arXiv ID. That paper is
> a conceptual cousin and is kept under `replication_target/source/` as
> **related work only** — it is **not** what we are replicating.

## What we are replicating

A **standard softmax-ReGLU transformer whose weights are computed analytically
(not trained)** that **correctly simulates a WebAssembly virtual machine on
arbitrary programs**. It encodes 35 WASM opcodes through a computation-graph DSL,
uses the residual stream as machine memory, and an O(log n) "hull" KV-cache
instead of O(n) softmax attention. Two modes: a *universal interpreter* (program
as input) and the *First Futamura projection* (program baked into weights).
Headline claims: correct VM simulation on arbitrary programs, ~30K tokens/sec,
and real algorithms (Sudoku ~900K tokens, fibonacci, collatz). Full source list +
claims: [`notes/sources.md`](./notes/sources.md).

## Replication status

In progress (recipe-first). The repo ships its own reproduction recipe
(`uv run wasm-run`), so the path is: run the authors' recipe, then verify its
output against the blog's headline claims. The concrete step queue is in
[`queue.md`](./queue.md); methodology in [`SKILL.md`](./SKILL.md).

## What this repo produces

Three compounding artifacts:

1. **The replication** — runnable code under `src/` + `scripts/run.py`.
2. **The legibility layer** — `FINDINGS.md`, published as a GitHub Pages
   site with a transportable PDF report (built by GitHub Actions).
3. **`SKILL.md`** — a reusable, agent-executable replication methodology.

## Layout

- `replication_target/` — the target and everything pulled about it:
  - `transformer-vm/` — the authors' code (git **submodule**); this is the
    real target and ships the reproduction recipe.
  - `source/` + `paper.pdf` — the arXiv "Neural Computers" e-print, kept as
    **related work only** (gitignored archive; committed `source/`).
- `notes/sources.md` — what we're replicating, all source links, and the claims.
- `data_lake/` — other downloaded/supplied material (NOT the paper).
- `src/` — your reimplementation. `scripts/run.py` — CI entry point.
- `results/` — metrics JSON (gitignored). `FINDINGS.md` — the report.
- `paper.json` — frozen metadata pulled from the arXiv API.
- `.github/workflows/` — `pages.yml` (site + PDF), `package.yml` (ZIP).

## Deliverables (GitHub Actions)

To publish, **make this repo public** and set **Settings -> Pages -> Source:
GitHub Actions**. Then `pages.yml` deploys the findings site + PDF report and
`package.yml` builds a downloadable ZIP replication package. Site shape
inspiration: http://latent-space.emmaleonhart.com/
