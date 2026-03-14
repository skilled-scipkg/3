---
name: mumax3-build-and-install
description: This skill should be used when users ask about prebuilt downloads, source builds, CUDA and compiler prerequisites, or platform-specific mumax3 build failures.
---

# mumax3: Build and Install

## High-Signal Playbook

**Route conditions**
- Stay here for driver/CUDA/Go/compiler setup, Linux `make`, Windows `deploy_windows.ps1`, or build-log triage (`README.md`, `deploy/deploy_linux.bash`, `deploy/deploy_windows.ps1`).
- Use `mumax3-cuda` when the question narrows to compute capability, CUFFT/CURAND, `cuda.h`, PTX coverage, or low-level GPU initialization.
- Use `mumax3-parallel-hpc` for runtime GPU selection, queue mode, or `mumax3-server` deployment after the binary already works.
- Use `mumax3-getting-started` once the binary is good and the next step is writing or running a first `.mx3` file.

**Triage questions**
- Do you want a precompiled binary or a source build?
- Which OS are you targeting: Linux, Windows, or WSL?
- What do `nvidia-smi`, `nvcc --version`, and `go version` report?
- What is the GPU compute capability, and what `CUDA_CC` are you building for?
- Are you using the root `Makefile` on Linux or the deploy scripts on Windows?
- Is the failure about missing headers/libs, Visual Studio setup, GCC compatibility, or the final executable not appearing?

**Canonical workflow**
- Prefer the download page if the target GPU has a supported compute capability and you do not need a custom build (`doc/templates/download-template.html`).
- For source builds, verify the four required components from `README.md`: NVIDIA driver, Go, CUDA, and a C compiler.
- Query compute capability with `nvidia-smi --query-gpu=compute_cap --format=csv`, convert `8.9 -> CUDA_CC=89`, and match CUDA version to the installed driver (`README.md`).
- On Linux, build from the repo with `make realclean && make`; override `NVCC_CCBIN` if your default GCC is newer than nvcc supports (`README.md`, `deploy/deploy_linux.bash`).
- On Windows, prefer `deploy/deploy_windows.ps1`, set the Visual Studio paths, and pass `-CUDA_VERSIONS` and `-CUDA_CC` explicitly when needed (`README.md`, `deploy/deploy_windows.ps1`).
- Validate the result with `mumax3 -test` before attempting a real simulation.

**Minimal working example**
- Docs: `README.md`, `doc/templates/download-template.html`, `deploy/deploy_linux.bash`, `deploy/deploy_windows.ps1`.

```bash
nvidia-smi --query-gpu=driver_version,compute_cap --format=csv
go version
nvcc --version

export CUDA_CC=89
make realclean
make
mumax3 -test
```

```powershell
./deploy_windows.ps1 -CUDA_VERSIONS 12.6 -CUDA_CC 86
where.exe mumax3.exe
.\mumax3.exe -test
```

**Pitfalls / fixes**
- MuMax3 3.11 prebuilt binaries require an NVIDIA GPU with compute capability `>= 5.0` (`doc/templates/download-template.html`).
- Linux builds often fail because nvcc does not support the system GCC; use `make NVCC_CCBIN=<path_to_older_gcc>` or edit `deploy/deploy_linux.bash` accordingly (`README.md`, `deploy/deploy_linux.bash`).
- Windows builds are sensitive to Visual Studio discovery and CUDA path handling; the project recommends `deploy/deploy_windows.ps1` instead of the Makefiles (`README.md`).
- `cuda.h` or `curand.h` not found means your `CGO_CFLAGS` / `CGO_LDFLAGS` or `CUDA_PATH` are wrong (`README.md`, `deploy/deploy_linux.bash`, `deploy/deploy_windows.ps1`).
- If `mumax3.exe` is never produced, try forcing `CGO_ENABLED=1` as noted in `README.md`.
- If `vcvars64.bat` or MSVC setup fails, rerun from the Developer PowerShell or initialize the Visual Studio environment manually (`README.md`).
- On Windows, avoid CUDA install paths with spaces if possible (`README.md`).

**Convergence / validation checks**
- `nvidia-smi` should show the driver and the intended GPU.
- `nvcc --version`, `go version`, and `gcc --version` or `where.exe cl.exe` should all succeed before the build starts.
- The produced binary should pass `mumax3 -test`; that is the cheapest end-to-end CUDA initialization check because `cmd/mumax3/main.go` calls `cuda.Init(...)`.
- If you build via deploy scripts, confirm the output bundle contains `mumax3`, `mumax3-convert`, and `mumax3-server`.

## Scope
- Prebuilt binary selection.
- Linux and Windows source builds.
- Build-log triage for CUDA, CGO, compiler, and Visual Studio issues.

## Primary documentation references
- `README.md`
- `doc/templates/download-template.html`
- `doc/static/download310.html`
- Full topic inventory: `references/doc_map.md`

## Source entry points for unresolved issues
- `deploy/deploy_linux.bash`: Linux packaging flow, `CUDA_HOME`, `CUDA_CC`, `NVCC_CCBIN`, and CGO flags.
- `deploy/deploy_windows.ps1`: CUDA version detection, Visual Studio compatibility, PTX generation, and packaging.
- `Makefile`: top-level `make` entry point.
- `cuda/init.go`: runtime GPU/context initialization reached by `mumax3 -test`.
- `cmd/mumax3/main.go`: the `-test` and `-v` paths used for validation.
- `engine/gofiles.go`: shared runtime flags such as `-gpu`, `-http`, `-cache`, and output-directory overrides.
- `cuda/Makefile`: wrapper build behavior when the failure is inside CUDA code generation.
- Ranked fallback map: `references/source_map.md`
