# mumax3 documentation map: CUDA

Use this before `source_map.md`.

## Primary docs
- `README.md`: CUDA toolkit, driver, and compute-capability requirements.
- `doc/templates/download-template.html`: current binary eligibility and download guidance.
- `cuda/cu/README`: CUDA driver wrapper package notes.
- `cuda/cufft/README`: CUFFT package notes.
- `cuda/curand/README`: CURAND package notes.

## Operational references
- `deploy/deploy_linux.bash`: Linux-side `CUDA_CC`, `CUDA_HOME`, and compiler compatibility handling.
- `deploy/deploy_windows.ps1`: Windows-side CUDA version and PTX-target handling.

## Validation checkpoints
- `mumax3 -test -gpu=0`
- `nvidia-smi --query-gpu=name,driver_version,compute_cap --format=csv`
- Confirm the actual device compute capability is covered by the build target list.
