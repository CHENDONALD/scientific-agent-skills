# Expert Activation Catalog

The orchestrator reads the task and uses judgment (not keyword matching) to decide which experts to activate. This catalog defines the signals to reason about.

## How to use this catalog

1. Identify the **core activity** of the task (data analysis? modeling? engineering? writing?).
2. Match it to one or more expert profiles below.
3. For each activated expert, read its agent file at `agents/<slug>.md` and adopt that expert's reasoning, failure-mode awareness, and verification discipline for the relevant part of the work.
4. When the task spans multiple domains, activate multiple experts in parallel and merge their perspectives — flag disagreements explicitly rather than averaging them away.

## Core Experts (activate the one matching the primary activity)

### Data Scientist
**Agent file:** `agents/data-scientist.md`
**Activate when** the task involves:
- Translating an ambiguous business/scientific question into an estimand, dataset, and model
- Building reproducible analysis pipelines (sklearn Pipelines, nested CV, leakage prevention)
- A/B testing, experimentation, SRM/AA guardrails, power analysis
- Defining semantic metrics in SQL/warehouse terms
- Producing Model Cards / Datasheets
**Treat as first-class failures:** train-test leakage, Simpson's paradox, peeking, PSI>0.25 drift.
**Do not activate for:** pure software engineering with no data/analysis component.

### Statistician
**Agent file:** `agents/statistician.md`
**Activate when** the task involves:
- Choosing estimators, test statistics, or confidence intervals
- Sample size / power calculations, multiple-comparison correction
- Experimental design (blocking, stratification, randomization)
- Frequentist vs Bayesian framing decisions
- Regression modeling where inference (not just prediction) matters
**Do not activate for:** pure prediction tasks with no inferential claim (Data Scientist covers those).

### Bayesian Statistician
**Agent file:** `agents/bayesian-statistician.md`
**Activate when** the task explicitly involves:
- Hierarchical/multilevel models, partial pooling
- Priors that must be defended (not default-flat), posterior predictive checks
- Stan/PyMC HMC-NUTS fits, PSIS-LOO model comparison, SBC calibration
- Decision-theoretic reporting under coherent uncertainty
**Treat as first-class failures:** divergent transitions/funnels, weak identifiability, label switching, improper posteriors, post-hoc prior tuning.
**Do not activate for:** routine frequentist inference (use Statistician instead).

### Machine Learning Researcher
**Agent file:** `agents/machine-learning-researcher.md`
**Activate when** the task involves:
- Designing ML experiments (hypothesis → ablation → evaluation)
- Population risk, double descent, inductive bias analysis
- Held-out sacred test sets, seed sweeps, benchmark contamination checks
- Reproducibility (NeurIPS/Pineau checklists, HELM/Dynabench-aware reporting)
**Treat as first-class failures:** leakage, meta-overfitting, benchmark contamination, Goodhart gaming, seed variance.

### Deep Learning Scientist
**Agent file:** `agents/deep-learning-scientist.md`
**Activate when** the task involves:
- CNN/Transformer/DiT/MoE architecture design or selection
- FLOPs-matched ablations, loss-landscape analysis (Li et al.), grokking/mode connectivity
- Scaling-law reasoning (Kaplan/Chinchilla ~20 tokens/param)
- Training stack: AdamW+cosine/WSD, Megatron-FSDP/DeepSpeed
- Evaluation: FID/MMLU-Pro/MMLU-CF with lm-eval decontamination
**Do not activate for:** classical ML (scikit-learn tier) — Machine Learning Researcher covers that.

### AI Researcher
**Agent file:** `agents/ai-researcher.md`
**Activate when** the task involves:
- Empirical ML experiment design and evaluation methodology broadly
- Reasoning about evaluation validity, not just model fit
- Cross-cutting AI questions that don't fit a narrower profile

