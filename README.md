# Wine Quality Prediction — End-to-End ML Pipeline on AWS

A production-style machine learning service that predicts wine quality from
physicochemical properties. Built as an applied learning project covering the
full lifecycle: data preparation, model selection, API design, cloud
deployment, and automated data ingestion.

**Status:** Phase 1 in progress — project setup complete.

---

## Why this project exists

The goal is demonstrated competence across an entire ML deployment pipeline,
not maximum predictive accuracy. Every architectural choice is made
deliberately and documented, including the choices to keep things simple.

---

## Target architecture

**Phase 1 — direct API access**

    Client (Python / curl / Streamlit)
            |
            v  HTTPS
       API Gateway
            |
            v
       AWS Lambda  ---- loads model artifact ---->  S3 (private bucket)
            |
            v
       Prediction (JSON)

**Phase 2 — automated ingestion**

    Customer drops messy CSV --> S3 input bucket
                                      |
                                      v  (S3 event notification)
                                Cleaning Lambda
                                      |
                                      v
                              Prediction Lambda
                                      |
                                      v
                                S3 results bucket

---

## Repository structure

| Path | Contents |
|---|---|
| `data/raw/` | Original UCI dataset, never modified |
| `data/processed/` | Cleaned train/validation/holdout splits (generated, gitignored) |
| `notebooks/` | Exploratory analysis and model development |
| `src/` | Reusable modules — data prep, training, inference |
| `models/` | Trained model artifacts (gitignored; deployed via S3) |
| `deployment/` | Lambda packaging, trimmed requirements, IAM notes |
| `tests/` | Tests for data prep and inference logic |

---

## Dataset

UCI Wine Quality dataset — approximately 6,500 records of red and white
Portuguese "Vinho Verde" wines, with 11 physicochemical features and a
quality score from 0 to 10 assigned by wine tasters.

Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009).
*Modeling wine preferences by data mining from physicochemical properties.*
Decision Support Systems, 47(4), 547-553.

---

## Local setup

Requires Python 3.9 or newer.

    git clone <REPO_URL>
    cd wine-quality-mlops

    python3 -m venv .venv
    source .venv/bin/activate
    pip install --upgrade pip
    pip install -r requirements.txt

On Windows, activate with: .venv\Scripts\activate

---

## Live API

Not yet deployed. Endpoint URL, request schema, and sample response will be
documented here on completion of Phase 1.G.

---

## Roadmap

**Phase 1 — build and deploy a working model**

- [x] 1.A Project setup and tooling
- [x] 1.B Data acquisition and exploration
- [ ] 1.C Data preparation
- [ ] 1.D Model building
- [ ] 1.E Local API wrapping (FastAPI)
- [ ] 1.F AWS environment setup (IAM, S3, CLI)
- [ ] 1.G AWS deployment (Lambda, API Gateway)
- [ ] 1.H Client integration and end-to-end test

**Phase 2 — automated data preparation pipeline**

- [ ] 2.A Define realistic messy-input scenario
- [ ] 2.B Data ingestion bucket with event triggers
- [ ] 2.C Transformation logic (Lambda, optionally Glue)
- [ ] 2.D Results delivery
- [ ] 2.E End-to-end demonstration
- [ ] 2.F Cleanup and architecture documentation

---

## Explicitly out of scope

Deep learning (unjustified for tabular data at this scale), real-time
streaming, production-grade security hardening, CI/CD, and drift monitoring.
Each is a deliberate exclusion rather than an oversight.

---

## Author

Gilles Georges — built to support applied AI/ML advisory work.
