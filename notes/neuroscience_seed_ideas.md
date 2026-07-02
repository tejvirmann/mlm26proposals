# MLM26 Project Brainstorm: Neuroscience Harnesses, Benchmarks & Datasets

*Compiled from planning discussion — ML Marathon 2026 (ml-marathon.wisc.edu)*

---

## Key Logistics (as of July 1, 2026)

- **Interest form deadline: July 1, 2026** — submit ASAP to reserve a challenge slot.
- Data must be uploadable to Kaggle by **Aug 1**; full draft (problem description, data, metric, sample submission) due **Aug 10**.
- Kaggle's free competition format only supports a **single-stage leaderboard** — no multi-phase competitions.
- Every challenge needs **one clearly defined predictive task** (classification/regression/detection) with a **measurable metric** and a **held-out test set**. Open-ended "explore the dataset" goals are explicitly disallowed.
- External/additional datasets can be encouraged as a **bonus/secondary track**, but the primary leaderboard metric must stay narrow and well-defined.

---

## Existing Neuroscience Benchmarks & Harnesses (so we don't reinvent)

| Name | What it does |
|---|---|
| **NWB (Neurodata Without Borders)** | Standard format for neurophysiology data (ephys, optical imaging, behavior); ecosystem includes data management, analysis, and archive tools (DANDI Archive). |
| **BIDS (Brain Imaging Data Structure)** | Standard for MRI/EEG/MEG data; backbone of OpenNeuro. |
| **ONE (Open Neurophysiology Environment)** | Adopted by the International Brain Laboratory (IBL) for standardized access across a shared Neuropixels dataset. |
| **MNE-Python** | De facto EEG/MEG analysis toolkit (filtering, epoching, source localization, connectivity, topomaps). |
| **Brainlife.io** | Cloud pipeline harness that runs standardized processing across multiple neuroimaging modalities. |
| **Brain-Score** | Benchmarks how well ANN models predict primate visual cortex responses. |
| **Neural Latents Benchmark (NLB)** | Evaluates latent variable models on neural spiking activity across brain areas/tasks; standardized held-out splits; "co-smoothing" metric (predicts firing rates of held-out neurons). |
| **Algonauts / Sensorium / Dynamic Sensorium** | Evaluate encoding models predicting brain activity (fMRI / mouse visual cortex) from visual stimuli. |
| **FALCON** | Few-shot benchmark for robust BCI decoding stability over time. |

