# mlm26proposals

Draft project proposals for the **2026 UW–Madison Machine Learning Marathon (MLM26)**, focused on **neuroscience, consciousness, bioelectricity, neuromodulation, and genomics** — with challenges designed to be useful to specific research labs (Michael Levin @ Tufts, Kip Ludwig @ UW–Madison/WITNe, the Wisconsin Institute for Sleep & Consciousness, and open-genomics groups).

---

## What is the ML Marathon?

The [Machine Learning Marathon](https://ml-marathon.wisc.edu/) is UW–Madison's annual, **semester-long ML/AI hackathon** (MLM26 is the 4th edition, running roughly **September → December**). Teams of up to 5 pick a challenge and work through it together, supported by hands-on cloud workshops (AWS, Google Cloud), compute credits, ML/AI and domain-expert advisors, and the **ML+X Nexus** resource hub. Most challenges are **sourced from or inspired by UW–Madison research labs**, capturing the Kaggle spirit of "learning by doing." It's open to anyone affiliated with UW–Madison, from complete beginners to experts.

### What a challenge must be (format rules)

Challenges are hosted on **Kaggle's free competition format**, which shapes what a valid proposal looks like:

- **One clearly defined predictive task** — classification, regression, or detection. Open-ended "explore the dataset" goals are **not** allowed.
- **A measurable metric** (e.g., accuracy, F1, AUROC, RMSE, Pearson r) and a **held-out test set** for the leaderboard.
- **Single-stage leaderboard only** — no multi-phase competitions.
- **External / additional datasets** can be encouraged as a **bonus/secondary track**, but the primary leaderboard metric must stay narrow and well-defined.

### Key dates (per planning notes)

- Interest form to reserve a challenge slot (early).
- Data uploadable to **Kaggle by ~Aug 1**.
- Full draft (problem description, data, metric, sample submission) due **~Aug 10**.

*(Confirm current-year dates on the official site before relying on them.)*

---

## What this repo is for

This repo collects **candidate challenge proposals** plus the supporting research that produced them. Each proposal is built to satisfy three hard requirements:

1. **A quality, Kaggle-uploadable dataset** (with source + access status).
2. **Published evidence** the underlying problem is genuinely open.
3. **A narrow predictive task + measurable metric + held-out test** (the benchmark).

Proposals are also chosen so a **target professor would personally benefit** from the result — ideally by building on **their own lab's data** — so the labs have a real motivation to advise or co-organize.

---

## The proposals (see [`proposal.md`](proposal.md))

| # | Proposal | Area | Target lab | Primary dataset |
|---|---|---|---|---|
| 1 | **Anatomical Compiler v0** — a fast emulator of bioelectric pattern formation | Bioelectricity | Michael Levin (Tufts) | Generated with **BETSE**; validated on **PlanformDB** |
| 2 | **Dialing the Vagus** — predict VNS therapy vs. side-effect from stimulation parameters | Neuromodulation | Kip Ludwig (UW–Madison/WITNe) | **NIH SPARC** vagus-nerve recordings |
| 3 | **Reading the Genome's Hidden Structures** — predict *active* non-B DNA from sequence | Genomics | Georgakopoulos-Soares lab | **Quadrupia** + **invertiaDB** |
| 4 | **NeuroAgent-Bench** — a harness + benchmark for AI agents that run neuroscience workflows | AI harness | Lab-agnostic infrastructure | Task suite over public neuro datasets |

Each write-up follows the [ML-X-Nexus proposal format](context/project_proposal_structure.md) and includes a pressure-test of data feasibility, a benchmark/metric design, a 9-week timeline, and a professor hook.

---

## Repo structure

```
.
├── proposal.md                 # ← the final, pressure-tested proposals
├── README.md
├── context/
│   └── project_proposal_structure.md   # reference: how an MLM proposal should look
├── research/                   # supporting research & earlier iterations
│   ├── proposals_ranked_shortlist.md       # broad ranked idea list
│   ├── landscape_datasets_benchmarks.md    # dataset & benchmark landscape (incl. lab-owned data)
│   └── proposals_v2.md                     # intermediate formatted draft
└── notes/                      # original seed notes / inputs
    ├── biotech.md
    ├── fmriidea.md
    └── neuroscience_seed_ideas.md
```

---

## Status

Draft / work-in-progress. Next steps: pick a lead proposal, confirm dataset licenses + formats, and draft the Kaggle problem description + sample submission + baseline notebook.
