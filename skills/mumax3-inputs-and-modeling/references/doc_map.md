# mumax3 documentation map: Inputs and Modeling

Use this before `source_map.md`.

## Primary docs
- `doc/templates/api-template.html`: mesh, shapes, regions, parameters, excitation, custom quantities, and custom field sections.
- `doc/templates/examples-template.html`: Standard Problem #2 plus geometry and hysteresis examples that show real script structure.
- `doc/static/api310.html`: stable static API copy when you want a rendered reference instead of the template source.

## Runnable examples
- `test/geom2d.mx3`: geometry fill-factor and average checks.
- `test/regions.mx3`: region definition, region overrides, and region output.
- `test/functionfromfile.mx3`: file-driven interpolation and time-dependent field validation.
- `test/loadfile.mx3`: importing OVF or dump data into scripts.
- `test/initgeomfromovf.mx3`: geometry import from OVF and post-resize validation.
- `test/custom_anisotropy.mx3`: custom field and matching energy term.
- `test/custom_exchange.mx3`: custom exchange term and region-aware validation.

## Practical validation order
- Save `geom`.
- Save `regions`.
- Add one or two region averages to the table.
- Run a short pulse or short relax before long dynamics.
