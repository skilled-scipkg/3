---
name: mumax3-tex
description: This skill should be used when users ask about run, restart, relax, minimize, autosave, tables, and output handling in mumax3.
---

# mumax3: Run Workflows and Output

## High-Signal Playbook

**Route conditions**
- Stay here for `Run`, `RunWhile`, `Relax`, `Minimize`, restart files, autosave, tables, snapshots, and output validation.
- Use `mumax3-getting-started` for a first script or baseline example if the user is not yet past the quickstart stage.
- Use `mumax3-inputs-and-modeling` when the problem is really about mesh, geometry, regions, or custom excitations rather than run control.
- Use `mumax3-build-and-install` or `mumax3-cuda` when the simulation cannot start because the binary or GPU is unhealthy.
- Use `mumax3-parallel-hpc` for `-gpu`, queue mode, or `mumax3-server`.

**Triage questions**
- Is the target state a ground state, a hysteresis loop, or a time-dependent driven run?
- Is the initial magnetization random/high-energy, or already close to equilibrium?
- Do you need to restart from an OVF file or another saved state?
- What outputs matter: OVF fields, tables, snapshots, region averages, or all of them?
- Are excitations such as field, current, or temperature active during the relaxation phase?
- What stopping rule are you using: fixed runtime, `RunWhile(...)`, `RelaxTorqueThreshold`, or `MinimizerStop`?

**Canonical workflow**
- Define mesh, geometry, material parameters, and initial `m` first.
- Use `Relax()` for rough or random high-energy states; it disables precession and does not advance `t` (`doc/static/api310.html`, `engine/relax.go`).
- Use `Minimize()` for near-equilibrium continuation, especially hysteresis loops (`doc/templates/examples-template.html`, `doc/static/api310.html`).
- Schedule `TableAdd`, `TableAutoSave`, `AutoSave`, or `AutoSnapshot` before the main run, then apply the field or current drive.
- Run with `Run(...)`, `Steps(...)`, or `RunWhile(...)`, and validate on `table.txt` plus a small number of representative saved fields.
- Restart by loading a saved state with `m.LoadFile(...)` when you need continuation from a previous output.

**Minimal working example**
- Docs: `doc/templates/examples-template.html`, `doc/templates/api-template.html`, `doc/static/api310.html`.

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

```mx3
MinimizerStop = 1e-6
TableAdd(B_ext)
m.LoadFile("relaxed.ovf")
Minimize()
TableSave()
```

**Pitfalls / fixes**
- `Relax()` assumes excitations are off, disables precession, and does not advance simulation time (`engine/relax.go`).
- `Minimize()` is faster but less robust from a random initial state; the docs recommend `Relax()` first for high-energy starts.
- `TableAdd(...)` must happen before the first table output; after initialization the table schema is fixed (`engine/table.go`).
- `TableAutoSave(...)`, `AutoSave(...)`, and `AutoSnapshot(...)` are schedules, not convergence checks; still validate cadence in `table.txt` and the output directory.
- Use `RunWhile(...)` when the stop condition is physical, such as a switching event, rather than purely elapsed time.
- `doc/tex/mumax3.tex` is an equation summary, not the operational workflow guide.

**Convergence / validation checks**
- Tighten the mesh or enlarge the box if switching behavior moves under refinement.
- Track `m.average()`, added table columns, and a few saved OVFs together; do not trust a single observable.
- Use `RelaxTorqueThreshold` or `MinimizerStop` deliberately and record the chosen values with the run.
- Re-run short segments from a saved state to confirm restart consistency when using `m.LoadFile(...)`.

## Scope
- Standard run, restart, and output workflow.
- `Relax` versus `Minimize`.
- Autosave, tables, snapshots, and stop conditions.

## Primary documentation references
- `doc/templates/examples-template.html`
- `doc/templates/api-template.html`
- `doc/static/api310.html`
- `doc/tex/mumax3.tex`
- Full topic inventory: `references/doc_map.md`

## Minimal tests and examples
- `test/runwhile.mx3`
- `test/minimizer.mx3`
- `test/table.mx3`
- `test/outputformat.mx3`
- `test/snapshot.mx3`

## Source entry points for unresolved issues
- `engine/run.go`: `Run`, `Steps`, `RunWhile`, and output cadence.
- `engine/relax.go`: actual relax stopping logic and `RelaxTorqueThreshold`.
- `engine/minimizer.go`: conjugate-gradient minimizer behavior, `MinimizerStop`, and `MinimizerSamples`.
- `engine/autosave.go`: `AutoSave`, `AutoSnapshot`, and `DoOutput()`.
- `engine/save.go`: `Save`, `SaveAs`, snapshots, output formats, and filenames.
- `engine/table.go`: `TableAdd`, `TableSave`, `TableAutoSave`, and table-header initialization.
- `engine/util.go`: `LoadFile(...)` for restart inputs.
- `engine/od.go`: output-directory initialization and `-o` behavior.
- Ranked fallback map: `references/source_map.md`
