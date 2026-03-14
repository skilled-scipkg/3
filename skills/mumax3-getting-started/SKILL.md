---
name: mumax3-getting-started
description: This skill should be used when users need a first successful mumax3 run, a quick syntax refresher, or a docs-first path from install to a minimal working .mx3 script.
---

# mumax3: Getting Started

## High-Signal Playbook

**Route conditions**
- Use `mumax3-build-and-install` if `mumax3 -test`, the NVIDIA driver, CUDA, or the compiler toolchain is not working yet (`README.md`, `doc/templates/download-template.html`).
- Use `mumax3-inputs-and-modeling` once the question becomes about mesh choice, geometry, regions, custom fields, or imported data (`doc/templates/api-template.html`, `test/geom2d.mx3`, `test/regions.mx3`).
- Use `mumax3-tex` for `Relax()`, `Minimize()`, restart files, autosave, table output, or convergence tuning (`doc/templates/examples-template.html`, `doc/static/api310.html`).
- Use `mumax3-parallel-hpc` for `-gpu`, multi-file queue mode, `mumax3-server`, or network execution (`cmd/mumax3/queue.go`, `cmd/mumax3-server/doc.go`).
- Use `mumax3-templates` for website/API/examples generation under `doc/`, not for simulation setup (`doc/README`, `doc/make.go`).
- Use `mumax3-advanced-topics` for legacy `api39c`, parser internals, or `source(...)`.

**Triage questions**
- Does `mumax3 -test` already succeed on the target GPU?
- Are you starting from a new `.mx3` file or repairing an existing one?
- Do you need an equilibrium state first, or immediate time dynamics?
- Do you already know the mesh and geometry, or should we start from Standard Problem #4?
- Do you need OVF fields, table output, GUI snapshots, or all three?
- Will this run locally on one GPU, across several local GPUs, or through `mumax3-server`?

**Canonical workflow**
- Verify the binary and GPU first, then start from the Standard Problem #4 pattern in `doc/templates/examples-template.html`.
- Set grid size, cell size, core material parameters, and initial magnetization before adding dynamics.
- Relax the starting state, save it, then schedule periodic outputs before applying the drive field.
- Run the script with `mumax3 myfile.mx3`; outputs land in `myfile.out`, and the live GUI is served on `http://localhost:35367` by default (`cmd/mumax3/main.go`).
- Use `table.txt` and `m.average()` as the first validation signal before digging into full OVF fields.

**Minimal working example**
- Docs: `doc/templates/examples-template.html`, `doc/static/api310.html`, `test/standardproblem4.mx3`.

```mx3
SetGridSize(128, 32, 1)
SetCellSize(500e-9/128, 125e-9/32, 3e-9)

Msat  = 800e3
Aex   = 13e-12
alpha = 0.02

m = Uniform(1, .1, 0)
Relax()
Save(m)

TableAutoSave(10e-12)
AutoSave(m, 100e-12)
B_ext = vector(-24.6e-3, 4.3e-3, 0)
Run(1e-9)
```

```bash
mumax3 -vet standardproblem4.mx3
mumax3 standardproblem4.mx3
```

**Pitfalls / fixes**
- `:=` declares a new variable; `=` updates an existing one (`doc/templates/api-template.html`, syntax section).
- The script language is case-insensitive, so `Msat`, `msat`, and `MSAT` are equivalent.
- Prefer power-of-two or small-prime grid dimensions for performance; `test/standardproblem4.mx3` and the Standard Problem #2 example encode that pattern.
- Large `SetPBC(...)` values increase demag initialization cost; start small and justify the extra images you add.
- The first useful check is usually `m.average()` or `table.txt`, not a large stack of OVF files.
- If the GUI does not start, inspect `cmd/mumax3/main.go` and `engine/html.go`.

**Convergence / validation checks**
- Refine the cell size if switching fields or average magnetization move noticeably.
- Relax before the driven run unless the initial state is already near equilibrium.
- Confirm that `table.txt` and periodic OVF output exist in the output directory; `engine/run.go` and `engine/autosave.go` drive that cadence.
- Compare `m.average()` against `test/standardproblem4.mx3` or `test/custom_std4.mx3` before scaling the problem up.

## Scope
- First successful local run.
- Quick syntax, mesh, and output orientation.
- Docs-first onboarding before deeper modeling, workflow, or HPC questions.

## Primary documentation references
- `README.md`
- `doc/templates/examples-template.html`
- `doc/templates/api-template.html`
- `doc/static/api310.html`
- Full topic inventory: `references/doc_map.md`

## Minimal tests and examples
- `test/standardproblem4.mx3`
- `test/custom_std4.mx3`
- `test/standardproblem4-3d-minimize.mx3`

## Source entry points for unresolved issues
- `cmd/mumax3/main.go`: single-file run, queue mode, GUI startup, `-test`, `-vet`, and interactive mode.
- `engine/gofiles.go`: shared runtime flags such as `-gpu`, `-http`, `-cache`, and output-directory overrides.
- `engine/run.go`: `Run`, `Steps`, `RunWhile`, and output cadence.
- `engine/html.go`: live GUI behavior and exposed controls.
- `script/compile.go`: parser/compiler error paths for inline script fragments.
- `script/world.go`: identifier resolution and top-level documentation registry.
- Ranked fallback map: `references/source_map.md`
