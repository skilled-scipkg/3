# mumax3 documentation map: Parallel and HPC

Use this before `source_map.md`.

## Primary docs
- `README.md`: high-level runtime expectations and binary prerequisites.
- `cmd/mumax3-server/doc.go`: authoritative operational note for `mumax3-server`, directory layout, scan ranges, ports, and re-queue behavior.
- `bench/gpus.txt`: rough single-GPU throughput table for order-of-magnitude expectations.

## Cheap validation cases
- `test/standardproblem4.mx3`: safe first input when validating one GPU or one node.
- A user-directory tree such as `john/test.mx3` is required before `mumax3-server` will schedule jobs.

## Practical validation order
- `mumax3 -test -gpu=N` on every intended GPU.
- `mumax3 file1.mx3 file2.mx3` for local queue mode.
- `mumax3-server -scan ... -ports ...` only after local runs are healthy.
