# Robustness-of-image-captioning-LLM
Ongoing research on transfer-based adversarial attacks (FGSM, MI-FGSM) against image captioning models, evaluated using BLEU, METEOR, and BERTScore. Planned extensions include C&amp;W and DeepFool.


# Transfer-Based Adversarial Attacks on Image Captioning Models

🚧 **Active Research – Work in Progress** 🚧

This repository contains an ongoing research project focused on evaluating the robustness of image captioning models under transfer-based adversarial attacks. The objective is to analyze how adversarial perturbations generated on a source model affect caption generation quality when transferred to a different target model.

---

## 📌 Research Objective

- Study robustness of image captioning models under transfer attacks
- Measure caption degradation using language similarity metrics
- Analyze attack transferability between different vision–language models
- Build a reproducible experimental pipeline for robustness evaluation

---

## ✅ Current Implementation

### 🔹 Attack Setting
- Transfer-based adversarial attacks
- Adversarial examples generated on a source model
- Evaluated on a separate target image captioning model

### 🔹 Implemented Attacks
- FGSM (Fast Gradient Sign Method)
- MI-FGSM (Momentum Iterative FGSM)

### 🔹 Evaluation Metrics
- BLEU
- METEOR
- BERTScore

### 🔹 Output Format
- Captions stored in structured JSON files
- Comparison between original and adversarial captions
- Metric-based robustness analysis

---

## 🧪 Repository Structure

├── attacks/
│   ├── fgsm.py
│   └── mifgsm.py
│
├── transfer/
│   ├── source_models.py
│   ├── target_models.py
│   └── transfer_pipeline.py
│
├── evaluation/
│   ├── bleu_meteor.py
│   └── bertscore_eval.py
│
├── results/
│   └── sample_transfer_results.json
│
├── README.md
└── requirements.txt

---

## 🚧 Planned Extensions (Future Work)

The following attacks are planned but not yet implemented:

- Carlini & Wagner (C&W)
- DeepFool
- Additional optimization-based transfer attacks

These extensions will be added after completing large-scale transfer evaluations using FGSM and MI-FGSM.

---

## ⚠️ Project Status

✔ Transfer pipeline implemented for FGSM and MI-FGSM  
✔ Caption evaluation integrated with BLEU, METEOR, and BERTScore  
⚠ Large-scale experiments ongoing  
⚠ Optimization-based attacks under development  

Results, code structure, and conclusions are subject to change as research progresses.

---

## 📚 Research Disclaimer

This repository represents ongoing academic research. The implementations and results are experimental and should not be considered finalized benchmarks.

---

## 👤 Author

N Abhiram Naidu  
B.Tech Student  

---

## 📬 Contact

For research discussions or collaboration, feel free to open an issue in this repository.
