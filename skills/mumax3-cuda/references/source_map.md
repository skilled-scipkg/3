# mumax3 source map: CUDA

Use this only after `doc_map.md`.

## Fast queries
- `rg -n "Init\\(|fatbinLoad|ERROR_NO_BINARY_FOR_GPU|ComputeCapability|DriverVersion|UseCC" cuda`
- `rg -n "CURAND|CUFFT|cuCtx|CUDA_PATH|CUDA_CC" cuda deploy`

## Function-level entry points
- `cuda/init.go`: top-level CUDA context creation, capability checks, and early PTX-load test.
- `cuda/fatbin.go`: PTX target selection and `no binary for GPU` failure path.
- `cuda/cu/init.go`: direct CUDA driver initialization wrapper.
- `cuda/cu/device.go`: device enumeration, compute capability, and memory reporting.
- `cuda/cu/context.go`: CUDA context lifecycle and synchronization.
- `cuda/cufft/plan.go`: CUFFT plan setup and execution plumbing.
- `cuda/curand/generator.go`: CURAND generator setup and use.
- `cuda/curand/status.go`: CURAND status decoding.
- `deploy/deploy_linux.bash`: Linux PTX target and include/library path assembly.
- `deploy/deploy_windows.ps1`: Windows PTX target and CUDA path assembly.

## High-value behavior checks
- `mumax3 -test -gpu=0`
- `go test ./cuda/cu`
- `go test ./cuda/cufft`
- `go test ./cuda/curand`
