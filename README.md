# ComfyUI-series

<!-- SEO Keywords -->
<!-- ComfyUI Docker, ComfyUI CUDA 13, ComfyUI Blackwell, DGX Spark ComfyUI, NVFP4 ComfyUI, ComfyUI GPU optimization -->

![GitHub Repo stars](https://img.shields.io/github/stars/masezou/ComfyUI-series?style=for-the-badge)
![GitHub forks](https://img.shields.io/github/forks/masezou/ComfyUI-series?style=for-the-badge)
![GitHub last commit](https://img.shields.io/github/last-commit/masezou/ComfyUI-series?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![CUDA](https://img.shields.io/badge/CUDA-13%20%7C%2012.x-76B900?style=for-the-badge&logo=nvidia)
![Blackwell](https://img.shields.io/badge/NVIDIA-Blackwell-green?style=for-the-badge)

---

# 🚀 ComfyUI Docker Images Optimized for CUDA, Blackwell, and DGX Spark

High-performance **ComfyUI Docker images** optimized for:

- NVIDIA Blackwell / RTX 50 series
- DGX Spark environments
- CUDA 13 (cu130) with NVFP4 support
- Stable CUDA 12.x compatibility layers

---

## 🔥 Why This Repo Matters

👉 If you want:
- Faster ComfyUI on **Blackwell GPUs**
- Working setup on **DGX Spark (Dynamic RAM fixed)**
- **NVFP4 enabled inference**
- Docker images with **torchaudio (missing in NGC)**

➡️ This repo gives you a **ready-to-run solution**

---

## ⚡ TL;DR (Get Started in 10 seconds)

```bash
docker run --gpus all -p 8188:8188 <image>
```

Open:
```
http://localhost:8188
```

---

## 🧠 Core Differentiation

### cu130 = NOT just "latest CUDA"

This repo provides:

- ✅ DGX Spark Dynamic RAM FIX
- ✅ NVFP4 ENABLED
- ✅ torchaudio INCLUDED
- ✅ Blackwell-optimized runtime

---

## 📊 Image Selection Guide

| Goal | Use |
|------|----|
| Max performance (Blackwell) | cu130 |
| Stable environment | cu128 |
| Debug / compare | cu129 / cu126 |

---

## 🧪 What Makes This Different From NGC?

| Feature | This Repo | NGC |
|--------|----------|-----|
| NVFP4 ready | ✅ | ❌ |
| DGX Spark RAM fix | ✅ | ❌ |
| torchaudio | ✅ | ❌ (often missing) |
| Blackwell tuning | ✅ | Partial |

---

## 🎯 Target Users

- ComfyUI users looking for **Docker setup**
- AI engineers running **DGX Spark**
- Users with **RTX 50 / Blackwell GPUs**
- People struggling with:
  - CUDA mismatch
  - NGC missing packages
  - Performance issues

---

## 📈 Benchmark Positioning

| Image | Position |
|------|---------|
| cu130 | Latest and Fastest |
| cu129 | Blackwell Suports |
| cu128 | Stable |
| cu126 | Legacy |

---

## 🏗 Architecture

```
ComfyUI
  ↓
PyTorch
  ↓
CUDA 13 / 12.x
  ↓
GPU (Blackwell / Ada / Ampere)
```

---

## ⭐ Star This Repo If:

- It saved your setup time
- You use ComfyUI + Docker
- You run Blackwell / DGX Spark
- You care about GPU optimization

---

## 🔗 Keywords

ComfyUI Docker
ComfyUI CUDA 13
ComfyUI Blackwell
ComfyUI DGX Spark
NVFP4 ComfyUI
ComfyUI performance optimization
ComfyUI GPU setup

---

## 🧩 Future Plans

- Benchmarks (cu130 vs cu128)
- Flux / Z-Image-Turbo presets
- Multi-GPU tuning
- Kubernetes deployment

---

## 📜 License

Check upstream licenses for ComfyUI, models, and dependencies.

