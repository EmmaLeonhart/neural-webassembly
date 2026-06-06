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

## Active — learn new CPU operations on the scaffold

Replication complete (REPLICATED, 6/6). Research thread: train *new CPU operations*
on the differentiable scaffold, then crystallize to exact weights — the
constructed+trained hybrid toward a Completely Neural Computer. Design + findings in
`notes/experiment_learned_ops.md`. **E0–E2 DONE** (see `devlog.md`):
- E0 — grad-enabled forward reproduces exact execution.
- E1 — learned **unsigned saturating add** to 100% exact (value-output). AND is a
  documented negative result (spectral bias; no runtime bit decomposition).
- E2 — crystallized sat_add to one exact ReGLU neuron; learned == crystallized on all
  65 536 pairs.

Remaining:
- **E3 — integrate as a native opcode.** Wire crystallized `sat_add_u` into the WASM
  interpreter as a real opcode (gated by `is_op`), build weights, and pass a program
  that uses it end-to-end through the transformer vs a reference trace (G2). This is
  the bigger build (touches `wasm/interpreter.py`, opcode tables, `compile_wasm.py`).
- **E4 — more learned ops / write-up.** Signed saturating add/sub, min/max, etc.;
  consolidate the negative (AND) + positive (sat_add) results into a short findings
  note. (sat_add — the original E4 north-star op — is already learned op-local.)
- **Yantra OS integration** — the forward goal; design in
  `notes/yantra_integration.md` (trap-and-resume; P0–P6 roadmap).

## Optional / nice-to-have (replication already succeeds without these)

- **Hull Python path:** `sudo apt install -y python3-dev`, then `uv run wasm-eval`
  (hull) and `uv run pytest -m "not slow"` should pass fast. CI already installs
  `python3-dev`. (Quantify hull vs `--nohull` timings for the O(log n) claim.)

---

## Pointers
- What we're replicating + all source links + claims: `notes/sources.md`.
- Methodology / definition of done: `SKILL.md`.
- Long-horizon items: `todo.md`.
- Completed work + milestones (chronological): `devlog.md`.
