# mumax3 source map: Index

Use this only after `doc_map.md` when the request spans several skills.

## Fast queries
- `rg -n "flag_|RunWhile|Relax|Minimize|SetGridSize|SetGeom|AddFieldTerm|mumax3-server|make html" cmd engine script cuda doc`
- `rg -n "FunctionFromDatafile|LoadFile|TableAutoSave|AutoSave|Queue|GiveJob|fatbinLoad" engine cmd cuda script`

## Cross-cutting entry points
- `cmd/mumax3/main.go`: top-level CLI, `-test`, `-vet`, GUI startup, and single-file versus queue dispatch.
- `engine/gofiles.go`: shared runtime flags such as `-gpu`, `-http`, `-cache`, and output-directory handling for Go-driven runs.
- `engine/run.go`: common run loop, `Run`, `Steps`, `RunWhile`, and output triggering.
- `engine/mesh.go`: mesh/PBC setup and resize side effects.
- `engine/relax.go`: robust equilibrium path for high-energy initial states.
- `engine/minimizer.go`: faster near-equilibrium minimization path and convergence thresholding.
- `cuda/init.go`: CUDA context startup and early PTX validation.
- `cmd/mumax3-server/doc.go`: cluster model, directory layout, and operational flags.
- `doc/make.go`: docs/examples generation pipeline and build outputs.

## High-value behavior checks
- `mumax3 -test`
- `mumax3 -vet test/standardproblem4.mx3`
- `mumax3 -vet test/geom2d.mx3`
- `cd doc && make html`
