# MLM26 — Final Proposals

*Four project proposals (bioelectricity · neuromodulation · genomics · AI harness), each pressure-tested for data feasibility and formatted like the ML-X-Nexus "Brain-to-Text '25" example. Companion background: [research/proposals_v2.md](research/proposals_v2.md), [research/landscape_datasets_benchmarks.md](research/landscape_datasets_benchmarks.md).*

---

## Pressure-test summary (read this first)

| # | Proposal | Feasibility | Biggest risk | Fix applied |
|---|---|---|---|---|
| 1 | **Anatomical Compiler v0 (BETSE)** | ✅ Medium | BETSE outputs *bioelectric patterns, not anatomy*; sim runtime | Primary task = **BETSE emulator** (config→Vmem); anatomy = framing/stretch. Use BETSE **fast solver**, target ~2–5k sims |
| 2 | **VNS Dose→Response (Ludwig/SPARC)** | ⚠️ Medium-low | SPARC VNS sets are **small-n (≈2 pigs each)** | Pool multiple sets + treat stim-trials as samples; **DBS fallback** (PD-rat + OpenNeuro STN-LFP) offered |
| 3 | **Non-B DNA Activity (Georgakopoulos-Soares)** | ✅✅ High | "Predict G4 from sequence" is *trivial* (regex motif) | Target **active/observed** structures (DeepG4-style); baseline to beat = DeepG4 AUROC 0.956 |
| 4 | **NeuroAgent-Bench (harness)** | ✅ Medium | Build effort; not a Kaggle leaderboard | Proven template = **BixBench**; test = graded task-suite + success-rate + wall-clock |

**Recommendation:** #3 is the safest hackathon win; #1 is the most on-mission for Levin and the most novel; #4 is the most exciting recruiting pitch; #2 is the best professor-fit but needs the data-pooling mitigation.

---
---

# 1. The Anatomical Compiler v0: A Fast Emulator of Bioelectric Pattern Formation

Tags: Projects · ML Marathon · MLM26 · Regression · Surrogate/Emulator models · Scientific ML · Bioelectricity · Morphogenesis
Author: Tejvir Mann · Draft: 2026-07-01

Bioreactors and ion-channel-modulating drugs already let biologists *write* to a tissue's bioelectric state — the bottleneck is that we can't cheaply *predict* the bioelectric pattern a given intervention produces, let alone invert it. Predicting and controlling those patterns is the core computation behind Michael Levin's "anatomical compiler." This challenge trains the first open ML **emulator of BETSE** (Levin & Pietak's Bioelectric Tissue Simulation Engine), turning a slow biophysics simulation into a fast, differentiable surrogate — the engine an anatomical compiler needs.

## Challenge design
- **Task:** Regression — given an ion-channel / gap-junction / boundary configuration, predict the resulting **steady-state per-cell Vmem pattern** produced by BETSE.
- **Domain:** Developmental bioelectricity, scientific ML, surrogate modeling.
- **Data:** A dataset **generated with BETSE** (open-source, BSD; created by Pietak & Levin), sweeping conductances, gap-junction coupling, and boundary conditions on a fixed cell mesh to produce **(config → Vmem field)** pairs. *Access: 🟢 — you generate and release it; nothing equivalent exists on Kaggle.*
- **Methods:** MLP / CNN / graph-neural-net surrogates over the cell graph; a cVAE or diffusion model for the inverse-design (compiler) bonus track.

## Why it matters
Levin's lab has publicly stated that the lack of large, standardized bioelectric datasets is a primary barrier — there is **no "Brain-Score for bioelectricity."** A validated emulator (a) accelerates the parameter search his lab already runs by orders of magnitude, and (b) is differentiable, so it enables gradient-based *inverse design* — "what intervention yields this target pattern?" — which is the literal definition of a compiler. Prior ML-on-bioelectric-networks work exists but is early (Scientific Reports 2019; "Supervised ML for Bioelectric Cellular Networks," bioRxiv 2024), so a released dataset + benchmark is a real contribution, not a crowded repeat.

## Benchmark & metric
- **Primary metric:** Pearson r / RMSE between predicted and true Vmem field on **held-out configurations**.
- **Held-out test:** an **extrapolation split** — test configs drawn from parameter regions absent in training (the honest test of a compiler, not interpolation).
- **Baseline:** nearest-neighbor in parameter space + a small MLP; publish the score to beat.

