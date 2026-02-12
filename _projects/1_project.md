---
layout: page
title: Utilizing ESAM with VLMs for Enhanced Biomedical Image Segmentation
description: Adapting Meta AI's Efficient Segment-Anything Model (ESAM) for biomedical image segmentation, with Vision Language Model integration for interpretation.
img: assets/img/publication_preview/wave-mechanics.gif
importance: 1
category: work
related_publications: false
---

Here’s the vibe: **ESAM is fast and promising**, but medical images are hard. We tested it on lung CTs, found where it breaks, and then leveled it up with **MedESAM** plus **GPT-4o interpretation** so the output isn’t just a mask — it’s a story clinicians can read.

## Why This Matters

- **Tiny tumors are tough**: most target regions are a tiny fraction of the lung.
- **Zero-shot isn’t enough** for clinical reliability.
- **Hybrid models + VLMs** can turn segmentation into explainable, actionable insight.

## Dataset (Quick Take)

- **MSD Task06 (Lung)**, CT scans
- 3D volumes, ~300 slices each, **512×512**
- Expert tumor masks

## Step-by-Step Methods

### 1) Prep the data

- Clip HU values → normalize
- Convert grayscale CT slices to **RGB**
- Keep only slices with tumors
- Save masks in EfficientSAM-ready format

### 2) Try zero-shot ESAM

**Point prompt**

- Erode tumor masks (5×5 kernel) to find a reliable center
- Randomly sample a central point to avoid boundary bias

**Box prompt**

- Build a tight bbox via `cv2.boundingRect`
- Expand **+6 px** on all sides to capture transitional tissue

### 3) Upgrade to MedESAM

- Pair the **ESAM encoder** (fast) with the **MedSAM decoder** (medical-aware)
- Goal: better tumor boundaries without losing efficiency

### 4) Score it

- **IoU** + **Dice (DSC)**

### 5) Interpret it with GPT-4o

- Only high-quality masks (**IoU ≥ 0.7**, **DSC ≥ 0.7**)
- Prompt styles: general analysis, region focus, metric meaning, color meaning, medical context

## Interesting Results

### Point prompt: not great

- IoU / DSC often near zero — small targets are just too hard

### Box prompt: decent on clear cases

- **ESAM-Tiny:** IoU **0.28 ± 0.16**, DSC **0.42 ± 0.19**
- **ESAM-Small:** IoU **0.29 ± 0.15**, DSC **0.43 ± 0.18**

### MedESAM: big jump

- **MedESAM-Tiny:** IoU **0.44 ± 0.17**, DSC **0.59 ± 0.18**
- **MedESAM-Small:** IoU **0.42 ± 0.15**, DSC **0.57 ± 0.17**

## Key Takeaways

- **ESAM alone struggles** on small, messy tumors
- **MedESAM stabilizes performance** and boosts accuracy
- **VLM interpretation** makes the results more human-readable and clinically meaningful

## Limitations (Still Real)

- High variance across tumor types
- Small targets remain the hardest
- No strong benchmarks yet for VLM-based interpretation quality

## Bottom Line

This is a practical pipeline for **fast segmentation + explanation**. ESAM gets you speed, MedESAM gets you accuracy, and GPT-4o helps turn pixel masks into **medical-grade insight**.
