# MLM26 — Ranked Project Ideas (Broad List, Pre-Deepening)

*For ML Marathon 2026 (ml-marathon.wisc.edu). Target advisors: **Michael Levin** (Tufts — bioelectricity/morphogenesis), **Kip Ludwig** (UW–Madison, WITNe — neuromodulation/bioelectronic medicine), plus the local **WISC** consciousness group (Tononi/Cirelli/Boly) and **biotech** collaborators (Krishnaswamy @ Yale; Georgakopoulos-Soares).*

This is the **broad list to choose from**. Once you pick winners, I'll deep-dive each into a full ML-X-Nexus-style proposal (verified data license, exact splits, baseline, 9-week milestones).

---

## How each idea is scored

Every idea is checked against your three hard requirements plus Kaggle-fit:

| Check | Meaning |
|---|---|
| **DATA** | Is there a real, licensable, Kaggle-uploadable dataset *today*? 🟢 ready / 🟡 exists but needs wrangling / 🔴 must be curated from scratch |
| **PROBLEM** | Is there published evidence this is a genuine open problem? |
| **BENCH** | Is there a narrow predictive task + measurable metric + clean held-out split? (MLM/Kaggle requires this) |
| **HOOK** | Why the target professor personally benefits from the result |

MLM26 constraint reminder: single-stage Kaggle leaderboard, one narrow predictive task, measurable metric, held-out test set. "Integrate more datasets" works as a **bonus/secondary track**, not the primary metric.

---

## Overall ranking (my recommendation)

| # | Idea | Area | DATA | Kaggle-fit | Prof hook |
|---|---|---|---|---|---|
| 1 | **EEG→Image zero-shot decoding (THINGS-EEG2)** | Consciousness/vision | 🟢 | Excellent (established 200-way protocol) | WISC (NSD) + your collaborator |
| 2 | **Xenobot morphology → fitness surrogate** | Bioelectricity | 🟢 (simulated) | Excellent (clean regression) | Levin — accelerates real design loop |
| 3 | **VNS dose → physiological response** | Neuromodulation | 🟡 | Good (regression) | Ludwig — his exact domain (vagus/baroreflex) |
| 4 | **Consciousness-state classifier + PCI harness** | Consciousness | 🟢 | Good (ordinal classification) | WISC — automates their own metric |
| 5 | **Non-B DNA structure prediction** | Biotech/genomics | 🟢 | Good (AUROC) | Georgakopoulos-Soares — his databases |
| 6 | **PlanformDB regeneration-outcome prediction** | Bioelectricity | 🟡 | Good (structured prediction) | Levin — his curated DB |
| 7 | **Closed-loop DBS biomarker prediction** | Neuromodulation | 🟡 | Good (classification/regression) | Ludwig — closed-loop DBS |
| 8 | **MPRA regulatory-activity prediction** | Biotech/genomics | 🟢 | Good (regression) | Georgakopoulos-Soares — MPRAbase |
| H1 | **NeuroHarness (opencode-for-neuro)** | Harness | n/a | Special track (not Kaggle leaderboard) | All labs — reusable tool |
| H2 | **Closed-loop neuromodulation Gym/benchmark** | Harness | 🟢 (sim) | Special track | Ludwig — controller benchmark |

Below, ideas grouped by your four areas + the harness track.

---

## Area 1 — Bioelectricity & Morphogenesis (Levin)

**Field reality:** Levin's own lab has stated a lack of large, standardized bioelectric-profiling datasets is a real barrier. There is **no "Brain-Score for bioelectricity."** That's the opportunity — but raw imaging data is scattered, so the two most Kaggle-viable ideas lean on **simulation** and a **pre-existing curated database**.

### 1A. Xenobot / soft-robot morphology → fitness surrogate  ⭐ (ranked #2)
- **Task:** regression — predict a voxel-body's task performance (e.g., locomotion distance) directly from its morphology, skipping the expensive physics sim.
- **DATA 🟢:** generate unlimited labeled examples with `voxcraft-sim` (the GPU engine used in actual xenobot design) or **Evolution Gym (evogym)** benchmark tasks. Real behavioral reference: `github.com/swarm-lab/xenobots`.
- **PROBLEM:** evolutionary design of xenobots is bottlenecked by simulation cost; surrogate models are the standard accelerator. Levin/Bongard's pipeline uses exactly this loop.
- **BENCH:** RMSE / Spearman ρ vs true simulated fitness on **held-out unseen morphologies**. Clean, unambiguous.
- **Integrate:** evogym multi-task suite; real xenobot trajectories; different material/voxel configs.
- **HOOK:** a good surrogate literally speeds up the design search his field runs. Deliverable is reusable the day the Marathon ends.

