# mumax3 documentation map: Index

Use this file to choose the right topic skill before opening code.

## Skill routing
- `mumax3-getting-started`: first local run, Standard Problem #4, basic syntax, and GUI startup.
- `mumax3-inputs-and-modeling`: mesh, geometry, regions, parameter assignment, imported data, and custom field expressions.
- `mumax3-tex`: `Run`, `RunWhile`, `Relax`, `Minimize`, restart, table output, autosave, and snapshots.
- `mumax3-build-and-install`: driver/CUDA/Go/compiler setup, prebuilt binaries, Linux `make`, and Windows deploy script usage.
- `mumax3-cuda`: compute capability, PTX coverage, CUDA driver/runtime failures, CUFFT, and CURAND.
- `mumax3-parallel-hpc`: `-gpu`, multi-file queue mode, `mumax3-server`, LAN scanning, and job re-queue behavior.
- `mumax3-templates`: `doc/` website generation, examples/API page generation, and template includes.
- `mumax3-advanced-topics`: legacy `api39c`, `source(...)`, parser internals, and low-signal legacy docs.

## Fast starting points
- `README.md`: installation, build, and runtime overview.
- `doc/templates/examples-template.html`: practical end-user examples.
- `doc/templates/api-template.html`: organized API reference by topic.
- `cmd/mumax3-server/doc.go`: authoritative operational note for clustered execution.
- `test/standardproblem4.mx3`: canonical simulation smoke test once the binary works.

## Durable routing rule
- If the question can be answered from docs or tests, stay out of source.
- If source is needed, open the chosen skill's `source_map.md` rather than browsing directories blindly.