**The gap:** none of these cover bioelectricity, morphogenesis, regeneration, or basal cognition (Levin's domain). There is currently no "Brain-Score for bioelectric pattern → anatomical outcome." This is a real, uncrowded opportunity.

---

## Data Situation for Levin-Style (Bioelectricity/Morphogenesis) Work

**Honest state of the field:** Levin's own lab has written that a lack of large, standardized "physiomic profiling" datasets (bioelectric state across many cells/tissues) is a known barrier. There is no ready-made, Kaggle-ready bioelectric dataset.

Two viable paths:

### 1. Xenobot simulation (ready now, no lab partnership needed)
- **voxcraft-sim** — GPU-accelerated, open-source voxel-based physics engine used in actual xenobot design (elastic voxels, tunable material properties).
- **Evolution Gym (evogym)** — open-source voxel-based soft-body simulator with benchmark tasks.
- Real behavioral data from the original xenobot team: `github.com/swarm-lab/xenobots`.
- **Project:** train a surrogate model to predict a xenobot morphology's task performance (e.g., locomotion distance) directly from its voxel structure — skips expensive physics simulation, accelerates evolutionary search loops used in real xenobot design. Clean regression task, held-out test = unseen morphologies, metric = RMSE/correlation against true simulated fitness.

### 2. Bioelectric imaging data (exists, but scattered)
- Published across many individual papers (planarian Vmem/DiBAC4(3) imaging, Xenopus regeneration studies) — not aggregated into one ML-ready set.
- **Project:** curating/standardizing this scattered data into one benchmark dataset would itself be a genuine, valuable contribution to the field — arguably more useful than another one-off model.

---

## UW-Madison Connection: Wisconsin Institute for Sleep and Consciousness (WISC)

This is the strongest local angle available:

- **Giulio Tononi's lab at UW-Madison developed Integrated Information Theory (IIT)** — the theoretical basis for the Perturbational Complexity Index (PCI), the leading clinical measure of consciousness.
- WISC (Tononi, Chiara Cirelli, Melanie Boly) has co-authored work using the **7T fMRI Natural Scenes Dataset** (Vanasse, Boly, Allen, Wu, Naselaris, Kay, Cirelli, Tononi — PNAS 2022) — a large, open, naturalistic-vision fMRI dataset, ideal for an Algonauts-style encoding-model challenge.
- **PCIst (fast PCI) is already open source**: `github.com/renzocom/PCIst` — a Python library for computing the state-transition Perturbational Complexity Index from TMS-EEG or other evoked responses.

### Likely needs (inferred — worth confirming directly with the lab before committing)
- Automating slow-wave/sleep-stage scoring and PCI computation at scale (currently semi-manual, specialist-dependent).
- Cross-subject, cross-site generalization of complexity metrics — PCI's sensitivity to individual calibration is a recurring theme in the literature.

### Recommended harness
Combine **MNE-Python** (loading/preprocessing) + **PCIst** (complexity scoring) + a thin **BIDS ingestion layer** into one pipeline:
- Input: any TMS-EEG or resting EEG dataset in BIDS format.
- Output: standardized report — PCI score, spectral profile, connectivity graph.
- **Benchmark:** does the automated score reproduce the published PCI* = 0.31 wake/unconscious cutoff across anesthesia, sleep, and disorders-of-consciousness datasets, using WISC's own published results as ground truth?

This produces a real, reusable tool a UW-Madison lab could use the day the Marathon ends — not just a leaderboard exercise.

**Suggested next step:** reach out to Tononi/Cirelli/Boly's group about co-organizing or advising a challenge. Given they originated PCI, "your own metric, benchmarked and packaged as a reusable open tool" is a strong, easy pitch.

---

## Consolidated Project Ideas

### A. NeuroBench Harness (general-purpose)
- Standardized pipeline (ingest → preprocess → extract features → decode) that plugs into any NWB/BIDS dataset.
- Benchmark: anchor task on PhysioNet EEG Motor Movement/Imagery dataset; generalization score = average accuracy/F1 across 2–3 additional held-out public EEG/BCI datasets, unmodified.
- Bonus metric: wall-clock time from "raw new dataset" to "first result" (the actual speedup claim, quantified).

### B. Consciousness State Classifier + Complexity Harness
- Leaderboard task: classify EEG epochs into an ordinal consciousness scale (wake / sedated / unconscious / disorders-of-consciousness) using anesthesia or PCI benchmark data. Metric: macro-F1 or ordinal accuracy on held-out subjects.
- Visualization deliverable: per-subject "complexity trajectory" dashboard (PCI/entropy over time, topomap animations, connectivity graphs before/after).
- Bonus generalization track: run the same harness on psychedelics datasets (PsiConnect, DMT EEG-fMRI) to test transfer.

### C. Xenobot Fitness Surrogate Model
- Regression task: predict task performance from voxel morphology using voxcraft-sim/evogym data.
- Directly accelerates real evolutionary design workflows used in xenobot research.

### D. Bioelectric Imaging Aggregator + Predictor
- Curate scattered published Vmem imaging data into one standardized dataset.
- Stretch goal: predict anatomical outcome (head/tail count, eye placement) from early bioelectric pattern.

### E. WISC PCI Automation Harness
- MNE-Python + PCIst + BIDS ingestion → standardized consciousness-complexity report for any EEG dataset.
- Benchmark against WISC's own published wake/unconscious cutoffs.

### F. Other domain ideas (breadth/diversity for submission)
- Hospital readmission/deterioration risk (MIMIC-IV, AUROC).
- Air quality/wildfire smoke forecasting (EPA AirNow + NOAA, RMSE).
- Crop stress/disease detection from drone/satellite imagery (Sentinel-2/PlantVillage, F1/mAP).
- Grid demand forecasting for renewable integration (utility load data, MAPE).
- Literature-grounded RAG assistant for a specific UW research domain (LLM-graded QA accuracy).

---

## Reference Datasets Mentioned

- **PhysioNet EEG Motor Movement/Imagery Dataset** — 109 subjects, executed/imagined motor tasks, open license.
- **Sleep-EDF Expanded** — sleep-stage scoring, PhysioNet.
- **CHB-MIT Scalp EEG** — seizure detection, PhysioNet.
- **OpenNeuro** — thousands of free, CC0-licensed EEG/MEG/fMRI datasets in BIDS format.
- **PsiConnect (OpenNeuro ds006110)** — psilocybin EEG + multimodal MRI, 62 participants.
- **DMT EEG-fMRI dataset (Timmermann et al., PNAS)** — 20 participants, IV DMT vs. placebo.
- **PCI benchmark (Casali et al. 2013)** — 719 TMS/hd-EEG sessions across wakefulness, sleep, anesthesia, disorders of consciousness.
- **Anesthesia EEG complexity dataset** — propofol/xenon/ketamine, dissociates unresponsiveness from unconsciousness.
- **7T fMRI Natural Scenes Dataset (NSD)** — large naturalistic-vision fMRI dataset, used by WISC researchers.
- **IBL Brain-Wide Map** — whole mouse brain Neuropixels recordings during decision-making, accessed via ONE.

---

*Generated for MLM26 challenge planning — UW-Madison ML Marathon.*