### 1B. Planarian regeneration-outcome prediction (PlanformDB)  (ranked #6)
- **Task:** predict regeneration outcome (head/tail count, polarity, morphology class) from an experimental manipulation (amputation geometry + drug/RNAi perturbation).
- **DATA 🟡:** **PlanformDB** — Lobo & Levin's curated, formalized database of planarian regeneration experiments (published, machine-readable). Needs conversion to ML tensors but is *already structured*.
- **PROBLEM:** reverse-engineering the rules of regeneration is an explicit Levin research goal; predicting outcomes from perturbations is unsolved.
- **BENCH:** outcome-classification accuracy / F1 on held-out experiments (split by manipulation type).
- **HOOK:** directly extends his lab's own database and "reverse-engineer the worm" program.
- **Risk:** dataset is modest in size — may suit a smaller leaderboard or a "beat the mechanistic model" framing.

### 1C. Bioelectric-pattern → anatomical-outcome aggregator  (stretch / high-novelty, 🔴 data)
- Curate scattered published Vmem / DiBAC4(3) imaging (planaria, Xenopus) into the **first** standardized ML-ready bioelectric benchmark. The *curation itself* is a genuine field contribution.
- **Risk:** 🔴 data readiness — likely too little standardized data for a solo Kaggle leaderboard in 9 weeks. Better as a "dataset paper" side-deliverable or fused with 1B.

---

## Area 2 — Neuromodulation & Bioelectronic Medicine (Ludwig)

**Ludwig's actual portfolio:** vagus nerve stimulation & human vagus mapping, baroreflex activation therapy, electrode safety testing, minimally-invasive/injectable electrodes, closed-loop stimulation. His domain is **peripheral/autonomic**, distinct from the fMRI/EEG-decoding world — so these ideas are differentiated.

### 2A. VNS dose → physiological response prediction  ⭐ (ranked #3 — best Ludwig fit)
- **Task:** regression — predict physiological outcome from stimulation parameters (+ evoked nerve activity): therapeutic effect (Δ heart rate) **and** side-effect (laryngeal EMG), across amplitude/frequency/pulse-pattern combinations.
- **DATA 🟡:** NIH **SPARC** portal (`sparc.science`) — FAIR-curated VNS dose-response datasets (anesthetized mouse/dog/pig) with cardiac, respiratory, EMG, and evoked-fiber recordings. Published dose-response studies provide the schema.
- **PROBLEM:** "which VNS parameters give therapy without side-effects" is *the* open question in bioelectronic medicine; personalizing dose from neural biomarkers is active research.
- **BENCH:** RMSE / R² on held-out parameter combinations or held-out animals.
- **Integrate:** multiple SPARC VNS datasets across species; baroreflex datasets.
- **HOOK:** maps onto his vagus-mapping + baroreflex + parameter-optimization grants precisely. A predictive dose-response model is directly usable.

### 2B. Closed-loop DBS biomarker prediction  (ranked #7)
- **Task:** classify motor state / predict symptom severity (or optimal stim contact) from subthalamic **LFP** windows — the core of adaptive/closed-loop DBS.
- **DATA 🟡:** open **PD-rat longitudinal DBS electrophysiology+behavior dataset** (published, Scientific Data); human STN-LFP sets on OpenNeuro; "optimal DBS contact from LFP" datasets.
- **PROBLEM:** finding robust LFP physiomarkers (beta-band etc.) for closed-loop control is an active, unsolved area with a large review literature.
- **BENCH:** classification accuracy / correlation on held-out subjects (cross-subject generalization is the hard part — good leaderboard design).
- **HOOK:** closed-loop neuromodulation is central to his and WITNe's mission.

### 2C. Electrode safety / signal-quality prediction  (🔴 — noted, likely skip)
- Predict electrode failure / impedance drift / stim artifact. Conceptually perfect for his $1.8M electrode-safety framework, but the relevant data isn't public. Flagging only.

---

## Area 3 — Consciousness (WISC: Tononi / Cirelli / Boly — strongest *local* tie)

