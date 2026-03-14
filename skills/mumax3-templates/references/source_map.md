# mumax3 source map: Templates

Use this only after `doc_map.md`.

## Fast queries
- `rg -n "buildAPI|renderAPI|createIndexPage|createDownloadPage|createHeaderPage|Example\\(" doc`
- `rg -n "mumax3-convert|mumax3-plot|gpus.svg|Include\\(" doc cmd`

## Function-level entry points
- `doc/make.go`: main site generator, embedded example execution, template includes, and HTML creation.
- `doc/apigen.go`: API-page generation from the registered engine world.
- `doc/gpus.go`: post-processing of the generated GPU benchmark SVG.
- `doc/Makefile`: `make html` recipe and static asset refresh.
- `cmd/mumax3-convert/main.go`: converter used to create example images.
- `cmd/mumax3-plot/main.go`: plotting helper used to turn `table.txt` into SVGs.
- `engine/html.go`: separate live-simulation GUI template stack when the request is not really about the docs site.

## High-value behavior checks
- `cd doc && make html`
- `cd doc && go run make.go -vet -builddir build`
- `cmd/mumax3-convert -help`
