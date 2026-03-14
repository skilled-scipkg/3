# mumax3 source map: Getting Started

Use this only after `doc_map.md`.

## Fast queries
- `rg -n "runFileAndServe|runScript|goServeGUI|Flag_port|Flag_gpu|Flag_od" cmd/mumax3 engine`
- `rg -n "RunWhile|DoOutput|TableAutoSave|AutoSave|CompileFile|EvalFile" cmd engine script`

## Function-level entry points
- `cmd/mumax3/main.go`: CLI entry point, `-test`, `-vet`, GUI startup, and dispatch into single-file or queue mode.
- `engine/gofiles.go`: shared runtime flags and Go-input bootstrap behavior.
- `engine/run.go`: common run loop and output trigger path.
- `engine/autosave.go`: periodic save scheduling.
- `engine/table.go`: default table setup and table output lifecycle.
- `engine/html.go`: live GUI page generation and controls.
- `script/compile.go`: parser/compiler path for `.mx3` input.
- `script/world.go`: symbol registration and case-insensitive identifier resolution.

## High-value behavior checks
- `mumax3 -vet test/standardproblem4.mx3`
- `mumax3 test/standardproblem4.mx3`
- `mumax3 -vet test/custom_std4.mx3`
