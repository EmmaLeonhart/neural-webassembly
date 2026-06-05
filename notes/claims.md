# Claims to reproduce + evidence

Target: Percepta `transformer-vm`. Source of claims: the repo README + the blog
"Can LLMs Be Computers?". See `notes/sources.md` for links.

## Headline claims

| # | Claim | How verified | Status |
|---|-------|--------------|--------|
| C1 | A standard softmax-ReGLU transformer with **analytically-computed (untrained) weights** correctly simulates a WebAssembly VM on arbitrary programs | `wasm-run`: transformer output must match the reference WASM trace token-for-token | pending (wasm-run building weights) |
| C2 | **35 WASM opcodes** encoded through a computation-graph DSL | opcode list in README; programs use them; reference traces execute | confirmed (compiles + runs; see C5) |
| C3 | **~30K tokens/second** inference (C++ engine) | `wasm-run` C++ engine throughput on the example programs | pending |
| C4 | Real algorithms run: **Sudoku (~900K tokens)**, fibonacci, collatz, Hungarian | reference traces + transformer match | partly confirmed (refs below) |
| C5 | Graph evaluator (exact arithmetic, no weights) matches reference | `wasm-eval` PASS per program | addition PASS; rest slow under brute-force (see hull note) |
| C6 | First Futamura projection: program baked into weights | `wasm-specialize` + run specialized model | pending |
| C7 | O(log n) **hull** KV cache vs O(n) softmax | hull extension / C++ CHT cache | C++ engine has it; Python hull ext needs `python3-dev` (see gap) |

## Reference outputs (WASM executed directly — validates VM semantics)

From `wasm-reference` (generated during the first `wasm-eval` run). These are the
ground-truth I/O traces the analytic transformer must reproduce:

| Program | Ref tokens | Output |
|---------|-----------:|--------|
| hello | 702 | `Hello World!\n` |
| addition | 3,425 | `19134\n` |
| fibonacci | 4,102 | `55\n` |
| collatz (n=7) | 40,522 | `7 22 11 34 17 52 26 13 40 20 10 5 16 8 4 2 1\n` |
| min_cost_matching | 162,539 | Hungarian algorithm, `optimal cost: 9` (full reasoning trace) |
| sudoku | 1,017,945 | puzzle solved: `534678912...345286179`, `clues: 30`, `propagated: 81/81`, `solved!` |

Note the sudoku trace is **~1.0M tokens**, consistent with the README's "~900K
tokens" headline (same order of magnitude; exact count depends on the puzzle).

The graph evaluator (`wasm-eval`, exact arithmetic, no transformer weights) is the
correctness oracle: `addition: PASS` confirmed. Larger programs are slow under
the brute-force fallback — see the gap below.

## Gaps / divergences observed

- **Hull Python extension did not build**: `hull_ext.cpp` needs `Python.h`
  (`python3-dev` / `python3.12-dev`), which was not installed, so `wasm-eval`
  fell back to brute-force O(n) attention (~30 tok/s) — impractical for the
  big programs. The **C++ inference engine** (`wasm-run`) embeds the CHT hull
  cache as a header (`hull2d_cht.h`) and does NOT need `Python.h`, so it is the
  path used to verify the throughput/scale claims. Installing `python3-dev`
  would let `wasm-eval --hull` and the Python inference path run fast too.
