# mumax3 documentation map: Run Workflows and Output

Use this before `source_map.md`.

## Primary docs
- `doc/templates/examples-template.html`: Standard Problem #4, hysteresis, and workflow-oriented examples.
- `doc/templates/api-template.html`: running, output, and restart-related API sections.
- `doc/static/api310.html`: static rendered API reference for the same run-control topics.
- `doc/tex/mumax3.tex`: equation summary when you need the theory reference after the workflow docs.

## Runnable examples
- `test/runwhile.mx3`: stopping on a physical condition.
- `test/minimizer.mx3`: `Minimize()` behavior.
- `test/table.mx3`: table schema and custom table variables.
- `test/outputformat.mx3`: output format changes.
- `test/snapshot.mx3`: snapshot formats and on-the-fly images.
- `test/standardproblem4-3d-minimize.mx3`: small 3D minimize example.

## Practical validation order
- `mumax3 -vet test/runwhile.mx3`
- Run a short case and confirm `table.txt` plus the expected saved files.
- Validate restart inputs with a short continuation run before long jobs.
