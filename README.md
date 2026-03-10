# Relative Classification Accuracy (RCA) for Fine-Grained K-pop Face Generation

## Overview
Denoising Diffusion Probabilistic Models (DDPMs) generate high-fidelity images, but standard metrics like Fréchet Inception Distance (FID) and Inception Score (IS) often fail to measure semantic controllability in specialized, single-domain tasks. 

This project explores **Class-Conditional DDPMs** tailored for fine-grained face generation, specifically focusing on K-pop idols. To address the evaluation gap, we introduce a novel metric: **Relative Classification Accuracy (RCA)**. RCA utilizes an "Oracle" classifier trained on real data to provide a calibrated measure of identity preservation, disentangling image quality from the intrinsic difficulty of fine-grained classification.

## The RCA Metric
Standard classifiers pretrained on general objects (like ImageNet) cannot meaningfully discriminate between distinct human identities. RCA solves this by normalizing the generative model's accuracy against a domain-specific classifier's performance on real test data.

$$\text{RCA}=\frac{\text{Accuracy}_{\text{gen}}}{\text{Accuracy}_{\text{real}}}$$

*   **Accuracy_gen**: The raw classification accuracy on the synthetic dataset.
*   **Accuracy_real**: The validation accuracy of the oracle classifier on the real, ground-truth dataset.

## Dataset
This project utilizes the **KoIn dataset**, specifically the **KoIn10 subset** (10 distinct identities). This dataset presents a unique challenge due to highly similar demographic features, makeup styles, and lighting conditions, resulting in low inter-class variance.

## Methodology
1. **Data Preprocessing:** Images are processed using MTCNN for face detection and alignment, cropped with a 25% margin to preserve contextual cues (like hair), and resized to $32\times32$ and $64\times64$ resolutions.
2. **Generative Framework:** A class-conditional DDPM (utilizing a UNet2DModel architecture) synthesizes the face images based on identity labels.
3. **Semantic Oracle:** A ResNet-34 classifier serves as the semantic oracle for the $32\times32$ experiments, achieving a Top-1 test accuracy of 76.8% on real data.

## Key Results
* **Visual Quality:** The model achieved a global FID score of **8.9294** at $32\times32$ resolution, indicating high perceptual quality.
* **Semantic Consistency:** The global RCA score is **0.27**. The diffusion model preserves approximately 27% of the semantic identity information relative to the real data distribution.
* **Identified Bottlenecks:** Performance constraints are primarily driven by low-resolution limits (loss of high-frequency identity markers), intra-gender ambiguity, and class-specific mode dominance.

## Future Work
* Integration of metric-learning objectives (e.g., ArcFace loss) to enforce larger angular margins between identity embeddings.
* Implementation of a Cascaded Diffusion Model for super-resolution to recover high-frequency facial details.

## Contributors
* Sylvey Lin (MS in Information Management, UIUC)
* Eranki Vasistha (MS in Information Management, UIUC)
