# mumax3 documentation map: Getting Started

Use this before `source_map.md`.

## Primary docs
- `README.md`: install and first validation steps.
- `doc/templates/examples-template.html`: canonical introductory examples and CLI expectations.
- `doc/templates/api-template.html`: syntax, mesh, output, and running sections.
- `doc/static/api310.html`: static rendered API reference for the same topics.

## Runnable examples
- `test/standardproblem4.mx3`: standard first successful run.
- `test/custom_std4.mx3`: same workflow with a custom-defined field term.
- `test/standardproblem4-3d-minimize.mx3`: simple 3D variant with `Minimize()`.

## Practical validation order
- `mumax3 -test`
- `mumax3 -vet test/standardproblem4.mx3`
- Run a short real case and inspect `table.txt` plus one saved field.
