# DAVis-Net: Reliable Retinal OCT Classification

Official repository for the research work: *DAVis-Net: A Dual-Attention Deep Supervision Framework for Reliable OCT Image Classification with Integrated Explainability and Uncertainty Quantification*

---

## 1. Research Problem

Optical coherence tomography (OCT) is central to diagnosing retinal conditions such as choroidal neovascularization (CNV), diabetic macular edema (DME), and drusen-associated age-related macular degeneration. Deep learning models now classify these conditions with high accuracy, but accuracy alone does not establish clinical trustworthiness. A model can be highly accurate on average while still misclassifying individual scans with unwarranted confidence — a risk that matters directly when a missed or delayed diagnosis affects patient outcomes.

## 2. Existing Literature Gaps

The OCT classification literature has largely optimized for benchmark accuracy while underexploring prediction reliability. Three gaps motivate this work:

- **Qualitative explainability.** Grad-CAM and similar attribution methods are widely used but rarely evaluated quantitatively for faithfulness.
- **Isolated uncertainty and conformal methods.** Uncertainty estimation and conformal prediction have each been applied to medical imaging, but their class-specific behavior on OCT data, and whether they overlap with explainability signals, remains unstudied.
- **No integrated framework.** No prior OCT study combines classification, quantitative explainability, uncertainty, calibration, and conformal prediction to test whether these reliability signals are redundant or complementary.

## 3. Proposed Work

**DAVis-Net** (Dual-Attention deep superVision Network) is a VGG16 backbone augmented with two Convolutional Block Attention Modules (CBAM) at Blocks 4 and 5, plus an auxiliary deep-supervision classification head trained with inverse-frequency-weighted focal loss.

Around this architecture, an integrated reliability framework was built, combining:
- **Explainability** — Grad-CAM, Grad-CAM++, ScoreCAM, evaluated quantitatively
- **Uncertainty** — Monte Carlo Dropout (T = 50 passes)
- **Calibration** — post-hoc temperature scaling
- **Conformal prediction** — split conformal prediction sets across multiple coverage targets
- **Joint correlation analysis** — testing whether the above signals are redundant or independent
- **Trust/Refer triage rule** — a composite rule for routing low-reliability predictions to specialist review

## 4. Results

- **Classification:** DAVis-Net achieved 98.02% ± 0.12% accuracy under 5-fold cross-validation, a statistically significant improvement over the VGG16 baseline (97.58% ± 0.22%, p = 0.004).
- **Calibration:** Temperature scaling reduced Expected Calibration Error from 0.0272 to 0.0044 with accuracy fully preserved.
- **Conformal prediction:** At 99% target coverage, empirical coverage of 99.23% was achieved with a mean prediction-set size of 1.058.
- **Joint correlation analysis:** Predictive entropy and conformal set size are strongly redundant (r = 0.67–0.78 across all classes), while Grad-CAM localization quality is orthogonal to both (|r| < 0.08) — establishing that distributional uncertainty and spatial attention correctness capture independent failure modes.
- **Trust/Refer triage:** The composite rule achieved 99.76% accuracy on trusted predictions (73.9% of the test set) and 90.83% accuracy on referred predictions (26.1%), confirming genuine triage capability.

## 5. Summary

DAVis-Net demonstrates that a targeted architectural modification can improve classification performance without sacrificing reliability, while the accompanying joint reliability framework shows that uncertainty and explainability signals are complementary rather than redundant. Together, these findings support a multi-dimensional evaluation approach for medical image classifiers, moving beyond accuracy alone toward a foundation for trustworthy, reliability-aware clinical decision support.

---

## Repository Structure

```
Notebooks/
├── 1_Exploratory_Data_Analysis.ipynb
├── 2_CNN_Backbone_Screening.ipynb
├── 3_Backbone_Architecture_Comparison.ipynb
├── 4_DAVis-Net_Initial_Training.ipynb
├── 5_XAI_UQ_Analysis_P1.ipynb
├── 6_XAI_UQ_Analysis_P2.ipynb
└── 7_XAI_UQ_CP_Correlation_Analysis_RERUN.ipynb

Tables_and_Diagrams_FINAL/
├── DAVis-Net Diagram.png
├── OCT Image.png
├── Trust_Refer-Workflow.png
├── 1_Explainability/       Grad-CAM / Grad-CAM++ / ScoreCAM analysis, quantitative metrics
├── 2_Uncertainty/          MC Dropout entropy, classification report, significance tests
├── 3_Calibration/          Temperature scaling, reliability diagram, calibration metrics
├── 4_Conformal/            Conformal prediction sweep, class-wise ambiguity
├── 5_Joint_Analysis/       Entropy–set size–localization correlation analysis
├── 6_Trust-Refer-Triage/   Triage accuracy and referral-rate results
├── 7_DRUSEN_Error_Analysis/ Three-way DRUSEN outcome analysis
├── 8_Pairing/              Ambiguous prediction-set pairing analysis
└── 9_Others/               Confusion matrix, per-class correlation grid, risk-coverage curves
```

Each subdirectory under `Tables_and_Diagrams_FINAL` follows a consistent layout: `paper/` contains manuscript-ready figures, `raw/` contains per-sample outputs, and `tables/` contains summary CSVs referenced in the paper.

---
