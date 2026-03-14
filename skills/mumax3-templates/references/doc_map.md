# mumax3 documentation map: Templates

Use this before `source_map.md`.

## Primary docs
- `doc/README`: tiny but relevant note for the docs directory.
- `doc/Makefile`: public entry point for `make html`.
- `doc/templates/head.html`: shared page head include.
- `doc/templates/header.html`: shared site navigation include.
- `doc/templates/headerpage-template.html`: wrapper that renders `header.html`.
- `doc/templates/examples-template.html`: examples page source and embedded example scripts.
- `doc/templates/api-template.html`: API page source.
- `doc/templates/download-template.html`: download page source.
- `doc/templates/index-template.html`: landing page source.

## Practical validation order
- `cd doc && make html`
- `go run make.go -vet -builddir build`
- Load the generated HTML files and confirm include rendering plus example/API sections.
