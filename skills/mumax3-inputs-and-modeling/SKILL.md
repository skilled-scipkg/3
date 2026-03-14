---
name: mumax3-inputs-and-modeling
description: This skill should be used when users ask about mesh design, geometry, regions, material parameters, excitations, imported data, or custom field terms in mumax3 scripts.
---

# mumax3: Inputs and Modeling

## High-Signal Playbook

**Route conditions**
- Stay here for mesh and cell size, PBC, shapes, `SetGeom`, region definitions, region-wise parameters, imported data, time-dependent excitations, `FunctionFromDatafile`, `LoadFile`, `AddFieldTerm`, and `AddEdensTerm`.
- Use `mumax3-getting-started` for the smallest possible first run when modeling details are still secondary.
- Use `mumax3-tex` once the model is defined and the question becomes about run control, restart, autosave, or convergence.
- Use `mumax3-cuda` or `mumax3-build-and-install` if the real blocker is GPU or binary health.
- Use `mumax3-advanced-topics` for legacy parser or `source(...)` questions.

**Triage questions**
- What dimensionality and physical size do you need, and what cell size is acceptable relative to exchange length or feature size?
- Is the sample the full box, a boolean-combined shape, an image mask, or imported geometry from OVF?
- Do you need multiple materials or regions, and which parameters vary by region?
- Is the excitation uniform, region-wise, mask-based, file-driven, or a custom field expression?
- Do you need imported initial states, custom energy terms, or current-driven terms?
- What is the cheapest validation output that proves the geometry and parameter assignment are correct?

**Canonical workflow**
- Set mesh, cell size, and any PBC first; MuMax3 requires the mesh before shapes, regions, or magnetization setup.
- Define geometry and immediately validate it with `Save(geom)` or `Snapshot(geom)` before running expensive dynamics.
- Define regions and save `regions` before adding complicated region-wise material overrides.
- Set material parameters and initial `m`, then add excitations or custom field terms.
- For file-driven inputs, keep the first run short and validate that the driven quantity actually changes as expected in `table.txt`.
- Only move on to long dynamics after geometry, regions, and one or two averaged observables look correct.

**Minimal working example**
- Docs: `doc/templates/api-template.html`, `doc/templates/examples-template.html`, `test/regions.mx3`, `test/functionfromfile.mx3`.

```mx3
SetGridSize(256, 128, 1)
SetCellSize(2e-9, 2e-9, 1e-9)

disk  := Circle(300e-9)
notch := Rect(60e-9, 120e-9).Transl(100e-9, 0, 0)
SetGeom(disk.Sub(notch))
Save(geom)

DefRegion(1, XRange(-inf, 0))
DefRegion(2, XRange(0, inf))
Save(regions)

Msat  = 800e3
Aex   = 13e-12
alpha = 0.02
Ku1.SetRegion(1, 400e3)
Ku1.SetRegion(2, 0)
anisU = vector(1, 0, 0)

m = Uniform(1, .1, 0)
TableAdd(m.Region(1))
TableAdd(m.Region(2))
TableAutoSave(10e-12)
Run(50e-12)
```

```mx3
drive := FunctionFromDatafile("test/functionfromfile.csv", 0, 1, "linear")
B_ext = vector(0, 0, drive(t))
Run(0.2e-9)
```

**Pitfalls / fixes**
- The mesh must exist before `SetGeom`, `DefRegion`, `m.Set(...)`, or most region-wise assignments (`engine/mesh.go`).
- Resizing the mesh removes extra excitation masks; `B_ext.Add(...)` and `J.Add(...)` terms must be re-added after resize (`engine/mesh.go`, `engine/excitation.go`).
- `DefRegion` is last-write-wins; always save `regions` if multiple shapes overlap.
- `FunctionFromDatafile` requires strictly increasing x-values and will fail on unsorted data (`engine/functionfromfile.go`).
- `AddFieldTerm(...)` changes `B_eff`, but `Relax()` and `Minimize()` only get a consistent energy landscape if you also add the matching `AddEdensTerm(...)` (`engine/customfield.go`).
- Use `Save(geom)` and `Save(regions)` before long runs; they are the fastest checks for modeling mistakes.

**Validation checkpoints**
- `mumax3 -vet yourfile.mx3` should pass before any expensive run.
- Run a short simulation and confirm that `geom`, `regions`, and `table.txt` reflect the intended model.
- Compare region averages with `m.Region(n)` or saved region outputs before trusting full-field dynamics.
- For imported geometry or data, validate a few specific cells or averages the way `test/initgeomfromovf.mx3` and `test/loadfile.mx3` do.

## Scope
- Mesh, cell size, and PBC choices.
- Geometry, regions, and material parameter assignment.
- Imported data, custom excitations, and custom field or energy terms.

## Primary documentation references
- `doc/templates/api-template.html`
- `doc/templates/examples-template.html`
- `doc/static/api310.html`
- Full topic inventory: `references/doc_map.md`

## Minimal tests and examples
- `test/geom2d.mx3`
- `test/regions.mx3`
- `test/functionfromfile.mx3`
- `test/loadfile.mx3`
- `test/initgeomfromovf.mx3`
- `test/custom_anisotropy.mx3`
- `test/custom_exchange.mx3`

## Source entry points for unresolved issues
- `engine/mesh.go`: `SetGridSize`, `SetCellSize`, `SetMesh`, `SetPBC`, and resize side effects.
- `engine/geom.go`: `SetGeom`, `EdgeSmooth`, and `ext_InitGeomFromOVF`.
- `engine/shape.go`: primitive shapes, boolean composition, transforms, and `ImageShape`.
- `engine/regions.go`: `DefRegion`, `RedefRegion`, per-cell assignment, and region reload behavior.
- `engine/parameter.go`: region-wise material parameters and time-dependent parameter updates.
- `engine/magnetization.go`: initial `m`, region-specific magnetization, and `LoadFile`.
- `engine/excitation.go`: vector excitations such as `B_ext` and `J`, including mask-based extra terms.
- `engine/scalar_excitation.go`: scalar excitation plumbing when the driven quantity is not vector-valued.
- `engine/functionfromfile.go`: CSV-driven interpolation for time-dependent inputs.
- `engine/customfield.go`: `AddFieldTerm`, `AddEdensTerm`, quantity algebra, and custom energies.
- Ranked fallback map: `references/source_map.md`
