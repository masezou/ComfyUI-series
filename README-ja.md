# 📦 ComfyUI Series（日本語版）

[![Docker](https://img.shields.io/badge/docker-supported-blue)]()
[![Platform](https://img.shields.io/badge/platform-amd64%20%7C%20arm64-green)]()
[![GPU](https://img.shields.io/badge/GPU-CUDA%20%7C%20ROCm%20%7C%20OpenVINO-orange)]()

---

## 📑 目次

- [概要](#概要)
- [特徴](#特徴)
- [CUDAバリエーション](#cudaバリエーション)
- [実行環境](#実行環境)
- [Dockerfileの種類](#dockerfileの種類)
- [ビルド方法](#ビルド方法)
- [COMFY_REF](#comfy_ref)
- [COMFY_COMMIT](#comfy_commit)
- [設計方針](#設計方針)
- [ユースケース](#ユースケース)
- [注意事項](#注意事項)
- [まとめ](#まとめ)

---

## 概要

このリポジトリは、GPU・プラットフォーム・用途ごとに最適化された
**ComfyUI用Dockerイメージ群**を提供します。

---

## 特徴

- CUDA / ROCm / OpenVINO / CPU を統一設計で提供
- NGC / Docker Hub 両対応
- マルチアーキテクチャ（amd64 / arm64）
- キャッシュ最適化ビルド（apt / pip）
- `COMFY_COMMIT` による再現性のあるビルド
- Custom Nodeの依存関係を起動時に自動解決
- Dynamic VRAM / RAM オフロード対応

---

## CUDAバリエーション

| Version | 内容 |
|--------|------|
| cu126 | Blackwell非対応（検証用） |
| cu128 | Blackwell初対応 |
| cu129 | 最適化開始 |
| cu130 | NVFP4対応・最適化 |
| cu131/132 | NGC最新系 |

---

## 実行環境

### NVIDIA（NGC）

- NVIDIA公式最適化スタック
- 高性能

#### 制約

- torchaudio未搭載
- DGX SparkでDynamic RAM無効

#### 本リポジトリの対応

- torchaudio追加
- comfy-aimdo追加

👉 実用環境として利用可能に

---

### ROCm（AMD）

- AMD GPU対応 (Strix Halo)

---

### OpenVINO（Intel）

- CPU / iGPU向け

---

### CPU

- GPUを使わない軽量・検証用

---

## Dockerfileの種類

```
Dockerfile構成
├── dh-*             → DockerHub PyTorch
├── dh-nvidia-*      → DockerHub + CUDA repo
├── ngc-*            → NVIDIA NGC
└── ngc-cuda-*       → CUDA + pip torch
```

---

## ビルド方法

### AMD64のみ

```bash
COMFY_REF="v0.19.0"
docker buildx build \
  --platform linux/amd64 \
  --build-arg COMFY_REF="${COMFY_REF}" \
  -t <image> \
  -f Dockerfile.xxx \
  .
```

### マルチアーキテクチャ

Pushができるレジストリサーバが必要

```bash
COMFY_REF="v0.19.0"
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  --build-arg COMFY_REF="${COMFY_REF}" \
  -t <image> \
  --push \
  -f Dockerfile.xxx \
  .
```

---

## COMFY_REF

```bash
COMFY_REF="v0.19.0"
```

- バージョン指定

---

## COMFY_COMMIT

```bash
COMFY_COMMIT=$(git ls-remote https://github.com/comfyanonymous/ComfyUI.git refs/heads/master | awk '{print $1}')
```

- upstream変更時のみ再ビルド
- キャッシュ効率最大化

---

## 設計方針

### Dynamic RAM / VRAM

- VRAM不足時のオフロード対応

DGX Sparkでは：

👉 comfy-aimdoで有効化

---

## ユースケース

- GPU世代検証
- DGX Spark
- NAS（QNAP QuTS hero6 cu129）
- ARM環境
- 自宅AI環境

---

## 注意事項

- arm64はCUDA制限あり
- ROCmはドライバ依存
- OpenVINOはノード依存
- cu126は意図的に非対応(Blackwellでは動かないことを証明するために作成)

---

## まとめ

👉 推奨：**ngc-cu130**

👉 ComfyUI 0.19.0で動作確認済み

---

## キーポイント

NGC pytorchイメージの弱点：

- torchaudioなし
- Dynamic RAMなし

👉 本リポジトリで補完済み
