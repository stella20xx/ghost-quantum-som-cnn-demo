# Ghost & Quantum Imaging with SOM–CNN

Label-free clustering and segmentation of ghost and SPDC-based quantum
imaging data using physics-aware Self-Organizing Maps (SOM) and a
lightweight CNN/U-Net student.

This demo shows how SOM can separate meaningful correlation / visibility
patterns from noise in low-photon ghost imaging and SPDC experiments,
and how the resulting codebooks transfer across different experimental
conditions.

See the full slide deck in
[`docs/ghost_quantum_som_cnn_demo.pdf`](docs/ghost_quantum_som_cnn_demo.pdf).

---

## Problem

- Ghost and quantum imaging experiments (e.g., SPDC-based setups) often
  operate in **low-photon, noisy regimes**.
- Measurements are high-dimensional correlation maps or visibility
  patterns rather than natural images.
- Manual region selection or heavily supervised models are impractical.
- Goal: **cluster and segment quantum/ghost imaging data in a label-free,
  physics-aware way**, robust across different optical conditions.

---

## Data

- **Ghost imaging:** correlation maps / reconstructed ghost images under
  changing photon budgets or noise levels.
- **SPDC quantum imaging:** joint probability / correlation distributions
  under several experimental configurations (e.g., "corrected", "ideal",
  "distorted" optical paths).
- No hand-drawn masks or class labels are used.

---

## Method (SOM → CNN student)

1. **Physics-aware feature extraction**

   Each pixel (or bin) in a correlation / ghost image is mapped into a
   feature vector including:

   - local correlation / visibility value,
   - neighborhood contrast,
   - position (e.g., radial, angular coordinates),
   - simple statistics capturing speckle or fringe structure.

2. **PCA + SOM clustering**

   - PCA reduces the feature dimension while preserving physically
     meaningful variance.
   - A Self-Organizing Map (SOM) is trained on data from one condition
     to form a small number of clusters (e.g., background, correlated
     region, high-visibility core).
   - Cluster centroids (weight vectors) are interpretable in terms of
     correlation level and spatial location.

3. **Cross-condition transfer**

   - The SOM codebook learned under one optical configuration
     (e.g., "ideal") is reused on data from other configurations
     (e.g., "distorted").
   - This yields consistent cluster identities across conditions, making
     changes in visibility or correlation structure easy to quantify.

4. **CNN/U-Net student (optional)**

   - A lightweight CNN/U-Net is trained using SOM labels as supervision.
   - The student provides smooth, pixel-level masks for correlated
     regions, enabling fast inference on large datasets.

Key properties:

- **Label-free:** no manual annotation of correlated vs uncorrelated
  regions is required.
- **Physics-aware:** features and clusters are defined in terms of
  correlation / visibility patterns, not arbitrary textures.
- **Transferable:** SOM centroids are reused across different optical
  configurations and noise levels.

---

## Results (see PDF)

Highlights from `docs/ghost_quantum_som_cnn_demo.pdf`:

- SOM clustering separates high-visibility / correlated regions from
  background and noise in ghost imaging reconstructions.
- SPDC correlation maps from different configurations can be projected
  into a **shared SOM feature space**, enabling direct comparison of
  correlation structure.
- When used as a teacher, SOM labels allow a small CNN/U-Net to learn
  stable masks that generalize across conditions.

---

## Status

This repository currently serves as a **documentation/demo hub** for the
ghost & quantum imaging SOM–CNN pipeline. Code and runnable notebooks
will be added progressively as the framework is cleaned and released.
