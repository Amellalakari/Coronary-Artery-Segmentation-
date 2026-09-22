Coronary Artery Segmentation from CT Angiography using MONAI


Deep learning pipeline for automated coronary artery segmentation from CT angiography (CCTA), built as a step toward detecting high-risk plaque features that routine clinical review sometimes misses.
Status: In progress — accepted for oral presentation, [Conference Name], Oct 2026.
Motivation
Coronary CT angiography is the standard non-invasive tool for evaluating coronary artery disease. However, certain plaque characteristics with strong prognostic value for future cardiac events — napkin-ring sign, low-attenuation plaque, spotty calcification, positive remodeling — require careful quantitative measurement and are inconsistently captured in routine reads. This project aims to build an automated pipeline that first reliably segments the coronary artery tree, as a foundation for later flagging these high-risk signals directly.
Dataset
ImageCAS — 1,000 coronary CTA volumes with expert-annotated artery segmentation masks, voxel spacing ~0.377 × 0.377 × 0.5 mm.
Current experiments use a 60-patient subset (48 training / 12 validation).
Method
Preprocessing: HU windowing (-100 to 800), intensity normalization, foreground cropping (MONAI)
Class imbalance handling: positive/negative patch sampling (RandCropByPosNegLabeld) — coronary artery voxels represent under 1% of volume
Model: 3D U-Net, patch size 128×128×64
Loss: Dice loss
Hardware: NVIDIA Tesla T4 (Google Colab)
Results
Metric
Value
Training Dice
0.664
Validation Dice*
0.470
Dataset size
60 patients (48 train / 12 val)
\* Excludes validation patches with empty ground-truth masks — see note below.
A note on methodology (read this before the numbers above)
Validation Dice improved across development as follows:
Checkpoint
Validation Dice
Cause
10 epochs
0.044
Initial training
60 epochs
0.210
Extended training
85 epochs
0.274
Further training
85 epochs, rescored
0.470
Excluded empty-mask validation patches
The improvement from 0.044 → 0.274 reflects genuine training progress. The further increase to 0.470 comes from a change in evaluation methodology (excluding patches with no artery present in the ground truth), not additional model learning. Properly resolving this — rather than exclusion — is planned work.
Qualitative results
Successful case — predicted mask closely matches ground truth for a well-defined vessel segment:
Failure mode — the model correctly identifies two vessel branches but misses the thin connecting segment between them (under-segmentation), the main observed failure pattern:
Repository structure
├── notebooks/
│   └── Cardiovascular_attempt_5.ipynb   # Full training & evaluation notebook
├── results/
│   ├── slice53_success.png
│   ├── slice63_undersegmentation.png
│   └── improvement_timeline.png
└── README.md
​
Planned work

Scale training from 60 to the full 200+ patient cohort

Properly resolve validation handling for empty-label cases (rather than exclusion)

Improve segmentation on small branch vessels (extended training, augmentation)

Plaque characterization stage (calcified / non-calcified / mixed)

High-risk feature flagging against established radiological criteria

Baseline comparison against nnU-Net
Author
[Your Name] — [your email or LinkedIn]
References
Zeng, Q. et al. ImageCAS: A Large-Scale Dataset and Benchmark for Coronary Artery Segmentation based on CT. Computerized Medical Imaging and Graphics, 2023.
Isensee, F. et al. nnU-Net: A Self-Configuring Method for Deep Learning-Based Biomedical Image Segmentation. Nature Methods, 2021.
Cardoso, M.J. et al. MONAI: An Open-Source Framework for Deep Learning in Healthcare Imaging. arXiv, 2022.
