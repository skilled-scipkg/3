# mumax3 source map: Build and Install

Use this only after `doc_map.md`.

## Fast queries
- `rg -n "CUDA_CC|NVCC_CCBIN|CGO_CFLAGS|CGO_LDFLAGS|CUDA_PATH|CGO_ENABLED" README.md deploy Makefile cuda cmd engine`
- `rg -n "flag_test|InitIO|Init\\(|fatbinLoad|make html" cmd engine cuda deploy`

## Function-level entry points
- `deploy/deploy_linux.bash`: Linux build env assembly, CUDA discovery, `CGO_*` flags, and GCC selection.
- `deploy/deploy_windows.ps1`: Windows CUDA/MSVC selection, PTX coverage, and output bundle creation.
- `Makefile`: top-level `make`, `realclean`, and binary targets.
- `cmd/mumax3/main.go`: `-test`, `-vet`, and the runtime path used to validate a finished build.
- `engine/gofiles.go`: shared `-gpu`, `-http`, `-cache`, and output-directory flags that surface in real runs.
- `cuda/init.go`: CUDA initialization path exercised by `mumax3 -test`.
- `cuda/Makefile`: lower-level CUDA package build behavior if failures happen after the top-level make starts.

## High-value behavior checks
- `nvidia-smi --query-gpu=driver_version,compute_cap --format=csv`
- `make realclean && make`
- `mumax3 -test`
- `powershell -ExecutionPolicy Bypass -File deploy/deploy_windows.ps1 -CUDA_VERSIONS 12.6 -CUDA_CC 86`
