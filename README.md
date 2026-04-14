# 📦 ComfyUI Series

[![Docker](https://img.shields.io/badge/docker-supported-blue)]()
[![Platform](https://img.shields.io/badge/platform-amd64%20%7C%20arm64-green)]()
[![GPU](https://img.shields.io/badge/GPU-CUDA%20%7C%20ROCm%20%7C%20OpenVINO-orange)]()

---

## 📑 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [CUDA Variants](#cuda-variants)
- [Runtime Environments](#runtime-environments)
- [Dockerfile Variants](#dockerfile-variants)
- [Build Instructions](#build-instructions)
- [COMFY_REF](#comfy_ref)
- [COMFY_COMMIT](#comfy_commit)
- [Design Principles](#design-principles)
- [Use Cases](#use-cases)
- [Notes](#notes)
- [Conclusion](#conclusion)

---

## Overview

This repository provides optimized Docker images for ComfyUI, tailored for different GPUs, platforms, and use cases.

---

## Features

- CUDA / ROCm / OpenVINO / CPU unified support
- NGC and Docker Hub base images
- Multi-architecture (amd64 / arm64)
- Cache-optimized builds (apt / pip)
- Reproducible builds using COMFY_COMMIT
- Runtime dependency loading for custom nodes
- Dynamic VRAM / RAM offloading

---

## CUDA Variants

| Version | Description |
|--------|------------|
| cu126 | No Blackwell support (intentional testing) |
| cu128 | First Blackwell support |
| cu129 | Initial optimization |
| cu130 | NVFP4 + optimized |
| cu131/132 | Latest NGC builds |

---

## Runtime Environments

### NVIDIA (CUDA - NGC)

- Highest performance
- Official NVIDIA optimized stack

#### Limitations

- torchaudio is NOT included
- Dynamic RAM disabled on DGX Spark

#### This repo fixes:

- Adds torchaudio
- Integrates comfy-aimdo

👉 Makes NGC production-ready

---

### ROCm (AMD GPU)

- Based on rocm/pytorch
- AMD GPU support

---

### OpenVINO (Intel)

- CPU + iGPU support
- OpenVINO nodes

---

### CPU

- Lightweight
- No GPU required

---

## Dockerfile Variants

```
Dockerfile Types
├── dh-*             → DockerHub PyTorch
├── dh-nvidia-*      → DockerHub + CUDA repo
├── ngc-*            → NVIDIA NGC PyTorch
└── ngc-cuda-*       → CUDA base + pip torch
```

---

## Build Instructions

### AMD64 only

```bash
COMFY_REF="v0.19.0"
docker buildx build   --platform linux/amd64 --build-arg COMFY_REF="${COMFY_REF}"  -t <image>   -f Dockerfile.xxx   .
```

### Multi-arch

```bash
COMFY_REF="v0.19.0"
docker buildx build   --platform linux/amd64,linux/arm64 --build-arg COMFY_REF="${COMFY_REF}"  -t <image>   --push   -f Dockerfile.xxx   .
```

---

## COMFY_REF

```bash
COMFY_REF="v0.19.0"
```

- Enable to set ComfyUI version

---

## COMFY_COMMIT

If you set COMFY_REF, you don't use set COMFY_COMMIT

```bash
COMFY_COMMIT=$(git ls-remote https://github.com/comfyanonymous/ComfyUI.git refs/heads/master | awk '{print $1}')
```

- Rebuild only when upstream changes
- Maximizes cache reuse

---

## Design Principles

### Dynamic RAM / VRAM

- Enables low VRAM operation
- Required for DGX Spark

Solution:

- comfy-aimdo integration

---

## Use Cases

- GPU validation (Ampere / Ada / Blackwell)
- DGX Spark environments
- NAS (QNAP QuTS hero 6 CU129)
- ARM systems
- Home AI labs

---

## Notes

- arm64 CUDA limitations exist
- ROCm depends on kernel/driver
- OpenVINO depends on node support
- cu126 intentionally does NOT support Blackwell

---

## Conclusion

👉 Recommended default: **ngc-cu130**
👉 Verified ComfyUI 0.19.0

This repository transforms raw GPU stacks into practical, production-ready environments.

---

## Key Insight

NGC images lack:
- torchaudio
- Dynamic RAM (DGX Spark)

This repo solves both.
