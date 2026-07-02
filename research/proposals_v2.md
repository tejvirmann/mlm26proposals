# MLM26 Project Proposals — v2

*Four proposals (one each: bioelectricity · neuromodulation · genomics · harness), formatted like the ML-X-Nexus "Brain-to-Text '25" example. A quality dataset & benchmark reference list follows at the end.*

**Selection criteria applied:** (1) a real, Kaggle-uploadable primary dataset, (2) published evidence it's a genuine open problem, (3) a narrow predictive task with a measurable metric + held-out test, (4) fits 9 weeks, (5) exciting enough to recruit a team, (6) the target professor personally benefits — ideally from *their own lab's data*.

---
---

# 1. The Anatomical Compiler v0: Learning the Bioelectric → Anatomy Map

Tags: Projects · ML Marathon · MLM26 · Regression · Surrogate models · Scientific ML · Bioelectricity · Morphogenesis
Author: Tejvir Mann · Draft: 2026-07-01

Bioreactors and ion-channel-modulating drugs already let biologists *write* to a tissue's bioelectric state — but we can't yet *predict* what anatomy a given bioelectric intervention will produce. That missing forward model is the core bottleneck on the way to Michael Levin's "anatomical compiler": a system that takes a target anatomy and returns the bioelectric intervention that builds it. This challenge trains the first open ML surrogate of that map, so that an expensive biophysics simulation (or a real experiment) can be replaced by a fast, differentiable model.

