---
name: mumax3-cuda
description: This skill should be used when users ask about CUDA runtime prerequisites, compute capability coverage, CUFFT/CURAND bindings, or low-level GPU failures in mumax3.
---

# mumax3: CUDA

## High-Signal Playbook

**Route conditions**
- Stay here for compute capability, `CUDA_CC`, driver/toolkit mismatches, `cuda.h` / `curand.h` / `cufft` failures, or GPU initialization problems.
- Use `mumax3-build-and-install` for the full platform build recipe when the problem is broader than CUDA.
- Use `mumax3-parallel-hpc` for runtime `-gpu` selection, multi-GPU queue mode, or `mumax3-server`.
- Use `mumax3-tex` or `mumax3-getting-started` once the binary is healthy and the question becomes about simulation behavior rather than GPU plumbing.

**Triage questions**
- Is this a prebuilt binary problem or a source-build problem?
- What are the GPU name, driver version, and compute capability from `nvidia-smi`?
- Which CUDA version are you targeting, and what `CUDA_CC` is baked into the build?
- Does `mumax3 -test -gpu=N` fail before any script runs?
- Is the failure about missing headers, missing shared libraries, or `no binary for GPU` style kernel load errors?
- Are you debugging CUDA driver context creation, CUFFT/CURAND wrappers, or generated PTX wrappers?

**Canonical workflow**
- Start with the current prerequisite docs in `README.md` and `doc/templates/download-template.html`.
- Match the installed driver to a supported CUDA line, then map the device capability to `CUDA_CC` (`README.md`, deploy scripts).
- Validate the binary with `mumax3 -test -gpu=0`; that forces `cuda.Init(...)` before any simulation work.
- If you are building from source, inspect the deploy scripts or `cuda/Makefile` to confirm the intended `CUDA_HOME`, `CGO_*`, and PTX targets.
- If the failure is runtime-only, inspect `cuda/init.go` and `cuda/cu/context.go` first, then move outward to CUFFT or CURAND bindings.

**Minimal working example**
- Docs: `README.md`, `cuda/cu/README`, `cuda/cufft/README`, `cuda/curand/README`, `deploy/deploy_linux.bash`.

```bash
nvidia-smi --query-gpu=driver_version,compute_cap --format=csv
export CUDA_CC=89
make
mumax3 -test -gpu=0
```

**Pitfalls / fixes**
- Prebuilt 3.11 downloads require compute capability `>= 5.0`; a working NVIDIA driver alone is not enough (`doc/templates/download-template.html`).
- `CUDA_CC` must include the actual device; otherwise the early PTX load in `cuda.Init()` can fail even before a script is parsed (`cuda/init.go`, deploy scripts).
- `cuda.h`, `curand.h`, or missing CUFFT/CURAND libraries usually mean `CUDA_HOME`, `CUDA_PATH`, `CGO_CFLAGS`, or `CGO_LDFLAGS` are wrong (`README.md`, deploy scripts).
- Different CUDA lines support different compute-capability sets; both deploy scripts encode those ranges explicitly (`deploy/deploy_linux.bash`, `deploy/deploy_windows.ps1`).
- If `cu.Init()` reports `ERROR_UNKNOWN`, MuMax3 suggests trying `sudo nvidia-modprobe -u` (`cuda/init.go`).
- On Windows, the deploy script reads `CUDA_PATH_V<major>_<minor>` and Visual Studio paths directly; missing environment variables or unsupported MSVC versions will block the build (`deploy/deploy_windows.ps1`).

**Convergence / validation checks**
- `mumax3 -test -gpu=N` should print the GPU info and exit cleanly.
- Confirm that the built PTX target list contains the actual device capability you need (`deploy/deploy_linux.bash`, `deploy/deploy_windows.ps1`).
- If several GPUs exist, validate each targeted device independently; MuMax3 creates one CUDA context per selected GPU (`cuda/init.go`, `cuda/cu/context.go`).
- Keep a copy of the exact driver version, CUDA version, and `CUDA_CC` used for a working build; most repeat failures come from drifting one of those three.

## Scope
- CUDA runtime and build prerequisites.
- Compute-capability coverage.
- CUFFT/CURAND and context-level debugging.

## Primary documentation references
- `README.md`
- `doc/templates/download-template.html`
- `cuda/cu/README`
- `cuda/cufft/README`
- `cuda/curand/README`
- Full topic inventory: `references/doc_map.md`

## Source entry points for unresolved issues
- `cuda/init.go`: context creation, capability checks, and early PTX-load test.
- `cuda/fatbin.go`: PTX target selection and `no binary for GPU` failures.
- `cuda/cu/device.go`: device enumeration, compute capability, and memory reporting.
- `cuda/cu/context.go`: CUDA driver context lifecycle and flags.
- `cuda/cufft/plan.go`: CUFFT plan setup and execution entry points.
- `cuda/curand/generator.go`: CURAND generator usage.
- `cuda/curand/status.go`: CURAND error/status decoding.
- `deploy/deploy_linux.bash`: Linux-side `CUDA_CC`, `CUDA_HOME`, `CGO_*` assembly.
- `deploy/deploy_windows.ps1`: Windows-side CUDA/MSVC/PTX assembly.
- Ranked fallback map: `references/source_map.md`
