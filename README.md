# Adversarial Robustness of Vision-Language Models under Transfer Attacks

> Research project evaluating the robustness of transformer-based image captioning models under white-box and cross-architecture transfer attacks, with comparison of Base, LoRA fine-tuned, and Full fine-tuned GIT-Large models.

---

## Overview

Vision-Language Models (VLMs) achieve strong performance on image captioning tasks, but their robustness under adversarial perturbations remains an important research challenge. Small perturbations added to input images can influence model predictions and reduce reliability in real-world applications.

This project investigates how adversarial perturbations generated on a surrogate CNN model transfer to a transformer-based captioning model and evaluates whether parameter-efficient fine-tuning methods improve robustness without requiring full model retraining.

The study includes:

- white-box attacks directly on the captioning model
- transfer attacks generated on a ResNet50 surrogate model
- robustness comparison across Base, LoRA fine-tuned, and Full fine-tuned models
- evaluation using BLEU, METEOR, and BERTScore
- analysis of CNN-to-Transformer adversarial transferability

---

## Research Objectives

- Evaluate robustness of GIT-Large image captioning model under adversarial perturbations
- Compare white-box attacks and cross-architecture transfer attacks
- Study robustness improvements from LoRA fine-tuning vs full fine-tuning
- Analyze how adversarial perturbations influence caption semantics
- understand CNN-to-Transformer adversarial transfer behaviour

---

## Models Evaluated

| Model | Description |
|------|-------------|
| Base GIT-Large | microsoft/git-large without additional fine-tuning |
| LoRA Fine-Tuned | GIT-Large with parameter-efficient LoRA fine-tuning |
| Full Fine-Tuned | GIT-Large with full model fine-tuning |
| ResNet50 | surrogate CNN used to generate transfer attacks |

All fine-tuning uses a mixed training dataset:

- 60% clean images
- 20% FGSM attacked images
- 20% MI-FGSM attacked images

Training dataset:
Flickr8k

Evaluation dataset:
MS-COCO subset (1000 images)

---

## Attack Setting

Two adversarial evaluation settings are used.

### White-box attacks
Adversarial perturbations are generated directly on the GIT-Large model using gradient-based methods.

### Transfer attacks
Adversarial perturbations are generated on ResNet50 and transferred to GIT-Large.  
This simulates realistic black-box scenarios where the attacker does not have access to target model parameters.

---

## Attacks Implemented

### White-box attacks (on GIT-Large)

| Attack | Description |
|-------|-------------|
| FGSM | Fast Gradient Sign Method |
| MI-FGSM | Momentum Iterative FGSM |

epsilon:
0.01

---

### Transfer attacks (ResNet50 → GIT-Large)

| Attack | Description |
|-------|-------------|
| FGSM | single-step gradient sign attack |
| MI-FGSM | momentum iterative attack |
| DI-FGSM | diverse input iterative attack |
| TI-MI-FGSM | translation invariant momentum iterative attack |

epsilon values:
8/255 and 16/255

---

## Fine-Tuning Configuration

### LoRA Fine-Tuning

| Parameter | Value |
|----------|-------|
| base model | microsoft/git-large |
| trainable parameters | ~1.8M |
| LoRA rank | 8 |
| LoRA alpha | 16 |
| target layers | q_proj, v_proj |
| learning rate | 5e-5 |
| batch size | 8 |
| epochs | 5 |
| mixed precision | fp16 |

---

### Full Fine-Tuning

| Parameter | Value |
|----------|-------|
| base model | microsoft/git-large |
| trainable parameters | ~347M |
| learning rate | 5e-5 |
| batch size | 8 |
| epochs | 5 |
| mixed precision | fp16 |

---

## Evaluation Metrics

Caption outputs are compared against ground truth captions using:

BLEU
- measures n-gram overlap between generated caption and reference caption

METEOR
- measures semantic similarity using stemming and synonym matching

BERTScore
- measures contextual similarity using pretrained language embeddings

---

## Key Results

### Clean performance

| Model | BLEU-4 | METEOR | BERTScore |
|------|-------|--------|-----------|
| Base | 0.0363 | 0.1499 | 0.8985 |
| LoRA | 0.0224 | 0.1433 | 0.8588 |
| Full FT | 0.0161 | 0.0839 | 0.8509 |

---

### White-box robustness (BLEU-4 drop)

| Model | FGSM drop | MI-FGSM drop |
|------|-----------|--------------|
| Base | 5.8% | 28.9% |
| LoRA | 1.8% | 12.5% |
| Full FT | 1.9% | 11.2% |

Observation:

LoRA and Full Fine-Tuning significantly reduce degradation under iterative attacks.

---

### Transfer attack robustness (ResNet50 → GIT)

| Model | FGSM drop | MI-FGSM drop |
|------|-----------|--------------|
| Base | 2.5% | 2.8% |
| LoRA | 0.0% | 1.3% |
| Full FT | -1.2% | -2.5% |

Observation:

Transfer attacks generated on ResNet50 produce minimal semantic degradation when evaluated on GIT-Large.

This suggests reduced transferability between CNN and transformer architectures.

---

## Key Findings

- transformer-based captioning models show resilience to CNN-generated adversarial perturbations
- LoRA improves robustness while requiring significantly fewer trainable parameters
- full fine-tuning provides strongest robustness but at higher computational cost
- cross-architecture transfer attacks show weaker impact compared to white-box attacks

---

## Repository Structure


```
├── experimental_logs          ← Full experiment logs with all tables
├── README.md                  ← This file
```

---

## Evaluation Metrics

- **BLEU1-4** — n-gram precision overlap between generated and reference captions
- **METEOR** — precision + recall with synonym and stemming support
- **BERTScore F1** — semantic similarity using contextual BERT embeddings

---

## Planned / Future Work

- [ ] Diffusion-based adversarial purification as a pre-processing defense
- [ ] Adaptive adversarial evaluation against defended models
- [ ] Comparison of other PEFT methods (prefix tuning, adapters, DoRA)
- [ ] Full MS-COCO validation set evaluation (5,000 images)

---

## Project Status

| Component | Status |
|---|---|
| White-box attack evaluation | ✅ Complete |
| ResNet50 transfer attack evaluation | ✅ Complete |
| LoRA adversarial fine-tuning | ✅ Complete |
| Full adversarial fine-tuning | ✅ Complete |
| Combined 3-model evaluation | ✅ Complete |
| DI-FGSM / TI-MI-FGSM transfer attacks | ✅ Complete |
|Stronger attack training mixtures | ✅ Complete|
| Diffusion purification defense | 🔲 Planned |

---

## Author

**N. Abhiram Naidu**  
B.Tech Student — Rajiv Gandhi University of Knowledge and Technologies, Nuzvid

---

## Research Disclaimer

This repository represents ongoing academic research. Implementations and results are experimental and subject to change as the work progresses.
