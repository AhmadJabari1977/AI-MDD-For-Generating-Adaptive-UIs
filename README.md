# AI-MDD-For-Generating-Adaptive-UIs
End-to-end implementation of a hybrid AI–MDD pipeline for adaptive UI generation, including feature extraction, template-based rendering, perceptual evaluation, and Selector/Planner mode
# AI-Driven Model-Driven Development for Adaptive User Interfaces

## Overview
This repository presents a reproducible implementation of a hybrid AI–Model-Driven Development (AI–MDD) pipeline for generating intelligent and adaptive user interfaces. The approach combines deterministic template-based generation with data-driven learning components to balance efficiency, reproducibility, and perceptual quality.

The pipeline is designed to address key limitations of traditional UI generation approaches, particularly the high computational cost of exhaustive candidate rendering and the lack of reliable decision mechanisms for runtime adaptation.

---

## Contributions
The main contributions of this work are:

- A hybrid AI–MDD pipeline integrating template-based generation with learning-based decision models.
- A **Selector model** that predicts high-fidelity UI primitives from PIM-derived features.
- A **Planner model** that estimates transformation cost and supports cost-aware strategy selection.
- A reproducible dataset linking platform-independent models (PIMs) with multiple generated UI realizations (PSMs), perceptual metrics, and transformation metadata.
- An evaluation protocol combining automated metrics (SSIM, MS-SSIM, LPIPS) with human judgment.

---

## Pipeline Overview
The framework consists of four main stages:

1. **Data Ingestion & PIM Construction**  
   UI artifacts are collected and transformed into platform-independent models.

2. **Feature Extraction**  
   Structural, textual, and layout features are derived from the PIM representation.

3. **Candidate Generation & Rendering**  
   Multiple UI candidates are generated using template-based primitives and rendered using a headless browser.

4. **Model Learning**  
   - Selector: predicts high-quality UI primitives  
   - Planner: estimates transformation cost and selects execution strategy

---

