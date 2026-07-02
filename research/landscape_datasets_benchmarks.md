# MLM26 — Dataset & Benchmark Landscape

*Companion to [proposals_ranked_shortlist.md](proposals_ranked_shortlist.md). Focus: what quality data/benchmarks already exist, and — importantly — **what each named professor's own lab publishes** that you could build a challenge on.*

**Access legend:** 🟢 public download · 🟡 public but needs wrangling/large · 🔵 request/registration · 🔴 not public

---

## TL;DR of the landscape

- **The "encoding/decoding brain-from-stimulus" space is mature and benchmarked** (Algonauts, THINGS, NSD, Brain-Score, Sensorium). Great for a *safe, well-scoped* challenge — but you're competing with an established field, so novelty comes from the *angle* (EEG instead of fMRI; a specific lab's question).
- **Ludwig's lab is a goldmine of underused, ML-ready data.** He is a prolific contributor to NIH **SPARC** — multiple public pig/mouse vagus-nerve datasets with EMG/ECG/neural recordings. This is *real lab-owned data with almost no ML benchmark built on it yet.* Best "impress the professor with his own data" opportunity.
- **Levin's lab owns two curated databases** (Planform/PlanformDB, PLANAtools) + xenobot behavioral data. Structured and unusual — high novelty, smaller scale.
- **Georgakopoulos-Soares owns 7 large genomic databases** — all downloadable, some literally millions of records. The most turnkey "big clean dataset" of anyone here.
- **Krishnaswamy's lab is benchmark-oriented** (tools + datasets on GitHub) — ideal *advisor* for a methods challenge.
- **WISC/consciousness data is partly gated:** the flagship PCI/TMS-EEG corpus (719 sessions) isn't a single public download, but the *metric code is open* and the NSD (which they co-authored) is fully public.

---

## 1. Established ML benchmarks in this space (prior art — know what exists)

