# Gemma 4B Quantization Benchmark & Telemetry

An empirical study and benchmark evaluating the impact of **Post-Training Quantization (PTQ)** on inference throughput, hardware resource utilization, and logical accuracy in the **Gemma 4B** model. 

This repository contains the completed research paper and the companion interactive telemetry dashboard.

---

## 📂 Repository Contents

* 📄 **[RMCS UTS Q5 Completed.docx](RMCS%20UTS%20Q5%20Completed.docx)**: The complete, academic-formatted research paper (IEEE template) containing introduction, methodology, quantitative results analysis, and conclusions.
* 📊 **[benchmark_data.html](benchmark_data.html)**: A premium, responsive HTML dashboard containing interactive tables for model deployment configurations, hardware telemetry loads, and question-by-question benchmark accuracy comparisons.

---

## 📈 Key Findings Summary

Our benchmarks reveal a sharp **performance vs. precision trade-off** when deploying quantized models locally on consumer edge hardware (NVIDIA GPU with 8.0 GB VRAM):

### 1. Telemetry & Throughput Performance
* **Gemma 4B Q4 (4-bit Quantized GGUF)**:
  * **Memory Size**: 7.7 GB (fits entirely in GPU VRAM).
  * **Offload Status**: 100% GPU-accelerated.
  * **Throughput**: **106.8 tokens/second** (average generation speed).
  * **CPU Load**: 14% utilization, keeping temperatures stable at 63°C.
* **Gemma 4B BF16 (Uncompressed Baseline)**:
  * **Memory Size**: 11.0 GB (exceeds VRAM capacity).
  * **Offload Status**: Split offload (59% CPU / 41% GPU).
  * **Throughput**: **25.5 tokens/second** (average generation speed).
  * **CPU Load**: **28% utilization** with heavy CPU heat build-up (**82°C**) due to memory bus transfer bottlenecks. This represents a **4.2x slowdown** compared to Q4.

### 2. Logical Accuracy & The "Intelligence Gap"
Using a custom 10-question Zero-Shot Multiple Choice Logic & Reasoning Benchmark, we identified a measurable logical degradation:
* **BF16 Accuracy**: **90% (9/10)**. Successfully solved 3D spatial rotations (Q7) and bitwise recursive logic (Q4).
* **Q4 Accuracy**: **80% (8/10)**. 
  * **Spatial Reasoning Failure (Q7)**: The 4-bit model failed to track 3D rotations, choosing 1 (Option A) instead of 5 (Option C).
  * **Arithmetic Hallucination (Q4)**: The 4-bit model incorrectly calculated bitwise operations during dry-runs (concluding $16 \& 8 = 8$), but selected the correct Option C (24) only because it forced itself to stop early to match the available multiple-choice options.
* **Complexity Limits (Q5)**: Both models failed to resolve complex epistemic logic (Cheryl's Birthday), indicating high-order multi-agent constraints remain an architectural limitation regardless of quantization.

---

## 🛠️ How to View the Dashboard

To view the detailed interactive tables (Ollama specs, CPU/GPU telemetry, and question accuracy runs):
1. Clone this repository:
   ```bash
   git clone https://github.com/HeaTTap/gemma4b-quantization-benchmark.git
   ```
2. Navigate to the folder:
   ```bash
   cd gemma4b-quantization-benchmark
   ```
3. Open `benchmark_data.html` in any web browser.

---

## 🎓 Academic Citations

This study builds on the following methodologies:
* **LLM.int8()**: Dettmers, T., et al. (2022). *8-bit Matrix Multiplication for Transformers at Scale*.
* **GPTQ**: Frantar, E., et al. (2023). *Accurate Post-Training Quantization for Generative Pre-trained Transformers*.