## Dataset
The dataset is constructed from a WebUI corpus {https://huggingface.co/datasets/biglab/webui-all} and includes:

- PIM representations (JSON format)
- Multiple generated PSM candidates per instance
- Rendered UI screenshots
- Perceptual similarity scores (SSIM, MS-SSIM, LPIPS)
- Transformation metadata (execution time, status)

---

## Results Summary
- Top-3 accuracy (Selector): ~65%
- Planner performance: R² ≈ 0.62
- Rendering cost reduction: ~69% compared to exhaustive baseline
- Minimal loss in perceptual fidelity (ΔSSIM ≈ -0.005)

---

## Reproducibility
The repository includes:

- Full pipeline implementation
- Feature extraction scripts
- Model training procedures
- Evaluation scripts
- Example dataset subset

All experiments can be reproduced using standard Python environments.

---
## Notebook Structure and Cell Description

The notebook `AI-MDD Code.ipynb` provides an end-to-end implementation of the proposed AI–MDD pipeline. For clarity and reproducibility, each section of the notebook is organized into logically structured cells as described below:

---

### 1. Environment Setup
This section initializes the execution environment and imports required libraries (e.g., NumPy, Pandas, Playwright, scikit-image). It also ensures compatibility with fallback configurations in case certain dependencies are unavailable.

---

### 2. Dataset Loading and Inspection
This section loads the processed PIM dataset and associated metadata. It includes basic inspection steps such as:
- Verifying dataset structure
- Checking available UI instances
- Exploring sample records

---

### 3. PIM Parsing and Preprocessing
In this part, platform-independent models (PIMs) are parsed from JSON files. Key operations include:
- Extracting UI elements and roles
- Handling missing or inconsistent fields
- Structuring data for downstream processing

---

### 4. Feature Extraction
This section derives structured features from PIM and HTML representations, including:
- Structural features (e.g., number of elements, images)
- Textual features (e.g., text length statistics)
- Interaction proxies (e.g., clickable elements)
- Layout descriptors

The output is a normalized feature matrix used for model training.

---

### 5. Candidate Generation (Template-Based)
This part generates multiple UI candidates using predefined primitives (e.g., hero, grid, list). It relies on template-based generation to ensure:
- Reproducibility
- Structural consistency
- Safe UI construction

---

### 6. Headless Rendering
Generated UI candidates are rendered using a headless browser (Playwright). This step:
- Captures screenshots
- Ensures consistent viewport settings
- Logs rendering status and timing

---

### 7. Perceptual Metrics Computation
This section computes visual similarity metrics between generated UIs and reference layouts:
- SSIM (primary metric)
- MS-SSIM (if available)
- LPIPS (optional)

Fallback mechanisms are used if certain libraries are unavailable.

---

### 8. Dataset Construction (Hybrid Dataset)
This stage aggregates all generated artifacts into a unified dataset, including:
- PIM
- Generated PSM candidates
- Screenshots
- Perceptual scores
- Transformation metadata

---

### 9. Selector Model Training
This section trains the Selector model, which predicts the most suitable UI primitive based on extracted features. It includes:
- Data preprocessing
- Model training (Random Forest / LightGBM)
- Evaluation using Top-K accuracy

---

### 10. Planner Model Training
The Planner model is trained to predict transformation cost. This section includes:
- Regression model training
- Evaluation using MAE, RMSE, and R²
- Analysis of prediction stability

---

### 11. Evaluation and Policy Simulation
This section evaluates different generation strategies:
- Exhaustive rendering
- Selector-only
- Planner + Selector
- Template-only

It compares computational cost and perceptual fidelity.

---

### 12. Visualization and Analysis
This section presents:
- SSIM distributions
- Performance plots
- Comparative analysis of strategies

---

### 13. Reproducibility and Export
Final outputs are saved, including:
- Feature matrices
- Model artifacts
- Evaluation results
- CSV manifests

This ensures that experiments can be replicated consistently.

---
REPRODUCIBILITY AND ARTIFACTS
The work artifact bundle and this paper aim for reproducibility. The provided materials include:
• PIM/PSM JSON schemas and exemplar PIM files for the 2,894 ids used in experiments.
• Generation templates (Jinja2 scaffolds), the template engine, and the generate_primitive_html notebook cells.
• Playwright-based rendering scripts and a multiprocess worker-pool launcher that produce manifest CSVs with per-candidate metrics and timings .
• Notebooks and scripts for feature extraction, Selector/Planner training, evaluation scripts, and the human eval sampling code.
• Environment pins (Conda YAML) and CPU-only fallbacks for deterministic baselines.
Minimal reproduction steps:
1. Clone the artifact repository and install the pinned environment (see env/conda_env.yml).
2. Run pythonreproducibility/check_env.py to verify binary availability (Playwright, optional PyTorch).
3. Execute scripts/render_candidates.py--manifestids.txt--primitivesall to (re)generate candidates and manifests.
4. Run notebooks/analysis/compute_metrics.ipynb to compute SSIM/MS-SSIM/LPIPS (LPIPS optional).
5. Run notebooks/models/train_selector_planner.ipynb to reproduce selector/planner experiments and notebooks/analysis/evaluate/policies.ipynb for cost simulations.
All CSVs, random-seed hashes and a small sample dataset for quick checks are included in the artifact bundle.

Optional extended description

This commit includes the full set of artifacts generated from executing the AI–MDD pipeline for intelligent UI generation. The outputs reflect all major stages of the pipeline, from data preprocessing to evaluation and selection.

Included artifacts:

Processed PIM data
Structured platform-independent models extracted from raw UI representations (e.g., WebUI dataset), serving as the foundation for downstream transformations.
Feature datasets (features.csv, extended_features.csv)
Preprocessed and normalized feature matrices capturing structural, visual, and interaction characteristics used for training the Selector model.
Transformation metadata (transform_metadata.csv)
Detailed logs of transformation steps, execution times, and configuration parameters used during candidate generation.
Generated UI candidates (generated_code_hybrid/)
Multiple UI implementations produced via hybrid template-based generation, including intermediate and final HTML outputs.
Rendering and evaluation results (ssim_scores_pw_batch.csv, ssim_merged_fixed.csv)
Perceptual similarity scores (SSIM and related metrics) computed using headless rendering (Playwright) to assess visual fidelity against ground truth.
Selection outputs (candidates_best_per_id.csv, selection_manifest_predicted.csv)
Final selected UI candidates per instance, based on model predictions and ranking strategies.
Statistical analysis results (ssim_correlations.csv, figures)
Correlation analysis and statistical summaries evaluating relationships between UI complexity, transformation cost, and visual quality.
Model artifacts (selector_centroid_model.npz, metadata)
Trained Selector model (fallback centroid-based) and associated configuration used for primitive prediction.

Purpose:

These outputs enable:

Reproducibility of the experimental pipeline
Evaluation of UI generation quality and efficiency
Training and validation of AI models (Selector & Planner)
Further analysis and benchmarking of adaptive UI generation

├── processed_pim/                 # Generated PIM models
├── webui_dataset/                # Source dataset (partial / references)
├── features.csv                  # Base feature matrix
├── extended_features.csv         # Enriched features
├── transform_metadata.csv        # Transformation logs
├── generated_code_hybrid/        # Generated UI candidates
│   ├── candidates/
│   ├── tmp_*/
├── ssim_scores_pw_batch.csv      # Raw SSIM scores
├── ssim_merged_fixed.csv         # Cleaned evaluation results
├── ssim_correlations.csv         # Statistical analysis
├── candidates_best_per_id.csv    # Best candidate per instance
├── selection_manifest_predicted.csv # Model predictions
├── selector_centroid_model.npz   # Trained fallback model
├── selector_metadata.json        # Model configuration
├── pw_batch.py                   # Rendering & SSIM script
├── FinalWorks.ipynb              # Main experimental notebook

## Citation

If you use this work, please cite:

@misc{aljabari2026aimdd,
  author = {Ahmad Aljabari},
  title = {AI-MDD-For-Generating-Adaptive-UIs},
  year = {2026},
  howpublished = {https://github.com/AhmadJabari1977/AI-MDD-For-Generating-Adaptive-UIs}
}