### Computational Scientist
**Agent file:** `agents/computational-scientist.md`
**Activate when** the task involves:
- Simulation, verification & validation (Roache code/solution verification, ASME V&V 10/20/40)
- UQ ensembles, Snakemake/Nextflow/CWL pipelines
- Conda-lock/Apptainer provenance, reproducible computational workflows
**Treat as first-class failures:** environment drift, workflow cache staleness, validation-vs-calibration conflation.

### Research Software Engineer
**Agent file:** `agents/research-software-engineer.md`
**Activate when** the task involves:
- Making research code citable, tested, maintainable (Software Carpentry, FAIR4RS)
- SemVer releases, CITATION.cff/SPDX metadata, pytest/Hypothesis CI gates
- Docker/Apptainer on Slurm
**Do not activate for:** analysis methodology itself — pair with a domain expert instead.

### Data Engineer
**Agent file:** `agents/data-engineer.md`
**Activate when** the task involves:
- Building/maintaining data pipelines (batch & streaming)
- Medallion bronze/silver/gold, Kimball grain, SCD2
- CDC/Debezium, watermark incremental loads, dbt/GX quality gates
- Iceberg/Delta lakehouse MERGE, data contracts, freshness SLIs
**Treat as first-class failures:** silent join drops, duplicate amplification, schema drift, green-DAG-wrong-numbers.

### MLOps Engineer
**Agent file:** `agents/mlops-engineer.md`
**Activate when** the task involves:
- ML lifecycle ops: feature stores (Feast), serving (KServe/Triton)
- Drift monitoring (Evidently PSI/KS), data validation (Great Expectations/TFDV)
- Registries (MLflow/W&B), rollback-readiness, train-serve parity
**Treat as first-class failures:** train-serve skew, data leakage, silent degradation, schema/concept drift.

### Mathematical Modeler
**Agent file:** `agents/mathematical-modeler.md`
**Activate when** the task involves:
- Building mechanistic models (ODE/PDE, stochastic, agent-based)
- Nondimensionalization, conservation/positivity laws, minimal-viable structure
- Identifiability (profile-likelihood, Fisher information), Sobol/Morris sensitivity
**Treat as first-class failures:** sloppy unidentifiable parameters, structural model uncertainty, out-of-regime extrapolation.

### Optimization Scientist
**Agent file:** `agents/optimization-scientist.md`
**Activate when** the task involves:
- Convexity-class reasoning, KKT/complementarity, LP/MIP relaxation gaps
- Interior-point / branch-and-cut (Gurobi, CPLEX, MOSEK, Ipopt)
**Treat as first-class failures:** loose big-M, IntegralityTol cheaters, IIS-hidden infeasibility, nonconvex KKT-as-global, MIPGap-at-TimeLimit-as-optimal.

### Numerical Analyst
**Agent file:** `agents/numerical-analyst.md`
**Activate when** the task involves:
- Well-posedness, discretization, conditioning, stability analysis
- MMS (method of manufactured solutions) and convergence studies for code verification
**Treat as first-class failures:** cancellation, stiffness, solver tolerance floors.

### Computational Physicist
**Agent file:** `agents/computational-physicist.md`
**Activate when** the task involves:
- HPC simulation of physical systems (DFT, MD, Monte Carlo, FEM/FVM)
- Workflows: VASP, LAMMPS, COMSOL, OpenFOAM, Quantum Espresso
- Governing-equation reasoning, discretization, HPC scaling
**Do not activate for:** pure data analysis with no physics governing equations.

## Combination guidance

Common high-value combinations:
- **Data analysis study**: Data Scientist + Statistician (estimand + inference)
- **ML paper**: Machine Learning Researcher + Deep Learning Scientist (method + scaling)
- **Simulation pipeline**: Computational Scientist + Research Software Engineer (workflow + code quality)
- **Production ML**: MLOps Engineer + Data Engineer (serving + pipelines)
- **Modeling study**: Mathematical Modeler + Numerical Analyst (formulation + solving)

When experts disagree, surface the disagreement with each expert's reasoning — do not silently pick one.
