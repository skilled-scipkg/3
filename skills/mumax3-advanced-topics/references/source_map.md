# mumax3 source map: Advanced Topics

Use this only after `doc_map.md`.

## Fast queries
- `rg -n "source\\(|CompileExpr|Compile\\(|Exec\\(|document\\(|resolve\\(" script`
- `rg -n "api39c|buildAPI|bench|gnuplot" doc cmd bench`

## Function-level entry points
- `script/compile.go`: parser/compiler entry points and error recovery boundary.
- `script/source.go`: `source("...")` file inclusion semantics.
- `script/world.go`: symbol declaration, scope lookup, and documentation registry.
- `script/error.go`: compile error formatting and source-position reporting.
- `script/exec.go`: direct compile-and-execute helpers.
- `cmd/mumax3-script/main.go`: minimal interpreter for isolated parser behavior.
- `doc/make.go`: legacy doc generation details not covered by the modern skills.
- `doc/apigen.go`: API page generation from engine docs.
- `bench/bench.mx3`: benchmark driver input.
- `cmd/mumax3-plot/main.go`: plotting helper for legacy benchmark tables.

## High-value behavior checks
- `printf '1+1\n' | mumax3-script`
- `mumax3 -vet bench/bench.mx3`
- `mumax3-plot path/to/table.txt`
