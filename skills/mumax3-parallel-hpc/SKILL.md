---
name: mumax3-parallel-hpc
description: This skill should be used when users ask about GPU selection, local multi-GPU batch execution, mumax3-server, or networked MuMax3 execution.
---

# mumax3: Parallel and HPC

## High-Signal Playbook

**Route conditions**
- Stay here for `-gpu`, multi-file queue mode, `mumax3-server`, peer scanning, GUI ports, or network execution.
- Use `mumax3-cuda` if the real failure is device capability, PTX coverage, or low-level CUDA initialization.
- Use `mumax3-build-and-install` if the node cannot build or pass `mumax3 -test`.
- Use `mumax3-tex` for single-run workflow questions once the execution environment is already stable.

**Triage questions**
- Is this one local GPU, multiple local GPUs, or several networked nodes?
- Are you running one input file or a batch of many `.mx3` files?
- Do all nodes already have a working `mumax3` binary and visible GPUs?
- Are job files organized by user directories for `mumax3-server`?
- Is the issue about scheduling, GUI reachability, firewall/port access, or performance?
- Do you need queue mode from `mumax3` itself, or the separate `mumax3-server` service?

**Canonical workflow**
- For a single local run, select the device with `-gpu=N` and keep the question local first.
- For multiple local inputs, invoke `mumax3 file1.mx3 file2.mx3 ...`; `cmd/mumax3/queue.go` fans jobs across available GPUs and serves a queue overview page.
- For a small cluster, start `mumax3-server` in a directory containing `user/*.mx3` input trees, let it auto-scan peers, and use the web UI on port `35360` (`cmd/mumax3-server/doc.go`).
- Override `-scan`, `-ports`, `-l`, `-exec`, or `-cache` only when the default LAN scan and binary selection are wrong.
- Validate that each node reports GPUs via `mumax3 -test -gpu=N` before trusting the cluster queue.

**Minimal working example**
- Docs: `cmd/mumax3-server/doc.go`, `cmd/mumax3/queue.go`, `README.md`, `bench/gpus.txt`.

```bash
mumax3 -gpu=0 job.mx3
mumax3 job1.mx3 job2.mx3 job3.mx3
mumax3-server -scan 127.0.0.1,192.168.0.1-128 -ports 35360-35361
```

**Pitfalls / fixes**
- MuMax3 is GPU-parallel, not an MPI/OpenMP solver; each running job consumes one GPU.
- Queue mode only happens when `mumax3` gets more than one positional input file (`cmd/mumax3/main.go`, `cmd/mumax3/queue.go`).
- The local queue overview uses the `-http` port, and individual job GUIs are offset from it by GPU index (`cmd/mumax3/queue.go`).
- `mumax3-server` expects inputs under user directories such as `john/file1.mx3`; loose files are ignored (`cmd/mumax3-server/doc.go`).
- Port `35360` must be reachable between nodes; when `localhost` does not work, use the exact IP address (`cmd/mumax3-server/doc.go`).
- Interrupted jobs are designed to be re-queued; repeated re-queues usually mean the underlying node or build is still unhealthy (`cmd/mumax3-server/doc.go`, `cmd/mumax3-server/watchdog.go`).

**Convergence / validation checks**
- Run `mumax3 -test -gpu=N` on every GPU you intend to schedule.
- Confirm the queue or server UI is reachable and shows the expected GPUs and job states.
- For `mumax3-server`, verify that `.out`, `stdout.txt`, host, and GUI links appear for a submitted job (`cmd/mumax3-server/status.go`, `cmd/mumax3-server/compute.go`).
- Use `bench/gpus.txt` only as an order-of-magnitude performance reference; it is not a sizing guarantee.

## Scope
- Local GPU selection.
- Local multi-GPU queue mode.
- `mumax3-server` cluster-style execution and network behavior.

## Primary documentation references
- `README.md`
- `cmd/mumax3-server/doc.go`
- `bench/gpus.txt`
- Full topic inventory: `references/doc_map.md`

## Source entry points for unresolved issues
- `cmd/mumax3/queue.go`: local multi-file queue mode and queue-overview web page.
- `cmd/mumax3/main.go`: single-file versus queue dispatch.
- `cmd/mumax3-server/doc.go`: operational model, directory layout, flags, and re-queue semantics.
- `cmd/mumax3-server/main.go`: listener, scan, and peer-discovery setup.
- `cmd/mumax3-server/compute.go`: GPU detection, process launch, GUI port assignment, and result handling.
- `cmd/mumax3-server/que.go`: queued job discovery, fairness, and rescan behavior.
- `cmd/mumax3-server/job.go`: job state transitions, output directories, and host tracking.
- `cmd/mumax3-server/status.go`: web UI rendering and job status links.
- `cmd/mumax3-server/watchdog.go`: dead-job detection and automatic re-queue.
- Ranked fallback map: `references/source_map.md`
