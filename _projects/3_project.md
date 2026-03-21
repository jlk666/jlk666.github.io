---
layout: page
title: Parameter-Efficient Fine-Tuning for CLIP
description: An empirical study of parameter efficient fine-tuning for adapting CLIP to downstream tasks.
img: assets/img/peftclip-figure.png
importance: 2
category: work
related_publications: false
---

Let’s face it: fine-tuning massive Vision-Language models like CLIP often feels like trying to parallel park a semi-truck in downtown Manhattan—it’s resource-heavy, slightly terrifying, and one wrong move leads to "catastrophic forgetting." In our latest project, Parameter-Efficient Fine-Tuning for Vision-Language Models, we decided to trade the heavy machinery for a surgical scalpel. We explored how Parameter-Efficient Fine-Tuning (PEFT) techniques—like Prompt Tuning and Adapters—can help these models adapt to new tasks without needing a supercomputer or losing their "common sense" pre-trained knowledge.

We put these methods through a gauntlet of 19 diverse datasets, moving beyond the usual ImageNet comfort zone to see how they handle the "weird" stuff: satellite imagery, medical scans, and complex scenes that require actual reasoning (like figuring out the elevation of a camera or the strategy of a game). The results were a bit of a reality check: while PEFT is an absolute rockstar at handling specialized data and small samples, it still gets a little "modal-mixed-up" when things get structurally complex. We’ve open-sourced our code and evaluation framework to help the community bridge this gap, proving that you don't need to retrain billions of parameters to make a model smart—you just need to tune the right ones.

An emprical study of parameter efficient fine-tuning for adapting CLIP to downstream tasks:

- [x] **Dataset**: [VTAB-1k](https://google-research.github.io/task_adaptation/)
- [ ] **Fine-tuning Strategy**
- [ ] **Backbone**

### Environment Setup
All the code is tested on **python 3.9+**, **CUDA 11.7/12.0**

```bash
# create a new conda environment
conda create -n peft_clip python=3.9
conda activate peft_clip

pip install -r requirements.txt
```

Optional: Install and configure [wandb](https://wandb.ai/site) for logging and visualization.

```bash
pip install wandb
wandb login
```

### Supported Tasks(Dataset) and Backbone
See [prepare_vtab.md](src/data/prompt.md) and [prompt.md](src/data/prompt.md) for prepare and learn the dataset
| Task | Backbone |
| :---: | :---: |
| vtab-caltech101 | ViT-B32 |
| vtab-cifar100 | ViT-B16 |
| vtba-dtd | ViT-L14 |
| vtab-oxford_flowers | MetaCLIP-B32-400m |
| vtab-oxford_pet | MetaCLIP-B32-2.5B |
| vtab-svhn | MetaCLIP-B16-400M |
| vtab-sun397 | MetaCLIP-B16-2.5B |
| vtba-pcam |
| vtab-eurosat |
| vtab-resisc45 |
| vtab-clevr_count |
| vtab-clevr_distance |
| vtba-dmlab |
| vtab-kitti |
| vtab-dSprites_location |
| vtab-dSprites_orientation |
| vtab-smallnorb_azimuth |
| vtab-smallnorb_distance |

### Supported Strategy
| Model |
| :---: |
| [CLIP-Adapter](https://github.com/gaopengcuhk/CLIP-Adapter) |
| [VPT-CLIP-Shallow](https://github.com/KMnP/vpt) |
| [VPT-CLIP-Deep](https://github.com/KMnP/vpt)|

### Running
```bash
python train.py \
      --data "<dataset_name>" \     # Specify the dataset(task) name from table in Supported Tasks
      --backbone "<backbone_name>" \ # Choose the backbone architecture from table in Supported backbone
      --model "<strategy_name>" \   # Define the strategy model from table in Supported Strategy
      --type "<inference_type>" \   # Set the inference type to either "vision" or "vision-language"
      --shots "<num_shots>" \       # Indicate the number of shots
      --seeds "<seed>"              # Provide the seed value for reproducibility
```
