# mumax3 source map: Parallel and HPC

Use this only after `doc_map.md`.

## Fast queries
- `rg -n "RunQueue|initGPUs|WaitForJob|GiveJob|WakeupWatchdog|GUI_PORT" cmd/mumax3 cmd/mumax3-server`
- `rg -n "flag_scan|flag_ports|flag_addr|flag_mumax|FairShare|Reque" cmd/mumax3-server`

## Function-level entry points
- `cmd/mumax3/queue.go`: local multi-file queue mode, GPU fan-out, and queue overview page.
- `cmd/mumax3/main.go`: single-file versus queue dispatch.
- `cmd/mumax3-server/doc.go`: operational model, directory layout, and flags.
- `cmd/mumax3-server/main.go`: listener startup, scan range parsing, and peer discovery.
- `cmd/mumax3-server/compute.go`: GPU detection, process launch, GUI port assignment, and result upload.
- `cmd/mumax3-server/que.go`: queued job discovery, user fairness, and re-scan behavior.
- `cmd/mumax3-server/job.go`: job state, output directories, host tracking, and status transitions.
- `cmd/mumax3-server/status.go`: web UI rendering and status page structure.
- `cmd/mumax3-server/watchdog.go`: dead-job detection and automatic re-queue.

## High-value behavior checks
- `mumax3 -test -gpu=0`
- `mumax3 test/standardproblem4.mx3 test/custom_std4.mx3`
- `mumax3-server -scan 127.0.0.1 -ports 35360-35361`
