# replicating-neural-computers — Devlog

**This file is where "done" lives.** `queue.md` is delete-only: when a queue
item is finished, the item is **deleted from `queue.md`** and a dated entry
is **appended here**, in the same commit as the work, then pushed. Never
tick a box in place — a checked box left in `queue.md` is the failure mode
this file exists to prevent.

Also record releases (tag + a one-line note), notable milestones, and
anything else worth a chronological trail. Newest entries at the bottom.

This is the **same convention as the cleanvibe repo's own `devlog.md`** —
every cleanvibe-scaffolded project gets one for the same reason.

See `CLAUDE.md` § "Workflow Rules" and `queue.md`'s preamble.

---

## 2026-06-05 — Project scaffolded

Scaffolded with `cleanvibe new` (cleanvibe v1.13.1). Future entries
land here as queue items get deleted.

## 2026-06-05 — Retargeted to Percepta transformer-vm

The queue's "Important" note made clear the real target is **not** the arXiv
paper "Neural Computers" (2604.06425) the scaffolder downloaded, but Percepta's
**`transformer-vm`** — a transformer with analytically-computed (untrained)
weights that simulates a WebAssembly VM. It has no arXiv (GitHub + blog +
Medium only). Researched all four source links (data_lake was empty), recorded
the target, architecture, claims, and recipe in `notes/sources.md`. Rewrote
`queue.md` as a recipe-first plan, updated `README.md` and `paper.json`. The
arXiv paper is kept under `replication_target/source/` as related work only.

## 2026-06-05 — Went live + submodule added

- User gave explicit consent to run the third-party code locally and chose the
  remote name `replicating-transformer-vm`.
- Created the PUBLIC repo and pushed:
  https://github.com/EmmaLeonhart/replicating-transformer-vm
- Added `Percepta-Core/transformer-vm` as a git submodule at
  `replication_target/transformer-vm`, pinned at `6cfee30`.
- Toolchain decision: Windows lacks uv/clang/MSVC and the repo only documents
  macOS/Ubuntu builds, so we run inside the existing **WSL Ubuntu 24.04** distro
  (Python 3.12, 16 CPU / 15Gi RAM, internet OK). On Linux the C++ engine builds
  with plain `g++ -std=c++17 -O3` — no BLAS. Setup scripted in
  `scripts/wsl_setup.sh`; only the `apt install build-essential clang lld cmake`
  step needs sudo (handed to the user).
