# 14. GPU & Compute Dashboards

_Section verified: April 2026_

Telemetry and management interfaces for the underlying compute. These surfaces disclose GPU inventory, utilization, workload metadata, and — in the case of cluster schedulers — the ability to submit arbitrary jobs that will run on someone else's hardware.

> Tier legend: 🟥 **T1** unauthenticated by default · 🟧 **T2** auth often misconfigured / known bypasses · 🟨 **T3** recon / fingerprint only.

| Shodan Query | Tier | Notes |
|---|---|---|
| `"NVIDIA DCGM" port:9400 "/metrics"` | 🟧 T2 | |
| `"nvidia-smi" port:8080` | 🟧 T2 | |
| `"RunPod" port:8888` | 🟧 T2 | |
| `"Vast.ai" port:8080` | 🟧 T2 | |
| `"GPUStack" port:80` | 🟧 T2 | |
