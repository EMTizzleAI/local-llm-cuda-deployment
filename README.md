
# CUDA-Accelerated Local LLM Deployment (llama.cpp)

## Overview

This project represents a stable, real-world local LLM deployment on consumer hardware.
Built and tuned for reliability, not just demonstration.
Designed as a foundation for future agent systems and embodied AI workflows.

The goal is stable, GPU-backed inference with a standardized launch workflow suitable for future embodied AI integration.

---

## System Specifications

- GPU: NVIDIA RTX 3070 Ti Laptop GPU
- CUDA: -  12.x (verify via nvidia-smi)
- OS: Linux Mint
- Model: mistral-7b-instruct-v0.2.Q4_K_M.gguf
- Context size: 4096
- GPU layer offloading: -ngl 40

---

## Build Process

1. Cloned `llama.cpp`
2. Built with CUDA enabled
3. Downloaded quantized GGUF model (Q4_K_M)
4. Verified GPU detection before runtime tuning

---

## Stable Launch Command

```bash
./bin/llama-cli \
  -m ../models/mistral-7b-instruct-v0.2.Q4_K_M.gguf \
  -ngl 40 \
  -c 4096