### 3A. EEG→Image zero-shot visual decoding  ⭐ (ranked #1 — highest feasibility)
- **Task:** reconstruct/retrieve the seen image from brain activity — the Algonauts spirit, but on EEG (your collaborator's exact angle).
- **DATA 🟢:** **THINGS-EEG2** (63-ch, 10 subjects, 1,654 train concepts, 200 held-out test concepts) — already the *de facto* benchmark. Plus **THINGS-fMRI**, the 7T **Natural Scenes Dataset (NSD)** (used by WISC/Tononi in PNAS 2022), and **Alljoined-1.6M**.
- **PROBLEM:** how vision is encoded in neural activity; EEG (cheap, portable) vs fMRI trade-off is open and hot.
- **BENCH:** **established 200-way zero-shot top-k retrieval accuracy** — an existing, unambiguous, leaderboard-ready protocol. Lowest design risk of any idea here.
- **Integrate:** NSD fMRI, THINGS-MEG, Alljoined for a cross-modality/generalization bonus track.
- **HOOK:** WISC co-authored the NSD work; ties naturally to encoding/decoding + IIT questions. Also directly matches your collaborator's THINGS/Algonauts pitch.

### 3B. Consciousness-state classifier + PCI complexity harness  ⭐ (ranked #4)
- **Task:** classify EEG epochs on an ordinal consciousness scale (wake / sedated / unconscious / disorders-of-consciousness).
- **DATA 🟢:** anesthesia EEG (propofol/xenon/ketamine), TMS-EEG PCI benchmark (Casali 2013, 719 sessions), Sleep-EDF, DoC datasets on OpenNeuro.
- **PROBLEM:** automated, calibration-free, cross-site consciousness scoring is unsolved; PCI is specialist-dependent.
- **BENCH:** macro-F1 / ordinal accuracy on **held-out subjects**; bonus: reproduce the published PCI* wake/unconscious cutoff.
- **Harness deliverable:** MNE-Python + open-source **PCIst** (`github.com/renzocom/PCIst`) + BIDS ingestion → standardized complexity report. Reusable by WISC immediately.
- **Integrate:** psychedelics transfer (PsiConnect ds006110, DMT EEG-fMRI).
- **HOOK:** PCI/IIT originated at UW–Madison — "your own metric, benchmarked and packaged as a reusable tool" is an easy pitch to Tononi/Boly/Cirelli.

### 3C. Sleep slow-wave / auto-staging at scale  (lower novelty)
- WISC (Cirelli/Tononi) sleep need. Sleep-EDF / DREEM; metric = Cohen's κ. Solid but a **crowded, near-solved** problem — keep as fallback.

---

## Area 4 — Biotech / Genomics

### 4A. Non-B DNA structure prediction from sequence  ⭐ (ranked #5)
- **Task:** predict whether a genomic sequence forms a non-B structure (cruciform/hairpin from inverted repeats) — a determinant of genomic instability & cancer.
- **DATA 🟢:** **invertiaDB** (>30M inverted repeats across 118,019 genomes), **Microsatellites Explorer**, **kmerDB** — all from Georgakopoulos-Soares' `georgakopoulos-soares-lab.com/databases`. Large and ML-ready.
- **PROBLEM:** non-B DNA → instability/disease mapping is his core research; sequence→structure prediction at scale is open.
- **BENCH:** AUROC / AUPRC on held-out genomes (cross-species generalization = natural hard split).
- **HOOK:** he builds these databases explicitly for community ML use — strong collaboration incentive.

### 4B. MPRA regulatory-activity prediction (MPRAbase)  (ranked #8)
- **Task:** predict regulatory-element activity from sequence (a focused, tractable "mini-Enformer").
- **DATA 🟢:** **MPRAbase** — curated Massively Parallel Reporter Assay results across cell types/species.
- **BENCH:** correlation / R² on held-out elements or held-out cell type.
- **HOOK:** same lab; clean regression; approachable for mixed-skill teams.

### 4C. Krishnaswamy-lab angle (single-cell geometry / mRNA design)
- Two sub-options: (i) trajectory/manifold-inference benchmark using her lab's **81-dataset / 171-method single-cell benchmark**; (ii) **property-guided mRNA sequence generation/property prediction** (cf. their RNAGenScape). 🟢 data.
- **PROBLEM/HOOK:** she is a *benchmarking-oriented* ML methods person → ideal advisor; but single-cell benchmarking is complex for a hackathon. mRNA property prediction is more contained and trendy.
- **Note:** your `biotech.md` links her lab video + the Georgakopoulos-Soares databases; 4A is the more turnkey of the two.

---

## Area 5 — AI Harness track (your "opencode-for-neuroscience" idea)

Harnesses **don't fit a Kaggle single-stage leaderboard cleanly** — flag this. Two ways to make them competition-legal:

### H1. NeuroHarness — standardized ingest→preprocess→decode for NWB/BIDS
- Plug any NWB/BIDS dataset into one pipeline; teams submit a **harness config** (not a per-dataset model) that must run *unmodified* across N held-out public datasets.
- **BENCH:** avg accuracy/F1 across held-out datasets **+ wall-clock "time from raw dataset → first result"** (the actual speedup claim, quantified). Anchor task: PhysioNet EEG Motor Movement/Imagery.
- **HOOK:** every lab above benefits; positions you as infra owner. Fits the "second project / advisor-tool" track your collaborator floated.

### H2. Closed-loop neuromodulation benchmark (Gym-style)  — Ludwig-flavored
- A standardized simulated environment (a "NeuroGym" for stimulation): teams submit a **controller/policy**, scored on symptom-suppression vs stimulation-energy trade-off.
- **DATA 🟢 (sim):** build on adaptive-DBS / VNS response models above.
- **BENCH:** the environment *is* the benchmark — reward = efficacy − energy penalty, on held-out patient/animal models.
- **HOOK:** gives Ludwig's closed-loop community a shared benchmark that doesn't exist yet.

---

## Recommended shortlist to deepen (pick 2–4)

To cover all four areas + one professor each, a balanced set:

1. **#1 EEG→Image (THINGS-EEG2)** — safest, established benchmark, WISC + collaborator. *(Consciousness)*
2. **#3 VNS dose→response (SPARC)** — best Ludwig fit. *(Neuromodulation)*
3. **#2 Xenobot fitness surrogate** — best Levin fit with ready (simulated) data. *(Bioelectricity)*
4. **#5 Non-B DNA prediction** — cleanest biotech option. *(Biotech)*

Plus **H1 NeuroHarness** or **H2 NeuroGym** as the "second project."

---

## Open questions before deepening

1. **Which professor is the primary target?** (Affects whether Levin/Ludwig ideas get the deepest treatment.) You can advise/co-organize with one lab more tightly.
2. **Team ML skill level?** (Genomics AUROC + surrogate regression are beginner-friendlier; EEG→Image diffusion decoding is advanced.)
3. **Is data-licensing legwork acceptable?** SPARC/PlanformDB need wrangling; THINGS-EEG2/invertiaDB/simulation are turnkey.
4. **Harness = competition entry or separate advisor deliverable?** (It's cleaner as the latter.)

---

## Source links

- Ludwig / WITNe: [engineering.wisc.edu](https://engineering.wisc.edu/blog/ludwig-improving-neuromodulation-safety-testing-through-brain-initiative-grant/), [neurosurgery.wisc.edu](https://www.neurosurgery.wisc.edu/staff/ludwig-kip/)
- ML Marathon: [ml-marathon.wisc.edu](https://ml-marathon.wisc.edu/)
- Levin lab computational: [as.tufts.edu/biology/levin-lab](https://as.tufts.edu/biology/levin-lab/publications/computational)
- SPARC: [sparc.science overview](https://www.nih.gov/common-fund/common-fund-programs/stimulating-peripheral-activity-relieve-conditions-sparc), [SPARC DRC paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC8265045/)
- VNS dose-response: [Bioelectronic Medicine 2023](https://pmc.ncbi.nlm.nih.gov/articles/PMC9936668/)
- DBS ML review: [PMC10576725](https://pmc.ncbi.nlm.nih.gov/articles/PMC10576725/); PD-rat DBS dataset: [PMC11096386](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11096386/)
- THINGS-EEG2 / decoding: [Alljoined-1.6M](https://arxiv.org/html/2508.18571v2)
- Georgakopoulos-Soares databases: [invertiaDB (NAR 2025)](https://academic.oup.com/nar/article/53/8/gkaf329/8118974), [lab databases](https://www.georgakopoulos-soares-lab.com/databases)
- Krishnaswamy lab: [projects](https://krishnaswamylab.org/projects), [RNAGenScape](https://arxiv.org/pdf/2510.24736)