| Benchmark | Domain | Task & metric | Access |
|---|---|---|---|
| **Algonauts 2025** | fMRI, naturalistic movies | Predict fMRI (1,000 parcels) from multimodal movies; **Pearson r**, held-out OOD-movie leaderboard. Built on CNeuroMod, 4 subjects, 65 h | 🟢 |
| **THINGS-EEG2** | EEG, object vision | Zero-shot **200-way image retrieval** (top-k acc); 10 subjects, 1,654 train / 200 test concepts | 🟢 |
| **Natural Scenes Dataset (NSD)** | 7T fMRI, vision/memory | Encoding + image reconstruction; 8 subjects, ~73k COCO images, 7T. *Co-authored by WISC's group* | 🟢/🔵 |
| **Brain-Score** | ANN ↔ primate visual cortex | How well a model predicts neural responses; ceiling-normalized score | 🟢 |
| **Neural Latents Benchmark (NLB '21)** | Spiking (monkey) | Latent-variable modeling; **co-smoothing**, forward-prediction, behavior decoding | 🟢 |
| **FALCON** | iBCI (human+monkey+bird) | **Few-shot stable decoding** across held-out sessions; 5 datasets (reach, handwriting, finger, birdsong) | 🟢 |
| **Sensorium / Dynamic Sensorium** | Mouse visual cortex | Predict neuronal responses to images/video; correlation | 🟢 |
| **PCIst (PCI)** | TMS-EEG, consciousness | Complexity index vs. wake/unconscious cutoff; *code open, benchmark corpus gated* | code 🟢 / data 🔴 |
| **Open Problems in Single-Cell** | scRNA/multiomics | 12 tasks, 81 datasets, 171 methods; standardized metrics. *Krishnaswamy-adjacent* | 🟢 |

**Gap worth noting (still true):** there is **no established benchmark for peripheral/autonomic neuromodulation (VNS/DBS dose→response)** and **none for bioelectric morphogenesis**. Those are the two open lanes where you'd be *creating* the benchmark, not competing in one.

---

## 2. Lab-owned datasets, by professor  ← the part you asked about

### Kip Ludwig (UW–Madison, WITNe) — via NIH SPARC / Pennsieve  ⭐ strongest "own data" story
Ludwig is an author on a cluster of public SPARC datasets. These carry **electrophysiology + physiological outcomes + stimulation parameters** — exactly the ingredients for a dose→response ML challenge, and essentially **no one has built an ML benchmark on them.**

| Dataset | Contents | Link |
|---|---|---|
| Influence of VNS on cardiac activity in pigs | 30-s windows of **vagus nerve signal + ECG** per stimulation — natural stim→response pairs | [Pennsieve #349](https://discover.pennsieve.io/datasets/349) |
| Pig-specific computational models of VNS (6-contact cuff) | **EMG + EKG** recordings in response to VNS; parameter sweeps | [Pennsieve #305](https://discover.pennsieve.io/datasets/305) |
| ECG recordings in mice during VNS | Cardiac response to VNS (cross-species transfer track) | [Pennsieve #375](https://discover.pennsieve.io/datasets/375) |
| In vivo pig vagus 'vagotopy' via ultrasound | Imaging of fascicle organization | [Pennsieve #252](https://discover.pennsieve.io/datasets/252) |
| Anatomy/histology of pig vagus for VNS | Fascicle organization + diameter from animals that also had ephys | [SPARC #85](https://sparc.science/datasets/85) |
| Quantified morphology of pig vagus | Segmented fascicle/nerve traces (TIFF), 9+9 samples | [SPARC #64](https://sparc.science/datasets/64) |

- **Access 🟡:** public on SPARC/Pennsieve under CC-BY, but stored as structured experiment packages (not a tidy CSV) → wrangling into ML tensors is real work (and is itself a contribution).
- **Challenge this enables:** predict Δheart-rate (therapy) and EMG side-effect from stimulation parameters + evoked nerve activity → RMSE on held-out parameter combos/animals. *This is the "impress Ludwig with his own data" project.*

### Michael Levin (Tufts, Allen Discovery Center) — curated databases + xenobot data
| Asset | Contents | Link |
|---|---|---|
| **Planform / PlanformDB** | Manually curated, **graph-encoded database of >1,000 planarian regeneration experiments** (manipulations → morphological outcomes) | [planform.daniel-lobo.com](http://planform.daniel-lobo.com) · [paper](https://academic.oup.com/bioinformatics/article/29/8/1098/229314) |
| **PLANAtools** | Interactive gene-expression repository for planarian *S. mediterranea* | [paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10076545/) |
| **Xenobot behavioral data** | Raw behavior + analysis for the living-machines work (Blackiston, Kriegman, Bongard, Levin) | [github.com/swarm-lab/xenobots](https://github.com/swarm-lab/xenobots) |
| Lab software hub | Bioinformatics of Shape, evolutionary-search tools, models | [levin-lab/software](https://as.tufts.edu/biology/levin-lab/resources/software) |

- **Access 🟢/🟡:** Planform + xenobots are downloadable; scale is modest (best for structured-prediction, not a giant leaderboard).
- **Challenge this enables:** predict regeneration outcome (head/tail/polarity) from a Planform-encoded manipulation → structured-prediction accuracy on held-out experiments. Directly extends his "reverse-engineer the worm" program. (Pair with the **simulation-based xenobot-fitness surrogate** for a bigger dataset.)

### WISC / consciousness (Tononi, Cirelli, Boly) — partly gated
| Asset | Contents | Access |
|---|---|---|
| **NSD** (co-authored) | 7T fMRI vision/memory, 8 subjects, ~73k images | 🟢/🔵 |
| **PCI / TMS-EEG corpus** (Casali/Casarotto/Comolatti) | 719 TMS-hd-EEG sessions across wake/sleep/anesthesia/DoC — *the* consciousness-complexity dataset | 🔴 (not a single public download) |
| **PCIst code** | Open implementation of the state-transition PCI | 🟢 [github/renzocom/PCIst](https://github.com/renzocom/pcist) |
| High-density sleep EEG | Their slow-wave/sleep work; raw HD-EEG mostly not open (public analog: **ANPHY-Sleep**, Sci Data 2024) | 🔴 / analog 🟢 |

- **Reality check:** the highest-value WISC data (PCI corpus, HD sleep) is **gated** — you'd need to *partner with the lab* to get it, or build on the open metric + public anesthesia/sleep EEG. This is why the safe WISC-flavored challenge uses **public EEG + their open PCI metric**, and the ambitious one requires an actual collaboration ask.

### Smita Krishnaswamy (Yale) — tools + datasets, benchmark-minded
- GitHub org [KrishnaswamyLab](https://github.com/KrishnaswamyLab): **PHATE, MAGIC, SAUCIE, MIOFlow** + the single-cell datasets they ship (e.g., embryoid-body scRNA time-course used in PHATE/MIOFlow). Recent: **MIOFlow 2.0**, **RNAGenScape** (property-guided mRNA generation).
- **Access 🟢.** Her lab *builds benchmarks* → she's the ideal **advisor** for a methods/generation challenge rather than a source of one giant dataset. Best fit: trajectory-inference or mRNA-property prediction with a clean held-out split.

### Ilias Georgakopoulos-Soares — 7 large, downloadable genomic databases  ⭐ most turnkey big data
| Database | Scale | Link |
|---|---|---|
| **Quadrupia** | ~140.2M G-quadruplexes across ~108k genomes | [GitHub](https://github.com/Georgakopoulos-Soares-lab/Quadrupia) |
| **invertiaDB** | 30.1M inverted repeats across 118k genomes (hairpins/cruciforms) | [NAR 2025](https://academic.oup.com/nar/article/53/8/gkaf329/8118974) |
| **MPRAbase** | 17.7M regulatory elements, 130 MPRA experiments, 35 cell types, 4 organisms | [paper](https://genome.cshlp.org/content/early/2025/04/22/gr.280387.124.abstract) |
| **Microsatellites Explorer** | Short tandem repeats across ~117k genomes | [paper](https://www.sciencedirect.com/science/article/pii/S2001037024003611) |
| **kmerDB** | Billions of genomic/proteomic k-mers per species | [CSBJ 2024](https://www.sciencedirect.com/science/article/pii/S2001037024001314) |
| **metagRoot** | Protein families from plant-root microbiomes | [NAR 2025](https://academic.oup.com/nar/advance-article/doi/10.1093/nar/gkaf862/8245223) |
| **Darling v2.0** | Gene–disease–drug associations from literature | [paper](https://www.sciencedirect.com/science/article/pii/S2001037025002405) |
| all | central index | [lab databases page](https://www.georgakopoulos-soares-lab.com/databases) |

- **Access 🟢.** Millions of labeled records, cross-species splits available for free. Cleanest path to a large, honest Kaggle leaderboard (sequence → non-B structure, or sequence → MPRA activity).

---

## 3. Supporting public datasets (to satisfy "integrate more data")

- **Neuromodulation:** PD-rat longitudinal DBS ephys+behavior (Sci Data, 🟢); human STN-LFP sets on OpenNeuro; broader SPARC autonomic catalog (🟢/🟡).
- **Consciousness/EEG:** anesthesia EEG (propofol/xenon/ketamine); Sleep-EDF Expanded (🟢); CHB-MIT (🟢); PsiConnect psilocybin ds006110 (🟢); DMT EEG-fMRI (🔵).
- **Vision decoding:** THINGS-fMRI, THINGS-MEG, Alljoined-1.6M (🟢) alongside THINGS-EEG2/NSD.
- **Bioelectricity:** voxcraft-sim + Evolution Gym for unlimited simulated morphology↔fitness data (🟢).
- **Genomics:** any of the 7 Georgakopoulos-Soares DBs cross-integrate; add ENCODE/Ensembl for annotations (🟢).

---

## 4. What this changes about the shortlist

| Idea (from shortlist) | Data verdict now |
|---|---|
| **VNS dose→response (Ludwig)** | **Upgraded.** Real Ludwig-authored SPARC data (#349, #305, #375) exists and is public — no benchmark built on it. Strongest "professor's own data" story. Data 🟡 (wrangling needed). |
| **EEG→Image (THINGS-EEG2)** | Confirmed 🟢, established 200-way protocol. Lowest risk. |
| **Xenobot fitness surrogate (Levin)** | Confirmed 🟢 (simulated). Pair with **PlanformDB** (🟢/🟡) for a second, lab-owned Levin task. |
| **Non-B DNA / MPRA (Georgakopoulos-Soares)** | **Upgraded.** 7 downloadable DBs, up to 140M records. Most turnkey big dataset here. |
| **Consciousness classifier + PCI (WISC)** | Metric code 🟢, but flagship PCI/sleep corpus 🔴 → needs lab partnership for the ambitious version; safe version uses public anesthesia/sleep EEG. |
| **DBS biomarker (Ludwig)** | 🟡 — PD-rat dataset public; human LFP scattered on OpenNeuro. |

**Bottom line for "impress the professor with their own lab's data":**
1. **Ludwig** → build the first ML benchmark on his public SPARC VNS recordings.
2. **Georgakopoulos-Soares** → biggest, cleanest data; instant leaderboard.
3. **Levin** → PlanformDB regeneration prediction (his curated DB) + simulated xenobot surrogate.
4. **Krishnaswamy** → best as advisor on a methods/generation challenge.
5. **WISC** → open metric + public EEG for the safe version; partnership required for the gated corpus.

---

## Open question for you
Do you want me to (a) **verify exact licenses + file formats** for the Ludwig SPARC datasets and the Georgakopoulos-Soares DBs (the two most Kaggle-ready), and/or (b) turn the top 2–3 into **full ML-X-Nexus proposals** with concrete splits, baseline model, and 9-week milestones?
