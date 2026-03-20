# Robustness of Image Captioning LLMs Against Adversarial Attacks

> **Active Research** — Evaluating adversarial robustness of GIT-Large image captioning under white-box and transfer attacks, with LoRA and full adversarial fine-tuning as defense strategies.

---

## Research Objective

- Evaluate the robustness of **GIT-Large** under white-box (FGSM, MI-FGSM) and black-box transfer attacks (ResNet50 surrogate)
- Compare two adversarial fine-tuning defense strategies: **LoRA** and **Full Fine-Tuning**
- Measure caption degradation using BLEU1-4, METEOR, and BERTScore F1
- Analyze CNN-to-Transformer adversarial transferability

---

## Models Evaluated

| Model | Description |
|---|---|
| **Base GIT-Large** | `microsoft/git-large` — no fine-tuning |
| **LoRA Fine-Tuned** | GIT-Large with LoRA adversarial fine-tuning (r=8, α=16) |
| **Full Fine-Tuned** | GIT-Large with full adversarial fine-tuning (all 347M params) |

All fine-tuning uses a **60% clean / 20% FGSM / 20% MI-FGSM** training mixture on **Flickr8k**.  
Evaluation is performed on a **1,000-image MS-COCO subset**.

---

## Key Results

### Clean Performance

| Model | BLEU-4 | METEOR | BERTScore |
|---|---|---|---|
| Base GIT-Large | 0.0363 | 0.1499 | 0.8985 |
| LoRA Fine-Tuned | 0.0224 | 0.1433 | 0.8588 |
| Full Fine-Tuned | 0.0161 | 0.0839 | 0.8509 |

### White-Box Attack Robustness (BLEU-4 Drop)

| Model | FGSM Drop | MI-FGSM Drop |
|---|---|---|
| Base GIT-Large | 5.8% | 28.9% |
| LoRA Fine-Tuned | 1.8% | 12.5% |
| Full Fine-Tuned | 1.9% | 11.2% |

### Transfer Attack (ResNet50 → GIT) — Near-Zero Degradation

All three GIT-Large variants show **near-complete immunity** to ResNet50 transfer attacks.  
This is due to the architectural gap between CNNs and vision transformers (patch-based attention).

| Model | FGSM Transfer Drop | MI-FGSM Transfer Drop |
|---|---|---|
| Base GIT-Large | 2.5% | 2.8% |
| LoRA Fine-Tuned | 0.0% | 1.3% |
| Full Fine-Tuned | −1.2% | −2.5% |

### Key Finding

> **LoRA adversarial fine-tuning achieves the best robustness-quality trade-off.**  
> It reduces MI-FGSM degradation by ~57% vs base, while retaining 40% more clean BLEU-4 than full fine-tuning.

---

## Attacks Implemented

### White-Box Attacks (on GIT directly)
| Attack | Type | ε | Notes |
|---|---|---|---|
| FGSM | Single-step | 0.01 | Gradient sign, clamp [-1,1] |
| MI-FGSM | Iterative + momentum | 0.01 | α=0.005, 5 iters |

### Transfer Attacks (crafted on ResNet50, applied to GIT)
| Attack | ε | Notes |
|---|---|---|
| FGSM | 8/255, 16/255 | Single-step on ResNet50 |
| MI-FGSM | 8/255, 16/255 | Iterative, α=0.005, 5 iters |
| DI-FGSM | 8/255, 16/255 | Diverse inputs for better transfer |
| TI-MI-FGSM | 8/255, 16/255 | Translation-invariant MI-FGSM |

---

## Fine-Tuning Configuration

| Parameter | LoRA | Full Fine-Tuning |
|---|---|---|
| Base Model | microsoft/git-large | microsoft/git-large |
| Trainable Params | ~1.8M (0.5%) | ~347M (100%) |
| LoRA Rank | 8 | N/A |
| LoRA Alpha | 16 | N/A |
| Target Modules | q_proj, v_proj | All |
| Learning Rate | 5e-5 | 5e-5 (cosine) |
| Batch Size | 8 | 8 |
| Epochs | 5 | 5 |
| Mixed Precision | fp16 | fp16 |
| Early Stopping | patience=3 | patience=3 |
| Training Platform | Google Colab T4 | Google Colab T4 |

---

## Datasets

| Dataset | Usage |
|---|---|
| **Flickr8k** | Adversarial fine-tuning training (8,092 images, 5 captions each) |
| **MS-COCO subset** (1,000 images) | Evaluation — unseen during training |

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
- [ ] Stronger attack training mixtures (PGD, C&W, AutoAttack)
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
| Diffusion purification defense | 🔲 Planned |

---

## Author

**N. Abhiram Naidu**  
B.Tech Student — Rajiv Gandhi University of Knowledge and Technologies, Nuzvid

---

## Research Disclaimer

This repository represents ongoing academic research. Implementations and results are experimental and subject to change as the work progresses.
