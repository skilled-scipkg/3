# mumax3 source map: Run Workflows and Output

Use this only after `doc_map.md`.

## Fast queries
- `rg -n "RunWhile|RelaxTorqueThreshold|MinimizerStop|TableAutoSave|AutoSave|AutoSnapshot|LoadFile" engine`
- `rg -n "FilenameFormat|SnapshotFormat|output directory|TableAdd|TableSave" engine`

## Function-level entry points
- `engine/run.go`: `Run`, `Steps`, `RunWhile`, and output triggering.
- `engine/relax.go`: `Relax`, torque thresholds, and non-advancing time behavior.
- `engine/minimizer.go`: `Minimize`, sampled `dm` convergence checks, and `MinimizerStop`.
- `engine/autosave.go`: autosave scheduling and `DoOutput()`.
- `engine/save.go`: `Save`, `SaveAs`, `Snapshot`, output formats, and filenames.
- `engine/table.go`: table schema creation, auto-save, and `TableAddVar`.
- `engine/util.go`: `LoadFile(...)` for restart inputs and imported fields.
- `engine/od.go`: output-directory initialization and `-o` behavior.

## High-value behavior checks
- `mumax3 -vet test/runwhile.mx3`
- `mumax3 -vet test/minimizer.mx3`
- `mumax3 -vet test/outputformat.mx3`
- `mumax3 -vet test/snapshot.mx3`
