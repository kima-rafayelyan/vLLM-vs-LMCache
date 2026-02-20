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