## Challenge design
- **Task:** Regression — given an ion-channel / gap-junction perturbation configuration, predict the resulting steady-state bioelectric pattern (per-cell Vmem map) and a coarse morphological-outcome label.
- **Domain:** Developmental bioelectricity / morphogenesis / scientific ML.
- **Data:** A dataset **generated with BETSE** (Bioelectric Tissue Simulation Engine — Pietak & Levin, open-source BSD), sweeping ion-channel conductances, gap-junction coupling, and boundary conditions to produce tens of thousands of (config → Vmem pattern → outcome) triples. Anchored/validated against **PlanformDB** (Levin's curated database of >1,000 real planarian regeneration experiments). *Access: 🟢 — you generate and release it; nothing like it exists on Kaggle today.*
- **Methods:** MLP / CNN / graph-neural-net surrogates on the cell graph; a diffusion or cVAE model for the inverse (compiler) bonus track.

## Why it matters
Levin's lab has publicly stated that the lack of large, standardized bioelectric-profiling datasets is a primary barrier to the field. There is **no "Brain-Score for bioelectricity."** A working forward surrogate is literally the engine an anatomical compiler needs, and it turns a slow physics loop into something you can optimize through. The hackathon deliverable — a released dataset + a benchmarked surrogate — is a first, concrete rung on the ladder to targeted bioelectric medicine (regeneration, birth defects, cancer normalization).

## Benchmark & metric
- **Primary metric:** RMSE / Pearson r between predicted and true Vmem pattern on held-out configurations; macro-F1 for the outcome-class head (e.g., normal / two-headed / no-head).
- **Held-out test:** configurations drawn from **unseen regions of parameter space** (extrapolation split), not just random holdout — this is the honest test of a compiler.
- **Baseline:** nearest-neighbor in parameter space + a small MLP; publish its score as the number to beat.

## Datasets to integrate (bonus / secondary track)
- **Inverse-design track:** predict the perturbation that yields a *target* pattern; score by re-simulating the prediction in BETSE and measuring distance to target.
- **PlanformDB** real-experiment validation set; **voxcraft-sim / Evolution Gym** morphology→function data for a cross-substrate generalization probe.

## 9-week timeline
- **Weeks 1–2:** stand up BETSE sweeps, define config schema, generate + QC dataset, upload to Kaggle.
- **Weeks 3–5:** release baseline + open leaderboard; teams train forward surrogates.
- **Weeks 6–8:** inverse-design bonus track; PlanformDB validation.
- **Week 9:** final eval, dataset/tool release, writeup.

## Professor hook
Directly serves Michael Levin's "reverse-engineer the worm" / anatomical-compiler program, uses **his own lab's simulator (BETSE) and database (PlanformDB)**, and produces a reusable open asset his field currently lacks. Strong basis to invite him as advisor/co-organizer.

## Links & resources
- BETSE: [github.com/betsee/betse](https://github.com/betsee/betse) · [BETSE paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC4933718/)
- PlanformDB: [planform.daniel-lobo.com](http://planform.daniel-lobo.com) · [paper](https://academic.oup.com/bioinformatics/article/29/8/1098/229314)
- Workshops/libraries: Intro to PyTorch (GNNs/CNNs); PyTorch Geometric; scientific-ML / surrogate-modeling primer.

---
---

# 2. Dialing the Vagus: Predicting Therapy vs. Side-Effect from Nerve-Stimulation Parameters

Tags: Projects · ML Marathon · MLM26 · Regression · Time-series · Signal processing · Neuromodulation · Bioelectronic medicine
Author: Tejvir Mann · Draft: 2026-07-01

Vagus nerve stimulation (VNS) treats epilepsy, depression, and inflammation — but choosing stimulation parameters is still trial-and-error, and the same current that helps the heart can trigger throat/laryngeal side-effects. This challenge asks teams to predict, from stimulation parameters and the evoked nerve response, both the **therapeutic effect** (change in heart rate) and the **off-target effect** (neck/laryngeal EMG) — the core computation behind personalized, closed-loop bioelectronic medicine. It is built on **Kip Ludwig's own publicly released SPARC recordings**, on which, to our knowledge, no ML benchmark yet exists.

## Challenge design
- **Task:** Multi-output regression — predict Δheart-rate (therapy) and laryngeal/neck EMG magnitude (side-effect) from {stimulation amplitude, frequency, pulse width, burst pattern} + evoked compound nerve activity.
- **Domain:** Peripheral/autonomic neuromodulation, bioelectronic medicine, physiological time-series.
- **Data:** NIH **SPARC** datasets authored by Ludwig et al. — e.g., *Influence of VNS on cardiac activity in pigs* (30-s windows of vagus signal + ECG per stim), *Pig-specific VNS computational models (6-contact cuff)* with EMG+EKG, and *ECG recordings in mice during VNS*. *Access: 🟡 — public CC-BY on SPARC/Pennsieve; stored as experiment packages, so curation into ML tensors is part of the project (and a contribution).*
- **Methods:** gradient-boosted trees on engineered features; 1D-CNN / temporal models on raw evoked waveforms.

## Why it matters
"Which VNS parameters give therapy without side-effects" is *the* open question in bioelectronic medicine, and personalizing dose from neural biomarkers is active, funded research (SPARC; Ludwig's baroreflex and vagus-mapping grants). A predictive dose→response model is directly usable in closed-loop stimulator design — the hackathon output could plug straight into the field's real workflow.

## Benchmark & metric
- **Primary metric:** RMSE / R² on held-out (parameter, response) pairs, reported separately for the therapy and side-effect targets.
- **Held-out test:** leave-out **unseen parameter combinations** and, for the hard track, **unseen animals** (cross-subject generalization is the real-world requirement).
- **Baseline:** linear dose-response fit + gradient-boosted trees; publish the score to beat.

## Datasets to integrate (bonus / secondary track)
- **Cross-species transfer:** train on pig, test on the mouse-ECG VNS set (or vice-versa).
- Broader SPARC autonomic catalog; pig vagus morphology/histology as anatomical covariates.

## 9-week timeline
- **Weeks 1–2:** pull SPARC packages, align stim-parameter logs to physiological recordings, build the tabular+waveform dataset, upload to Kaggle.
- **Weeks 3–5:** baseline + leaderboard; teams model therapy/side-effect.
- **Weeks 6–8:** cross-animal / cross-species generalization track.
- **Week 9:** final eval + a short "recommended safe-parameter map" deliverable.

## Professor hook
Uses **Kip Ludwig's own SPARC data**, targets his exact research (VNS, baroreflex, closed-loop neuromodulation, electrode safety), and hands him the first ML benchmark on those recordings. Best "impress the professor with his own data" project in the set; natural WITNe advisor ask.

## Links & resources
- SPARC datasets: [Pennsieve #349](https://discover.pennsieve.io/datasets/349) · [#305](https://discover.pennsieve.io/datasets/305) · [ECG-in-mice #375](https://discover.pennsieve.io/datasets/375) · [SPARC portal](https://sparc.science/)
- Workshops/libraries: Intro to Time-Series / Signal Processing; scikit-learn / XGBoost; PyTorch 1D-CNN.

---
---

# 3. Reading the Genome's Hidden Structures: Predicting Non-B DNA & Instability from Sequence

Tags: Projects · ML Marathon · MLM26 · Classification · Genomics · Sequence models · Cancer biology
Author: Tejvir Mann · Draft: 2026-07-01

Most of the genome is assumed to be the classic B-form double helix, but sequences can fold into **non-B structures** — G-quadruplexes, cruciforms, hairpins — that drive replication stress, mutation, and cancer. The Georgakopoulos-Soares lab has catalogued these at unprecedented scale across the tree of life. This challenge asks teams to predict, directly from sequence, whether a locus forms a non-B structure and how much genomic instability it carries — a clean, large-scale, high-impact sequence-to-function problem.

## Challenge design
- **Task:** Binary/multiclass classification (± regression) — from a DNA sequence window, predict non-B structure formation (G-quadruplex / cruciform / none) and/or an instability score.
- **Domain:** Computational genomics, cancer mutagenesis, sequence modeling.
- **Data:** Georgakopoulos-Soares lab databases — **Quadrupia** (~140M G-quadruplexes across ~108k genomes) and **invertiaDB** (30M inverted repeats / cruciforms across 118k genomes), with negative sets sampled from matched genomic background. *Access: 🟢 — downloadable; among the largest clean labeled datasets available to this hackathon.*
- **Methods:** k-mer / CNN sequence classifiers; DNA language-model fine-tuning (e.g., Nucleotide Transformer / HyenaDNA embeddings).

## Why it matters
Non-B DNA → genomic-instability mapping is central to understanding cancer genome evolution, and predicting structure from sequence at scale is unsolved. Because the labels are large and cross-species, the leaderboard has a natural hard mode (**generalize to held-out organisms**), and the output — a sequence→instability predictor — is directly useful for variant interpretation and cancer-hotspot prioritization.

## Benchmark & metric
- **Primary metric:** AUROC / AUPRC (class-imbalanced), with a cross-species held-out split.
- **Held-out test:** entire **held-out genomes/clades** — tests real generalization, not memorization of one species' motifs.
- **Baseline:** k-mer logistic regression + a small CNN; publish the score to beat.

## Datasets to integrate (bonus / secondary track)
- **MPRAbase** (17.7M regulatory elements) for a "does structure affect regulatory activity?" bonus.
- **Microsatellites Explorer** / **kmerDB** for additional non-B classes and features; ENCODE/Ensembl annotations.

## 9-week timeline
- **Weeks 1–2:** assemble positive/negative sets from Quadrupia + invertiaDB, define splits, upload to Kaggle.
- **Weeks 3–5:** baseline + leaderboard; teams train sequence classifiers.
- **Weeks 6–8:** DNA-LM fine-tuning; cross-species generalization track.
- **Week 9:** final eval + feature-importance / motif writeup.

## Professor hook
Built entirely on **Ilias Georgakopoulos-Soares's own databases**, which his lab explicitly publishes for community ML use — strong incentive to advise. Cleanest big-data option in the set and beginner-friendly on-ramp (tabular/k-mer) with an advanced ceiling (DNA language models).

## Links & resources
- Databases: [lab index](https://www.georgakopoulos-soares-lab.com/databases) · [Quadrupia](https://github.com/Georgakopoulos-Soares-lab/Quadrupia) · [invertiaDB](https://academic.oup.com/nar/article/53/8/gkaf329/8118974)
- Workshops/libraries: Intro to NLP / sequence models; HuggingFace; HyenaDNA / Nucleotide Transformer.

---
---

# 4. NeuroAgent-Bench: An Open Harness & Benchmark for AI Agents that Run Neuroscience Workflows

Tags: Projects · ML Marathon · MLM26 · LLM agents · Benchmarking · Tooling · Neuroscience infrastructure
Author: Tejvir Mann · Draft: 2026-07-01

Neuroscience analysis is bottlenecked by human labor: loading messy BIDS/NWB data, preprocessing in MNE, fitting a decoder, computing a metric, writing it up. "opencode for neuroscience" would be an AI agent that does these workflows end-to-end — but there's **no standard way to measure whether such an agent actually works**. This project builds both halves: a lightweight **agent harness** wired to real neuro tools, and **NeuroAgent-Bench**, a SWE-bench-style suite of realistic analysis tasks with programmatic graders.

> **Format note:** this is a *tooling + benchmark* project, not a single-stage Kaggle leaderboard. It fits MLM26 best as the "second project / lab-tool track." The competition-legal core is the graded task-suite (below); the harness is the deliverable teams and labs keep.

## Challenge design
- **Task:** Given a natural-language analysis request + a dataset, the agent must produce the correct numerical/artifact answer (e.g., "decode left-vs-right imagery from this EEG set and report held-out accuracy"). Scored by **task success rate** against gold graders.
- **Domain:** LLM agents, research tooling, reproducible neuro-analysis.
- **Data:** A curated task suite built on public datasets — PhysioNet EEG Motor Movement/Imagery, Sleep-EDF, THINGS-EEG2, a SPARC VNS set, an OpenNeuro fMRI set — each task packaged with a fixed input, a hidden held-out portion, and an automatic grader. *Access: 🟢 — all component datasets are public; the contribution is the task packaging + graders + harness.*
- **Methods:** an agent loop (tool-use over MNE-Python, Nilearn, scikit-learn, NWB/BIDS loaders); models compared under a fixed toolset.

## Why it matters
Agentic coding tools are transforming software but barely touch scientific workflows, partly because **there's no benchmark to trust them on**. A public NeuroAgent-Bench + harness would (a) let every lab above evaluate/adopt agent assistance safely, (b) quantify the real speedup ("raw dataset → first result" wall-clock), and (c) be a genuinely novel, portfolio-grade contribution. It's also the most *exciting* recruiting pitch — students get to build AI agents, not just fit models.

## Benchmark & metric
- **Primary metric:** task success rate across held-out tasks (exact-match / tolerance on the numeric answer or artifact check).
- **Secondary metric:** wall-clock **time-to-first-correct-result** vs. a human baseline; robustness = success on **held-out datasets the agent never saw during harness tuning**.
- **Baseline:** a scripted (non-agent) pipeline + a naive single-prompt LLM; publish both.

## Datasets to integrate (bonus / secondary track)
- Add tasks from any lab in this doc (a "Levin BETSE analysis" task, a "PCI computation" task) to grow the suite — the benchmark is designed to be extended.

## 9-week timeline
- **Weeks 1–3:** build harness (tool adapters) + 8–12 seed tasks with graders.
- **Weeks 4–6:** baseline agents + scripted/naive baselines; publish leaderboard.
- **Weeks 7–8:** hardening, held-out task set, wall-clock study.
- **Week 9:** release harness + benchmark + report.

## Professor hook
Every advisor in this document is a potential *user and task-contributor*: the harness is lab-agnostic infrastructure, and each lab's workflow becomes a benchmark task. Positions the team as owners of shared neuroscience tooling — the "second project" your collaborator floated.

## Links & resources
- Libraries: MNE-Python, Nilearn, NWB/BIDS, scikit-learn; agent frameworks (Claude Agent SDK / tool-use).
- Prior art to cite: SWE-bench (software agents); EEGDash (ML on public neuro data).

---
---

# Appendix — Quality Datasets & Benchmarks (reference list)

## A. The richest brain datasets ("data of the human mind" and beyond)

| Dataset | Why it's rich | Modality | Access |
|---|---|---|---|
| **CNeuroMod (Courtois NeuroMod)** | **Deepest individual human data:** 100+ h fMRI *per subject* (Friends, movies, video games). Basis of Algonauts 2025 | 3T fMRI, naturalistic | 🟢 |
| **Natural Scenes Dataset (NSD)** | 8 subjects, ~73k COCO images, 7T ultra-high-field; co-authored by UW's WISC group | 7T fMRI, vision | 🟢/🔵 |
| **Brain Treebank** | Human **intracranial (SEEG)**, ms-resolution, during naturalistic movies + aligned language | iEEG, language | 🟢 |
| **MICrONS "cortical mm³"** | **Richest overall:** co-registered EM connectome (200k cells, **523M synapses**) + 2-photon function of ~75k of the same neurons watching movies | EM + calcium | 🟢 |
| **IBL Brain-Wide Map** | 621k neurons, 279 regions, 139 mice, standardized decision task | Neuropixels | 🟢 |
| **Allen Brain Observatory** | Large standardized Neuropixels + 2-photon visual-coding sets | Ephys + calcium | 🟢 |
| **THINGS (EEG2 / fMRI / MEG)** | Same object-image ontology across 3 modalities; established decoding benchmark | EEG/fMRI/MEG | 🟢 |

**Which is richest?** For the *human mind specifically*, **CNeuroMod** (unmatched per-subject depth). For neuroscience overall, **MICrONS** (structure + function co-registered on the same neurons). For *high-temporal-resolution naturalistic human*, **Brain Treebank**.

## B. Established ML benchmarks (prior art — what already exists)

| Benchmark | Task / metric |
|---|---|
| **Algonauts 2025** | Predict fMRI (1,000 parcels) from multimodal movies; Pearson r, OOD leaderboard |
| **THINGS-EEG2** | Zero-shot 200-way image retrieval (top-k) |
| **Brain-Score** | ANN ↔ primate visual cortex predictivity |
| **Neural Latents Benchmark** | Latent models of spiking; co-smoothing |
| **FALCON** | Few-shot *stable* iBCI decoding across held-out sessions |
| **Sensorium** | Predict mouse V1 responses to images/video |
| **PCIst (PCI)** | Consciousness-complexity index vs. wake/unconscious cutoff (code 🟢, corpus 🔴) |
| **Open Problems in Single-Cell** | 12 tasks, 81 datasets, 171 methods (Krishnaswamy-adjacent) |

**Open lanes with no established benchmark (where you'd *create* one):** peripheral/autonomic neuromodulation dose→response (Proposal 2) and bioelectric morphogenesis (Proposal 1).

## C. Lab-owned datasets by professor (the "their own data" plays)

- **Kip Ludwig →** SPARC/Pennsieve VNS datasets (#349, #305, #375, + vagus anatomy) — public, no ML benchmark yet. *Untapped.*
- **Michael Levin →** BETSE (simulator), PlanformDB (>1,000 planarian experiments), xenobot behavioral data.
- **Georgakopoulos-Soares →** 7 databases: Quadrupia (140M G4), invertiaDB (30M), MPRAbase (17.7M), Microsatellites Explorer, kmerDB, metagRoot, Darling v2.0.
- **Smita Krishnaswamy →** PHATE/MAGIC/MIOFlow/SAUCIE + single-cell datasets (best as *advisor* for a methods challenge).
- **WISC (Tononi/Cirelli/Boly) →** open PCIst code + public anesthesia/sleep EEG (safe track); flagship PCI/TMS-EEG corpus is gated (partnership needed).

## D. Supporting public datasets (to satisfy "integrate more data")

- **Neuromodulation:** PD-rat DBS ephys+behavior (Sci Data); human STN-LFP on OpenNeuro; broader SPARC catalog.
- **Consciousness/EEG:** anesthesia EEG (propofol/xenon/ketamine); Sleep-EDF; CHB-MIT; PsiConnect (ds006110); DMT EEG-fMRI.
- **Vision decoding:** THINGS-fMRI/MEG, Alljoined-1.6M, NSD.
- **Bioelectricity:** voxcraft-sim + Evolution Gym (simulated morphology↔function).
- **Genomics:** all 7 Georgakopoulos-Soares DBs cross-integrate; ENCODE/Ensembl for annotations.

---

*Companion docs: [proposals_ranked_shortlist.md](proposals_ranked_shortlist.md) (broad ranked list), [landscape_datasets_benchmarks.md](landscape_datasets_benchmarks.md) (full data landscape).*
