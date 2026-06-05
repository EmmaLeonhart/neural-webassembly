# replicating-pertepta — Work Queue

**This file is a queue of concrete, executable steps, not a state snapshot.**
Finished work lives in `devlog.md` (dated entries) and `git log`;
longer-horizon items live in `todo.md`. **When an item is done, delete it
from this file AND append a dated entry to `devlog.md` in the same commit,
then push.** No checkmarks, no status indicators in place.

---

## Target (READ FIRST)

We are **not** replicating the arXiv paper "Neural Computers" (2604.06425) — the
scaffolder downloaded it only because the flow expects an arXiv ID, and it is kept
as *related work only*. The **real target** is Percepta's **`transformer-vm`**: a
standard softmax-ReGLU transformer with **analytically-computed (untrained)
weights** that **correctly simulates a WebAssembly VM on arbitrary programs**. It
has no arXiv — only a GitHub repo (which ships the reproduction recipe), a blog
post, a LinkedIn post, and a Medium write-up. Full details + claims live in
`notes/sources.md`. Remote name requested: **`replicating-pertepta`**.

This is a **recipe-first** replication: the repo ships `uv run wasm-run`. Run it
first, then verify its output against the blog's headline claims.

---

## Active

5. **Finish the light path.** `uv run wasm-eval` already compiled all 6 programs,
   generated correct reference traces, and gave `addition: PASS`. Remaining:
   (a) ask the user to `sudo apt install -y python3-dev` so the `hull_ext` Python
   extension builds (currently falls back to slow brute-force, impractical for the
   big programs); (b) re-run `uv run wasm-eval` with the hull extension to PASS the
   larger programs; (c) `uv run pytest -m "not slow"`. Capture to `results/`. Commit.

6. **Run the full recipe**: `uv run wasm-run`. Capture stdout, example-program
   outputs, and any tokens/sec into `results/`. Commit.

7. **Verify headline claims** against the blog (`notes/claims.md`): analytic
   weights correctly simulate the WASM VM; example programs (collatz, fibonacci,
   sudoku) produce correct outputs; throughput order-of-magnitude (~30K tok/s);
   First Futamura projection (`wasm-specialize`) works. Note what reproduced and
   what didn't. Commit.

8. **`scripts/run.py`** — wrap the verified recipe steps as the CI entry point;
   emit metrics JSON into `results/`. Commit.

9. **Write `FINDINGS.md`**: reproduced vs claimed (table); what the recipe covered
   vs what we filled; gaps/divergences (esp. platform/toolchain limits). Commit
   and push.

10. **Publish and finish.** Set the verdict in `paper.json` `status`
    (`replicated` / `failed` / `insufficient-hardware`). Confirm
    `.github/workflows/pages.yml` + `package.yml` run green; Settings → Pages →
    Source: GitHub Actions. Keep `SKILL.md` truthful. Stop / hand back when
    `FINDINGS.md` reports at least one headline claim reproduced, `scripts/run.py`
    runs from a clean clone (or documents the un-automatable step), the repo is
    public + pushed, and Pages is green.

---

## Pointers
- What we're replicating + all source links + claims: `notes/sources.md`.
- Methodology / definition of done: `SKILL.md`.
- Long-horizon items: `todo.md`.
- Completed work + milestones (chronological): `devlog.md`.
