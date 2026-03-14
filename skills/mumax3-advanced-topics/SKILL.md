---
name: mumax3-advanced-topics
description: This skill should be used for low-frequency or legacy MuMax3 topics that were collapsed from one-doc stubs: legacy API pages, parser internals, tiny website notes, and old benchmark scraps.
---

# mumax3: Advanced Topics

## Scope
- Legacy `api39c` reference use.
- Parser/interpreter internals and `source(...)`.
- Website-build notes that are too small for a dedicated skill.
- Old benchmark/comparison scraps that are not operational HPC guidance.

## Route the request
- Use `mumax3-getting-started` for current syntax and first-run questions.
- Use `mumax3-inputs-and-modeling` for current mesh, geometry, material, or excitation API usage.
- Use `mumax3-tex` for run, restart, relax/minimize, and output workflows.
- Use `mumax3-build-and-install` for current install/build guidance.
- Use `mumax3-parallel-hpc` for GPU selection, queue mode, or `mumax3-server`.
- Stay here only for legacy or implementation-heavy edge cases where the main skills are too broad.

## Primary documentation references
- `doc/static/api39c.html`
- `doc/README`
- `script/test.txt`
- `bench/oommf4M.txt`
- Full topic inventory: `references/doc_map.md`

## Source entry points for unresolved issues
- `script/compile.go`: parser/compiler error mapping.
- `script/world.go`: top-level symbol registration and case-insensitive lookup.
- `script/source.go`: `source("file")` behavior.
- `script/exec.go`: direct compile-and-execute helpers used by the interpreter layer.
- `cmd/mumax3-script/main.go`: toy interpreter path.
- `doc/make.go`: website-generator details that the tiny `doc/README` does not cover.
- `doc/apigen.go`: legacy/current API page generation behavior.
- `bench/bench.mx3`: simple benchmark script behind the bench topic.
- `cmd/mumax3-plot/main.go`: table-plot helper entry point.
- Ranked fallback map: `references/source_map.md`
