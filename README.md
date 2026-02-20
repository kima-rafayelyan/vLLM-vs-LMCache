# 🚀 vLLM vs vLLM + LMCache Benchmark

This project benchmarks **large language model inference performance** using **vLLM** with and without **LMCache**, demonstrating how **KV cache reuse and prefix caching** can dramatically reduce latency and improve throughput for long-context workloads.

---

## 📌 Overview

Modern LLM applications often reuse **long shared prefixes** (system prompts, documents, RAG contexts).  
Recomputing these prefixes for every request is expensive.

This benchmark compares:

- **Baseline:** vLLM without prefix caching  
- **Optimized:** vLLM + LMCache with KV cache reuse  

The goal is to quantify:

- Latency improvement
- Throughput improvement
- GPU memory trade-offs

---

## 🧠 Model Used

- **Model:** `Qwen/Qwen2.5-1.5B-Instruct`
- **Context length:** Very long shared prefix (repeated text ×400)
- **Hardware:** NVIDIA Tesla T4 (FP16)

---

## ⚙️ Installation

```bash
pip install -U -q vllm lmcache transformers accelerate pandas

```

## 🧩 Experiments

### 1️⃣ vLLM Baseline (No Prefix Caching)

- Prefix caching disabled  
- Every request recomputes the full context  
- Represents a **cold-start production scenario**

### 2️⃣ vLLM + LMCache

- Prefix caching enabled  
- KV cache shared using `LMCacheConnectorV1`  
- Two runs:
  - **Cold run:** fills cache
  - **Warm run:** reuses cached KV states

---

## 📊 Results

| Method        | Latency (s) | Throughput (req/s) | GPU Memory (MB) |
|---------------|-------------|-------------------|----------------|
| vLLM Cold     | 8.15        | 0.12              | 11,950         |
| LMCache Warm  | 0.31        | 3.25              | 13,416         |

## 🚀 Speedup

With LMCache, throughput is **27.08× faster** compared to the vLLM cold run.  

This demonstrates how **prefix caching and KV reuse** can drastically reduce latency for long-context LLM workloads.
