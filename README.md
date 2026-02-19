# ComfyUI-series

A clean and structured "textbook-style" Docker build set for running
**ComfyUI** on modern NVIDIA GPUs and multi-architecture environments.

This repository provides multiple Dockerfile variants targeting:

-   Different CUDA versions (cu130 / cu128 / cu126)
-   Different base images (Docker Hub PyTorch / NVIDIA CUDA / NGC)
-   Single-arch (amd64) and multi-arch (amd64 + arm64)
-   Blackwell-optimized and compatibility-focused builds

The goal is clarity, reproducibility, and predictable GPU compatibility
across generations.

------------------------------------------------------------------------

## Overview

### GPU Generations Covered

-   Blackwell (50x0 / DGX Spark)
-   Hopper
-   Ampere
-   Ada
-   Turing

Some legacy builds intentionally exclude Blackwell for compatibility or
driver reasons.

------------------------------------------------------------------------

## Image Families

  ----------------------------------------------------------------------------
  Family              Base Image               Arch         Purpose
  ------------------- ------------------------ ------------ ------------------
  comfyui-dh          Docker Hub               amd64        Minimal amd64-only
                      pytorch/pytorch                       reference builds

  comfyui-dh-nvidia   Docker Hub nvidia/cuda   amd64 +      Multi-arch CUDA
                                               arm64        userland builds

  comfyui-ngc         NVIDIA NGC PyTorch       amd64 +      NVIDIA official
                                               arm64        full-stack builds
  ----------------------------------------------------------------------------

------------------------------------------------------------------------

# 1) Docker Hub PyTorch Base (amd64 only)

These builds use `pytorch/pytorch` from Docker Hub.\
Target architecture: **linux/amd64 only**.

------------------------------------------------------------------------

## A. CU130 / Blackwell-Optimized

**Dockerfile.dh**

-   First generation optimized for Blackwell
-   Also runs on Hopper / Ampere / Ada / Turing
-   Recommended for modern systems

``` bash
docker buildx use default
docker buildx build   --platform linux/amd64   --build-arg COMFY_REF=master \
--build-arg COMFY_CACHEBUST=$(date +%Y%m%d%H%M%S) \
--tag registry.example.com/comfyui-dh:2.10.0-cu130 \
--cache-from type=registry,ref=registry-buildcache.example.com/buildcache/comfyui-dh:cu130 \
--cache-to   type=registry,ref=registry-buildcache.example.com/buildcache/comfyui-dh:cu130,mode=max \
--push \
-f Dockerfile.dh \
.
```

------------------------------------------------------------------------

## B. CU128 / Compatibility Mode

**Dockerfile.dh-cu128**

-   Works on Blackwell but not optimized
-   Good compatibility fallback

``` bash
docker buildx use default
docker buildx build   --platform linux/amd64   --build-arg COMFY_REF=master \
--build-arg COMFY_CACHEBUST=$(date +%Y%m%d%H%M%S) \
--tag registry.example.com/comfyui-dh-cu128:2.7.0-cu128 \
--cache-from type=registry,ref=registry-buildcache.example.com/buildcache/comfyui-dh-cu128:cu128 \
--cache-to   type=registry,ref=registry-buildcache.example.com/buildcache/comfyui-dh-cu128:cu128,mode=max \
--push \
-f Dockerfile.dh-cu128 \
.
```

------------------------------------------------------------------------

## C. Legacy / Pre-Blackwell

**Dockerfile.dh-legacy**

-   For Hopper / Ampere / Ada / Turing
-   Not intended for Blackwell

``` bash
docker buildx use default
docker buildx build   --platform linux/amd64   --build-arg COMFY_REF=master \
--build-arg COMFY_CACHEBUST=$(date +%Y%m%d%H%M%S) \
--tag registry.example.com/comfyui-dh-legacy:2.6.0-cu126 \
--cache-from type=registry,ref=registry-buildcache.example.com/buildcache/comfyui-dh-legacy:cu126 \
--cache-to   type=registry,ref=registry-buildcache.example.com/buildcache/comfyui-dh-legacy:cu126,mode=max \
--push \
-f Dockerfile.dh-legacy \
.
```

------------------------------------------------------------------------

# 2) NVIDIA CUDA Base (Multi-Arch)

These builds use `nvidia/cuda` images.\
Target architectures: **linux/amd64 + linux/arm64**

------------------------------------------------------------------------

## A. CU130 / Blackwell-Optimal

**Dockerfile.dh-nvidia**

``` bash
docker buildx use mpbuilder
docker buildx build   --platform linux/amd64,linux/arm64   --build-arg COMFY_REF=master \
--build-arg COMFY_CACHEBUST=$(date +%Y%m%d%H%M%S) \
--tag registry.example.com/comfyui-dh-nvidia:cu130 \
--cache-from type=registry,ref=registry-buildcache.example.com/buildcache/comfyui-dh-nvidia:cu130 \
--cache-to   type=registry,ref=registry-buildcache.example.com/buildcache/comfyui-dh-nvidia:cu130,mode=max \
--push \
-f Dockerfile.dh-nvidia \
.
```

------------------------------------------------------------------------

# 3) NGC Base (Multi-Arch)

These builds use official NVIDIA NGC PyTorch containers.

------------------------------------------------------------------------

## ⚠️  NGC Access Requirement

Images based on **NVIDIA NGC (`nvcr.io`)** require an NVIDIA NGC
account.

1.  Create an account at https://ngc.nvidia.com/
2.  Generate an API key
3.  Login:

``` bash
docker login nvcr.io
```

Username: `$oauthtoken`\
Password: `<your NGC API key>`

Without authentication, Docker cannot pull NGC base images.

------------------------------------------------------------------------

## Multi-Architecture Verification

``` bash
docker buildx imagetools inspect registry.example.com/comfyui-dh-nvidia:cu130
docker buildx imagetools inspect registry.example.com/comfyui-ngc:25.11
```

Expected:

-   comfyui-dh:\* → linux/amd64 only
-   comfyui-dh-nvidia:\* → linux/amd64 + linux/arm64
-   comfyui-ngc:\* → linux/amd64 + linux/arm64

------------------------------------------------------------------------

## Notes

-   Ensure your NVIDIA driver version is compatible with the selected
    CUDA version.
-   Blackwell-optimized builds require sufficiently recent drivers.
-   Multi-arch builds require a properly configured buildx builder.
-   Registry cache configuration significantly reduces rebuild time.

------------------------------------------------------------------------

## License

This repository contains Docker build definitions and does not
redistribute proprietary binaries.

Refer to:

-   NVIDIA container license
-   Docker Hub image licenses
-   ComfyUI upstream license

------------------------------------------------------------------------

This repository focuses on clarity, reproducibility, and predictable GPU
compatibility across generations and architectures.

