# CNN Face‑Alignment & Cosmetic Editing

This repository provides an open, lightweight pipeline that
* **detects 32 semantic facial landmarks** on portrait images, and
* **applies realistic eye‑ and lip‑recolouring** using only those landmarks.

This project investigates the capabilities of moderately-sized Convolutional Neural Networks (CNNs) when trained on a modest dataset, specifically avoiding reliance on large, pre-trained models. It presents three iteratively refined CNN architectures, complemented by the full training workflow, comprehensive evaluation tools, and a live demonstration of the cosmetic editing functionality.

---
## Why this project?
Precise facial landmark localisation is fundamental to numerous applications, including head-pose estimation, expression capture, and augmented reality filters. While commercial solutions exist, they often lack transparency due to being closed-source. This repository offers a transparent and reproducible alternative, developed with TensorFlow/Keras and OpenCV, showcasing an effective approach built on accessible technologies.

---
## Core Tasks
| Task | Input | Output |
|------|-------|--------|
| **Face Alignment** | 242 × 242 RGB crop | NumPy array `(32, 2)` |
| **Cosmetic Re‑colour** | Image + landmarks | Edited RGB image |

---
## Feature Highlights
| Component | Tech | Purpose |
|-----------|------|---------|
| **Model 1** | 10‑layer 2‑D CNN | Baseline architecture |
| **Model 2** | 6‑layer CNN + dropout + batch‑norm | Leaner, better generalisation |
| **Model 3** | Model 2 + random‑contrast augmentation | Best overall performer |
| **Make‑up Module** | OpenCV drawing ops | Re‑colour lips & irises with alpha‑blend |
| **Evaluation Suite** | NumPy · Matplotlib | CED plots & failure‑mode gallery |

---
## Dataset
* **2 811** aligned face crops (242 × 242) with 32 manual landmarks each.
* Faces pre‑normalised ⇒ no separate face‑detection step required.
* Train/validation split **70 % / 30 %** (stratified).
* Hidden test set (804 images); predictions stored in `results.csv`.

---
## Methodology Overview
1. **Pre‑processing** – images scaled to [0, 1]; optional horizontal flip & contrast jitter.
2. **Architecture search** – explored depth, filter count and dense‑layer width (≈ 260 k params best trade‑off).
3. **Loss / Metric** – mean‑squared error on flattened 64‑D coordinate vector.
4. **Regularisation** – dropout (p = 0.25) and batch‑norm after each conv block.
5. **Augmentation** – small rotations (±10°) and contrast jitter to improve robustness.

---
## Results
| Model | Avg Training Acc | Avg Val Acc | Avg Euclidean Error (px) |
|-------|----------------:|-----------:|-------------------------:|
| **Model 1** | 0.71 | 0.79 | 10.44 |
| **Model 2** | 0.89 | 0.85 | 4.72 |
| **Model 3** | **0.90** | **0.86** | **3.24** |

---
## Limitations & Next Steps
* **Expression variation** – large yaw or wide‑open mouths still degrade accuracy; collect or synthesise such poses.
* **Lighting realism** – cosmetic recolouring ignores scene lighting; an intrinsic‑image or GAN approach would improve realism.

---
## License
All Rights Reserved.

