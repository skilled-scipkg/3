# mumax3 documentation map: Build and Install

Use this before `source_map.md`.

## Primary docs
- `README.md`: prerequisite checks, Linux and Windows build steps, `CUDA_CC`, GCC or MSVC caveats, and `mumax3 -test`.
- `doc/templates/download-template.html`: current download and install guidance, including prebuilt-binary eligibility.
- `doc/static/download310.html`: stable static copy of the download page for version-specific wording.

## Operational scripts
- `deploy/deploy_linux.bash`: exact Linux environment variables, CUDA path handling, and compiler selection.
- `deploy/deploy_windows.ps1`: CUDA version switches, Visual Studio discovery, PTX targets, and packaged build flow.
- `Makefile`: top-level Linux build entry point.

## Validation checkpoints
- `nvidia-smi`
- `nvcc --version`
- `go version`
- `gcc --version` on Linux or `where.exe cl.exe` on Windows
- `mumax3 -test`
