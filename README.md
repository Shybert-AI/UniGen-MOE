[![中文](https://img.shields.io/badge/lang-中文-red.svg)](README_zh.md)

# UniGen-MOE: Diagnosing & Demarcating Mixture-of-Experts Routing Failures in Video Diffusion
### One Person · One GPU · One Obsession: Stepping into Every Pitfall of MoE in Video Generation

<p align="center">
  <a href="https://arxiv.org/"><img src="https://img.shields.io/badge/arXiv-XXXX.XXXXX-b31b1b.svg" alt="arXiv"></a>
  <a href="https://github.com/Shybert-AI/UniGen-MOE"><img src="https://img.shields.io/github/stars/Shybert-AI/UniGen-MOE?style=social" alt="GitHub stars"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="License"></a>
</p>

> **"Tech giants show you how far MoE can go. I map out every hidden reef along the way."**

This is not a repository chasing State-of-the-Art. It is a **deep diagnostic report** on **Mixture-of-Experts routing in visual diffusion models**, authored by an **independent researcher**.

Between 2024 and 2026, Leading companies are actively promoting the development of DiT architecture towards MoE (Mixture-of-Experts), I — as a solo developer — systematically uncovered a **"selective deadlock" pathology** in MoE routers during video generation training, all within the extreme constraints of a single **NVIDIA A100 (80GB) GPU** and **20,000 open-source data samples**.

---

## 🔭 Why This Matters

The prevailing narrative says Mixture-of-Experts is the answer to scaling diffusion transformers. Yet beneath the surface of industry benchmarks, a systematic failure mode has gone undiagnosed — until now.

This work provides:
- **The first complete phenomenology of routing deadlock** in video diffusion MoEs, spanning three router architectures and 5,000 training steps.
- **Precise calibration of the Token-Choice paradigm boundary** under real-world, single-GPU constraints.
- **A new theoretical lens** — the Functional Redundancy Hypothesis — that reframes deadlock not as a bug, but as a phased, rational system behavior.

---

## 🔬 Core Discoveries

Tracking every MoE routing decision across **30 Transformer layers** over more than **65 million tokens**, three findings stand out:

### 1. Selective Deadlock
After eliminating global deadlock, roughly **one-third of layers** permanently route over 90% of tokens to a single expert. **Increasing the auxiliary loss — even to 0.2 — cannot prevent this.**

### 2. U-Shaped Distribution
Deadlock is not random. It concentrates heavily in **shallow visual processing layers (L1–L8)** and **deep semantic integration layers (L22–L29)**. Intermediate layers remain near-perfectly balanced.

### 3. The bfloat16 Precision Trap
At tiny initialization scales and large weight magnitudes, optimizer updates fall below the hardware's minimum resolvable step in bfloat16. Weights get **truncated to zero** silently — an easily overlooked numerical pitfall.

<img width="1305" height="468" alt="image" src="https://github.com/user-attachments/assets/0db7f50f-9f49-49bd-813e-6ae6ef3589af" />

---

## 📖 The Full Story

This project is accompanied by a detailed, three-part technical narrative chronicling the entire journey — from a naive MoE conversion to a fully calibrated diagnostic framework.

| Part | Title | Core Theme |
|------|-------|------------|
| **I** | [Open-Source Release! One Model for Nine AI Creation Tasks](https://zhuanlan.zhihu.com/p/2033547974647739944) | The starting point: first proposing **MoE as a potential solution to multi-task conflict** within the 9-in-1 unified framework. |
| **II** | [Dense-to-MoE Weight Conversion from Scratch: A Beginner's Groping Journey](https://zhuanlan.zhihu.com/p/2034196108247814223) | A complete autopsy of three router architectures: linear gate global deadlock → MLP gate honeymoon → cross-attention gate self-recovery illusion. |
| **III** | [When MoE Meets Video Generation: A Complete Diagnostic and Repair Journey of Routing Collapse](https://zhuanlan.zhihu.com/p/2036215377823257490) | How mixed-precision training and dynamic expert grouping pushed through single-GPU limits, with a complete roadmap for large-scale extension. |

> For an English overview of the technical findings, please refer to our **[arXiv paper](https://arxiv.org/)**.

---

## 📂 Repository Status: Research Preview

> ⚠️ **Code is currently not publicly available.** This project is in an active research phase, and the codebase is undergoing substantial restructuring for the next phase (dual-GPU full-expert collaborative training). The code will be organized and open-sourced upon completion.

What you can do right now:
- **Read the paper** for complete methodology, experimental design, and the Functional Redundancy Hypothesis.
- **Follow the Chinese series** for a behind-the-scenes narrative of every pitfall encountered and resolved.
- **Watch this repository** — code and model weights will be released upon completion of the next-phase experiments.

---

## 🤝 Collaboration & Contact

I am currently advancing this work in the following directions and welcome all forms of exchange:

- **Resource Collaboration**: Seeking compute sponsorship to scale the diagnostic framework to 7B+ parameter models, empirically locating the critical point predicted by the Functional Redundancy Hypothesis.
- **Academic Collaboration**: Co-authoring technical reports or benchmarking against emerging industry paradigms.

If you share a curiosity about the fundamental mechanics of visual MoE, or wish to support independent research, please reach out.

---

## 📜 Citation

If you use the work from this repository in your research, please cite:

```bibtex
@misc{unigenmoe2026,
  title = {Sparse Mixture-of-Experts Routing in Visual Diffusion Transformers: 
           Diagnosis, Boundary Calibration and Evolutionary Roadmap 
           from Routing Collapse to Selective Deadlock},
  author = {Haiying Sha and Yan Zheng},
  year = {2026},
  howpublished = {\url{https://github.com/Shybert-AI/UniGen-MOE}},
  note = {arXiv preprint}
}
```

Also please cite the underlying base model:

```bibtex
@misc{kiwi2026,
  title = {Kiwi-Edit: Versatile Video Editing via Instruction and Reference Guidance},
  author = {Y. Lin and others},
  year = {2026},
  eprint = {arXiv:2603.02175},
}
```

---

## 📜 License
This project is open-sourced under the MIT License. Model weights follow the original base model (Wan2.2) open-source agreement.

---

<p align="center">
  <i>Fill enough potholes, and a road appears.</i>
</p>
```
