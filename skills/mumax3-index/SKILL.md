---
name: mumax3-index
description: This skill should be used when users ask how to use mumax3 and the correct documentation-first skill must be selected before any deeper source inspection.
---

# mumax3 Skills Index

## Start here
- `mumax3-getting-started`: first successful run, quick syntax refresher, Standard Problem #4 style onboarding, local GUI basics.
- `mumax3-inputs-and-modeling`: mesh design, geometry, regions, material parameters, excitations, imported data, and custom field terms.
- `mumax3-tex`: run control, `Relax()` versus `Minimize()`, restart files, autosave, tables, snapshots, and convergence checks.
- `mumax3-build-and-install`: prebuilt downloads, Linux and Windows builds, CUDA/Go/compiler prerequisites, and build-failure triage.
- `mumax3-cuda`: compute capability, `CUDA_CC`, CUFFT/CURAND, `cuda.h` or library failures, and low-level GPU initialization.
- `mumax3-parallel-hpc`: `-gpu`, local queue mode, `mumax3-server`, peer scanning, and network execution.
- `mumax3-templates`: website/examples/API page generation under `doc/`.
- `mumax3-advanced-topics`: legacy `api39c`, parser/source internals, tiny website notes, and old benchmark scraps.

## Prefer these repo roots first
- `README.md`: install, build, and runtime overview.
- `doc/templates/`: current user-facing examples, API reference, download page, and home page templates.
- `doc/static/`: static legacy API/download pages that still answer version-specific questions.
- `test/`: runnable regression-backed `.mx3` inputs for minimal examples and behavior checks.
- `cmd/`: CLI behavior, queue mode, converters, and `mumax3-server`.
- `engine/`: solver, modeling, output, GUI, restart, and run-control semantics.
- `script/`: parser, interpreter, `source(...)`, and symbol resolution details.
- `cuda/`: device initialization, compute-capability coverage, CUFFT/CURAND, and PTX loading.

## Escalation order
- Use this skill's `references/doc_map.md` when the request is ambiguous.
- Open the routed skill's `references/doc_map.md` before reading code.
- Only then inspect the routed skill's `references/source_map.md` and the pointed implementation files.
- If the request spans multiple areas, use this skill's `references/source_map.md` for the cross-cutting entry points that connect CLI, engine, CUDA, and docs generation.
