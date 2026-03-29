# Enhanced Human Action Recognition from Synthetic UAVs Data Using Landmark Extraction and Mamba-based Model

## Overview
HARLEM is a robust, end-to-end Human Action Recognition (HAR) pipeline optimized for in-the-wild video data. This repository contains the official implementation of our research, which introduces a three-stage architecture: automated subject tracking and centralization, spatiotemporal skeletal landmark extraction, and deep sequence classification.

This work was officially accepted and published in the proceedings of the International Symposium on Integrated Uncertainty in Knowledge Modelling and Decision Making (IUKM 2025).

## Pipeline Architecture

<p align="center">
  <img width="671" alt="Pipeline Architecture" src="https://github.com/user-attachments/assets/3d28d365-2b31-4f0b-926e-592aa49b92da" />
</p>

The system is modularized into three core components, corresponding to the provided implementation files:

### 1. Automated Zooming and Centralization (`Auto_zooming.ipynb`)

<p align="center">
  <img width="800" alt="Auto zooming system" src="https://github.com/user-attachments/assets/dfaa828d-fbd5-4acf-b99a-19d397967e70" />
  <br>
  <i>Fig 1. Automated Zooming System using YOLOv5.</i>
</p>

To mitigate background noise, camera jitter, and scale variations, this module utilizes a YOLOv5-based 2-pass processing algorithm.
* **Pass 1:** Scans the entire temporal sequence to determine the maximum global bounding box dimensions occupied by the subject.
* **Pass 2:** Tracks the subject using a custom Moving Average filter (`window_size = 5`) to smooth spatial coordinates. The Region of Interest (RoI) is then dynamically cropped and resized to a fixed `480x800` resolution, ensuring aspect ratio consistency without mechanical viewport jitter.

<p align="center">
  <img width="600" alt="Before & After Zooming" src="https://github.com/user-attachments/assets/9c8a0764-710d-4978-b4f5-66c4f34282b7" />
  <br>
  <i>Fig 2. Before & After Zooming process.</i>
</p>

### 2. Skeletal Landmark Extraction (`landmarks_extraction.ipynb`)
Operating on the stabilized, centralized video output from the previous stage, this module extracts structural human pose data. By converting raw RGB pixel data into structured sequences of spatiotemporal skeletal joints and angles, the system drastically reduces computational dimensionality while preserving critical motion dynamics required for action modeling.

<p align="center">
  <img width="600" alt="Skeleton-based human keypoints" src="https://github.com/user-attachments/assets/504b4faf-ecc9-46b6-9d2d-693b854bd766" />
  <br>
  <i>Fig 3. Skeleton-based human keypoints.</i>
</p>

### 3. Action Recognition Modeling (`HAR-LEM.ipynb`)

<p align="center">
  <img width="720" alt="4-Head Mamba Classification Model" src="https://github.com/user-attachments/assets/e9a6299d-2a61-4973-bc5e-f4f5bb66e19c" />
  <br>
  <i>Fig 4. The 4-Head Mamba Classification Model categorizes sequences into 7 base classes of the RoCoG-v2 dataset, plus an 'Unrecognized' class.</i>
</p>

The final classification is performed using a PyTorch-based deep learning architecture trained entirely on the extracted skeletal sequences.
* Integrated with **Weights & Biases (WandB)** for real-time tracking of training metrics (loss, accuracy).
* Implements dynamic checkpointing to automatically preserve optimal model weights based on validation accuracy thresholds.

## Repository Structure
```text
.
├── Auto_zooming.ipynb          # YOLOv5 tracking and 2-pass bounding box extraction
├── landmarks_extraction.ipynb  # Skeletal feature and spatial angle extraction
├── HAR-LEM.ipynb               # PyTorch model training, validation, and evaluation
└── README.md
```

## Experimental Results

<p align="center">
  <img width="800" alt="Table Comparison" src="https://github.com/user-attachments/assets/3e94e30a-7ab1-43d6-a357-13537baba0b2" />
  <br>
  <i>Table 1. Comparison of our method and baselines on the RoCoG-v2 dataset.</i>
</p>

<p align="center">
  <img width="600" alt="Accuracy curve" src="https://github.com/user-attachments/assets/a277a04f-4953-4e6d-b452-9cb2d76b3e5a" />
  <br>
  <i>Fig 5. Accuracy curve over training steps (Peak test accuracy: 57.59%).</i>
</p>

## Citation
If you find this codebase or our research methodology helpful in your work, please consider citing our paper:

```bibtex
@inproceedings{harlem2025,
  title={Enhanced Human Action Recognition from Synthetic UAVs Data Using Landmark Extraction and Mamba-based Model},
  author={Tri, N. Q. and Anh, L. H. and Vu, L. Q. and Khoa, T. Q. and Hung, P. D.},
  booktitle={Proceedings of the International Symposium on Integrated Uncertainty in Knowledge Modelling and Decision Making (IUKM)},
  year={2025}
}
```