## Datasets to integrate (bonus / secondary track)
- **Inverse-design track:** predict the config that yields a *target* Vmem pattern; grade by **re-simulating** the prediction in BETSE and measuring distance to target.
- **Real-data anchor:** **PlanformDB** (Levin's >1,000-experiment planarian regeneration database) as a qualitative validation set; **voxcraft-sim/Evolution Gym** for a cross-substrate probe.

## 9-week timeline
- **Weeks 1–2:** stand up BETSE (fast solver), fix mesh + config schema, generate ~2–5k sims, QC, upload to Kaggle.
- **Weeks 3–5:** release baseline + open leaderboard; teams train emulators.
- **Weeks 6–8:** inverse-design bonus (BETSE-in-the-loop scoring).
- **Week 9:** final eval, dataset/tool release, writeup.

## Related Professor
Uses **Michael Levin's own simulator (BETSE) and database (PlanformDB)**, directly advances his anatomical-compiler program, and produces a reusable open asset his field lacks.

## Links & resources
- BETSE: [github.com/betsee/betse](https://github.com/betsee/betse) · [BETSE paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC4933718/)
- PlanformDB: [planform.daniel-lobo.com](http://planform.daniel-lobo.com)
- Libraries: PyTorch, PyTorch Geometric (GNNs); scientific-ML / surrogate-modeling primer.

---
---

# 2. Dialing the Vagus: Predicting Therapy vs. Side-Effect from Nerve-Stimulation Parameters

Tags: Projects · ML Marathon · MLM26 · Regression · Time-series · Signal processing · Neuromodulation · Bioelectronic medicine
Author: Tejvir Mann · Draft: 2026-07-01

Vagus nerve stimulation (VNS) treats epilepsy, depression, and inflammation, but choosing parameters is trial-and-error and the same current that helps the heart can trigger throat/laryngeal side-effects. This challenge asks teams to learn the **dose-response surface**: predict the therapeutic effect (change in heart rate) and the off-target effect (neck/laryngeal EMG) from stimulation parameters + evoked nerve activity — the core computation behind closed-loop bioelectronic medicine, built on public **NIH SPARC** recordings.

## Challenge design
- **Task:** Multi-output regression — predict Δheart-rate (therapy) and laryngeal/neck EMG magnitude (side-effect) from {amplitude, frequency, pulse width, burst pattern} (+ evoked compound nerve activity where available).
- **Domain:** Peripheral/autonomic neuromodulation, physiological time-series.
- **Data:** Pooled **SPARC/Pennsieve** VNS datasets (e.g., *Influence of VNS on cardiac activity in pigs* [#349], *Pig-specific VNS computational models, 6-contact cuff* [#305, Musselman/Settell/**Ludwig**/Grill/Pelot], *ECG in mice during VNS* [#375]). *Access: 🟡 — public CC-BY, stored as experiment packages → curation into ML tensors is part of the project.*
- **Methods:** gradient-boosted trees on engineered features; 1D-CNN / temporal models on raw evoked waveforms.

## ⚠️ Feasibility note (pressure-tested)
Individual SPARC VNS datasets are **small-n animals** (e.g., #349 = 2 pigs). Two mitigations make this a valid ML task: (1) **each animal contributes hundreds–thousands of stimulation trials** across a parameter grid → thousands of labeled samples, so learning the within-surface dose-response is well-posed with an *unseen-parameter-region* held-out split; (2) **pool multiple SPARC sets** for the cross-animal/cross-species stretch track. If reviewers want more subjects, the **DBS fallback** below swaps in for the same "predict physiological effect of stimulation" framing with more subjects.
> **DBS fallback:** public **PD-rat longitudinal DBS ephys+behavior** dataset + human **STN-LFP** sets on OpenNeuro → predict motor-state/beta-power from LFP (adaptive-DBS biomarker). More subjects, same closed-loop theme, still squarely Ludwig's domain.

## Benchmark & metric
- **Primary metric:** RMSE / R² on held-out (parameter, response) pairs, reported separately for therapy and side-effect targets.
- **Held-out test:** leave-out **unseen parameter combinations** (primary); **unseen animals/species** (hard track).
- **Baseline:** linear dose-response fit + gradient-boosted trees.

## Datasets to integrate (bonus / secondary track)
- Cross-species transfer (train pig → test mouse-ECG); broader SPARC autonomic catalog; pig vagus morphology/histology as covariates.

## 9-week timeline
- **Weeks 1–2:** pull SPARC packages, align stim logs to physiology, build tabular+waveform dataset, upload to Kaggle.
- **Weeks 3–5:** baseline + leaderboard.
- **Weeks 6–8:** cross-animal/species generalization track.
- **Week 9:** final eval + a "recommended safe-parameter map" deliverable.

## Related Professor
Targets **Kip Ludwig's** exact research (VNS, baroreflex, closed-loop neuromodulation) and uses SPARC data his lab co-produces — the first ML benchmark on those recordings.

## Links & resources
- SPARC: [#349](https://discover.pennsieve.io/datasets/349) · [#305](https://discover.pennsieve.io/datasets/305) · [#375](https://discover.pennsieve.io/datasets/375) · [portal](https://sparc.science/)
- Libraries: scikit-learn / XGBoost; PyTorch 1D-CNN; MNE for signal handling.

---
---

# 3. Reading the Genome's Hidden Structures: Predicting *Active* Non-B DNA from Sequence

Tags: Projects · ML Marathon · MLM26 · Classification · Genomics · Sequence models · Cancer biology
Author: Tejvir Mann · Draft: 2026-07-01

Sequences can fold into **non-B DNA structures** — G-quadruplexes, cruciforms, hairpins — that drive replication stress, mutation, and cancer. Whether a structure is *actually active in a given genome/cell type* depends on subtle context, not just the core motif. The Georgakopoulos-Soares lab has catalogued these at unprecedented scale. This challenge asks teams to predict, from sequence context, whether a locus forms an **active** non-B structure — a clean, large, high-impact sequence-to-function problem with a strong existing baseline to beat.

## Challenge design
- **Task:** Classification (± regression) — from a sequence window (~200 bp of genomic context), predict whether it forms an **active/observed** non-B structure (G-quadruplex / cruciform) vs. a matched negative.
- **Domain:** Computational genomics, cancer mutagenesis, sequence modeling.
- **Data:** **Quadrupia** (~140M G-quadruplexes across ~108k genomes) and **invertiaDB** (30M inverted repeats/cruciforms across 118k genomes) as positives; negatives sampled from **GC- and length-matched** genomic background. *Access: 🟢 — downloadable; among the largest clean labeled datasets available to this hackathon.*
- **Methods:** k-mer/CNN sequence classifiers; DNA language-model fine-tuning (HyenaDNA, Nucleotide Transformer).

## Why it matters (and why it's not trivial)
Naive "does this sequence contain a G4 motif" is a regex — uninteresting. But predicting **active** G4 regions from *context* is a genuine, published ML problem: **DeepG4** reaches **AUROC 0.956** using 201-bp context and beats G4Hunter/Quadron/pqsfinder — proving there's real signal beyond the motif. Framed this way, the leaderboard has (a) a clear baseline to beat (DeepG4), (b) a natural hard split (**held-out organisms/clades**), and (c) direct payoff for variant interpretation and cancer-hotspot prioritization.

## Benchmark & metric
- **Primary metric:** AUROC / AUPRC (class-imbalanced), cross-species held-out split.
- **Held-out test:** entire **held-out genomes/clades** — tests generalization, not memorization of one species' motifs.
- **Baseline:** k-mer logistic regression + reproduce **DeepG4**; publish both scores.

## Datasets to integrate (bonus / secondary track)
- **MPRAbase** (17.7M regulatory elements) — "does structure affect regulatory activity?"; **Microsatellites Explorer**/**kmerDB** for extra non-B classes; ENCODE/Ensembl annotations.

## 9-week timeline
- **Weeks 1–2:** build positive/negative sets from Quadrupia + invertiaDB, define matched negatives + splits, upload to Kaggle.
- **Weeks 3–5:** baseline (k-mer + DeepG4) + leaderboard.
- **Weeks 6–8:** DNA-LM fine-tuning; cross-species track.
- **Week 9:** final eval + motif/feature-importance writeup.

## Related Professor
Built entirely on **Ilias Georgakopoulos-Soares's own databases**, which his lab publishes for community ML use. Cleanest big-data option; beginner on-ramp (k-mer) with an advanced ceiling (DNA LMs).

## Links & resources
- Databases: [lab index](https://www.georgakopoulos-soares-lab.com/databases) · [Quadrupia](https://github.com/Georgakopoulos-Soares-lab/Quadrupia) · [invertiaDB](https://academic.oup.com/nar/article/53/8/gkaf329/8118974)
- Baseline: [DeepG4](https://github.com/raphaelmourad/DeepG4) · Libraries: HuggingFace, HyenaDNA, Nucleotide Transformer.

---
---

# 4. NeuroAgent-Bench: An Open Harness & Benchmark for AI Agents that Run Neuroscience Workflows

Tags: Projects · ML Marathon · MLM26 · LLM agents · Benchmarking · Tooling · Neuroscience infrastructure
Author: Tejvir Mann · Draft: 2026-07-01

Neuroscience analysis is bottlenecked by human labor: loading messy BIDS/NWB data, preprocessing in MNE, fitting a decoder, computing a metric, writing it up. "opencode for neuroscience" would be an agent that does these end-to-end — but there is **no standard way to measure whether such an agent works**. This project builds both halves: a lightweight **agent harness** wired to real neuro tools, and **NeuroAgent-Bench**, a graded suite of realistic analysis tasks.

> **Format note:** this is a *tooling + benchmark* project, not a single-stage Kaggle leaderboard — it fits MLM26 as the "second project / lab-tool track." The competition-legal core is the graded task-suite below.

## Is there already a benchmark? Can we build one easily? (pressure-tested)
- **For neuroscience specifically: no.** There is no agentic benchmark for neuro-analysis workflows.
- **For science agents generally: yes — and they prove the template works.** **BixBench** (FutureHouse) evaluates LLM agents on 50+ real computational-biology analyses with ~300 questions in a notebook environment; frontier models score only **~17%** open-answer. Siblings: **ScienceAgentBench**, **MLE-bench** (Kaggle-style ML engineering), **BioCoder**. We port that proven recipe to neuroscience.
- **Yes, we can build it in 9 weeks**, because every task is just: *fixed public dataset + natural-language request + hidden reference answer + automatic grader.*

## Challenge design
- **Task:** Given an analysis request + a dataset, the agent must produce the correct answer/artifact (e.g., "decode left-vs-right motor imagery from this EEG set; report held-out accuracy").
- **Domain:** LLM agents, reproducible neuro-analysis tooling.
- **Data / task suite:** 12–20 tasks built on public data — PhysioNet EEG Motor Movement/Imagery, Sleep-EDF (staging), THINGS-EEG2 (decoding), a SPARC VNS set, an OpenNeuro fMRI set, and a BETSE-analysis task (ties to Proposal 1). *Access: 🟢 — components public; the contribution is task packaging + graders + harness.*
- **Methods:** an agent loop with tool-use over MNE-Python, Nilearn, scikit-learn, NWB/BIDS loaders.

## How we test if it works (the benchmark design)
Three grader types, mirroring BixBench + adding objective checks:
1. **Programmatic / numeric** (primary): closed-form answers checked within tolerance vs. a reference we computed — decoding accuracy, sleep-staging **Cohen's κ**, a PCI value, an ERP peak latency. Objective, cheap, reproducible.
2. **Artifact check:** does the produced file/figure/model meet a spec (right shape, saved BIDS-derivative, plot exists)?
3. **LLM-judge** (open-answer interpretation), calibrated against human ratings on a held-out slice, as BixBench does.

- **Primary metric:** **task success rate** across held-out tasks.
- **Secondary metrics:** **time-to-first-correct-result** (wall-clock vs. a human baseline — the actual speedup claim) and **robustness** = success on **held-out datasets the harness was never tuned on**.
- **Baselines to publish:** (a) scripted non-agent pipeline (upper bound), (b) naive single-prompt LLM (lower bound), (c) the agent harness.

## 9-week timeline
- **Weeks 1–3:** build harness (tool adapters) + 8–12 seed tasks with graders.
- **Weeks 4–6:** baselines + agent runs; publish leaderboard.
- **Weeks 7–8:** hardening, held-out task set, wall-clock study.
- **Week 9:** release harness + benchmark + report.

## Related Professor
There isn't one. Lab-agnostic infrastructure: **every advisor here becomes a task-contributor and a user** (a Ludwig VNS task, a Levin BETSE task, a WISC PCI task). Positions the team as owners of shared neuro tooling.

## Links & resources
- Prior art: [BixBench](https://arxiv.org/abs/2503.00096) · ScienceAgentBench · MLE-bench · [EEGDash](https://arxiv.org/pdf/2606.16041)
- Libraries: MNE-Python, Nilearn, NWB/BIDS, scikit-learn; Claude Agent SDK / tool-use.

---
---

## Appendix — Quality datasets & benchmarks (quick reference)

**Richest brain data:** CNeuroMod (100+ h fMRI/subject — richest human), MICrONS (523M synapses + co-registered function — richest overall), Brain Treebank (human intracranial + naturalistic language), IBL Brain-Wide Map (621k neurons), NSD (7T, WISC-co-authored), THINGS (EEG2/fMRI/MEG).

**Established ML benchmarks (prior art):** Algonauts 2025 (fMRI←movies, Pearson r), THINGS-EEG2 (200-way retrieval), Brain-Score (ANN↔cortex), Neural Latents Benchmark (co-smoothing), FALCON (stable iBCI decoding), Sensorium (mouse V1), PCIst (consciousness), Open Problems in Single-Cell, DeepG4 (active G4), BixBench (bio-analysis agents).

**Open lanes with no benchmark (create one):** peripheral neuromodulation dose→response (P2), bioelectric morphogenesis (P1), neuroscience-workflow agents (P4).

**Lab-owned data:** Ludwig → SPARC VNS (#349/#305/#375, small-n); Levin → BETSE + PlanformDB + xenobots; Georgakopoulos-Soares → 7 DBs (Quadrupia 140M, invertiaDB 30M, MPRAbase 17.7M, Microsatellites, kmerDB, metagRoot, Darling); Krishnaswamy → PHATE/MAGIC/MIOFlow (advisor); WISC → open PCIst code + public anesthesia/sleep EEG (flagship corpus gated).
