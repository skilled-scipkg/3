---
name: mumax3-templates
description: This skill should be used when users ask about generating or editing the MuMax3 website pages, examples page, API page, or HTML templates under doc/templates.
---

# mumax3: Docs Templates and Site Generation

## High-Signal Playbook

**Route conditions**
- Stay here for `doc/templates/`, `doc/make.go`, `doc/apigen.go`, `make html`, and website regeneration.
- Use `mumax3-getting-started`, `mumax3-inputs-and-modeling`, or `mumax3-tex` for actual simulation questions; the template files document MuMax3, but they do not control solver behavior directly.
- Use `mumax3-build-and-install` if the docs build fails because the local `mumax3` binaries or helper tools are missing.
- Use `mumax3-advanced-topics` only for the tiny legacy `doc/README` note or parser experiments; normal doc generation belongs here.

**Triage questions**
- Are you editing page chrome (`head.html`, `header.html`) or content pages (`examples-template.html`, `download-template.html`, `api-template.html`, `index-template.html`)?
- Do you need to regenerate the site, or only understand which template feeds which page?
- Should examples be executed, only vetted, or left untouched?
- Are you changing the API reference layout, the examples content, or the download/home page?
- Is `gnuplot` available for GPU and hysteresis figures?
- Are you trying to modify the docs site, or the live simulation GUI from `engine/html.go`?

**Canonical workflow**
- Edit the relevant template under `doc/templates/`.
- Rebuild the site from `doc/` with `make html`; this compiles the doc generator, renders HTML, and refreshes static assets (`doc/Makefile`).
- For examples, `doc/make.go` can execute embedded `.mx3` snippets and populate `example*.out`; use `-vet` when you only want syntax checking.
- The API page is not hand-maintained; `doc/apigen.go` walks `engine.World` docs and renders `api-template.html`.
- Validate the generated `index.html`, `download.html`, `examples.html`, `api.html`, and `header.html` in the chosen build directory rather than trusting the template edit alone.

**Minimal working example**
- Docs: `doc/README`, `doc/Makefile`, `doc/make.go`, `doc/apigen.go`.

```bash
cd doc
make html
```

```html
{{.Include "head.html"}}
{{.Include "header.html"}}
```

**Pitfalls / fixes**
- `headerpage-template.html` is just the wrapper; the actual nav and links live in `header.html`.
- `api.html` content comes from engine/script doc registration through `doc/apigen.go`; editing generated HTML is the wrong layer.
- `examples-template.html` can run MuMax3 examples during site generation, so it needs a working binary unless you use `-vet`.
- `make html` tries to generate `gpus.svg` with `gnuplot`; if `gnuplot` is missing, that asset may stay stale.
- The example generator rewrites newlines inside raw-string example blocks as a template hack; keep embedded example code inside backticks, not ad hoc HTML escapes.
- The live simulation GUI is a different template stack in `engine/html.go`; do not change the docs site expecting the in-run GUI to follow.

**Convergence / validation checks**
- Confirm the expected HTML files appear in the build directory after regeneration.
- If example content changed, check that the corresponding `example*.out` directories or plots also changed.
- If API docstrings changed in Go, confirm the rendered `api.html` reflects them after regeneration.
- Load the generated pages and verify that `head.html` and `header.html` includes render correctly.

## Scope
- Documentation website generation.
- Examples and API page templating.
- Mapping template files to generated HTML pages.

## Primary documentation references
- `doc/README`
- `doc/Makefile`
- `doc/templates/headerpage-template.html`
- `doc/templates/header.html`
- `doc/templates/head.html`
- `doc/templates/examples-template.html`
- `doc/templates/api-template.html`
- `doc/templates/download-template.html`
- `doc/templates/index-template.html`
- Full topic inventory: `references/doc_map.md`

## Source entry points for unresolved issues
- `doc/make.go`: site generator, example execution, template includes, and page creation.
- `doc/apigen.go`: API page generation from `engine.World` docs.
- `doc/gpus.go`: post-processing of the generated GPU benchmark SVG.
- `doc/Makefile`: `make html` entry point and static asset handling.
- `cmd/mumax3-convert/main.go`: image/data conversion helper used during doc generation.
- `cmd/mumax3-plot/main.go`: table plotting helper used by examples and docs.
- `engine/html.go`: live simulation GUI HTML for requests that are not actually about the docs site.
- Ranked fallback map: `references/source_map.md`
