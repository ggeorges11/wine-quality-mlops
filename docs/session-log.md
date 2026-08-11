# Wine Quality ML Project — Status & Session Log

**Charter:** `Wine_Quality_ML_Project_Charter.docx`
**GitHub repo:** https://github.com/ggeorges11/wine-quality-mlops
**Local path:** `~/Projects/wine-quality-mlops`

---

## Current status

**Phase 1.A — Project setup and tooling: COMPLETE**

**Next up: Phase 1.B — Data acquisition and exploration** (not started)

---

## Environment (verified working)

| Item | Value |
|---|---|
| Machine | MacBook, Apple Silicon (`arm64`) |
| Python | 3.13.5 (from Anaconda at `/opt/anaconda3`) |
| Project venv | `~/Projects/wine-quality-mlops/.venv` — 581 MB |
| Anaconda `base` | Auto-activates on shell start; prompt shows `(.venv) (base)` — normal |
| git | 2.39.5 (Apple Git), `credential.helper=osxkeychain` |
| GitHub auth | Credential stored in macOS Keychain — pushes work with no prompt |
| PAT | Created (classic, `repo` scope, 90-day expiry); not needed so far |

### Resuming a session

    cd ~/Projects/wine-quality-mlops
    source .venv/bin/activate

Activation does **not** persist across Terminal windows. If a "but I already
installed that" error appears, check for `(.venv)` in the prompt first.

---

## Installed packages

pandas 3.0.5, numpy 2.5.2, scikit-learn 1.9.0, scipy 1.18.0,
matplotlib 3.11.1, seaborn 0.13.2, jupyterlab 4.6.3, fastapi 0.141.1,
uvicorn 0.52.1, joblib 1.5.3, boto3 1.43.68, requests 2.34.2

Full pinned list (119 packages) in `requirements.txt`.

---

## Decisions made and why

- **venv over conda**, despite Anaconda being installed. Lambda packaging
  consumes `requirements.txt` (pip format); conda's `environment.yml` doesn't
  translate. One format end-to-end. Anaconda left installed but unused.
- **Committed `data/raw/`, gitignored `data/processed/` and `models/`.** Raw data
  is small and public so the repo stays self-contained; derived artifacts are
  regenerable, and the trained model ships via S3.
- **`models/*` plus `!models/.gitkeep` negation pattern** rather than ignoring
  the directory — ignoring a directory outright means git never evaluates the
  negation, so the placeholder would be lost.
- **Repo is public** — it's a portfolio artifact.
- Home-folder convention: capitalized (`~/Projects`); repo names
  lowercase-hyphenated (they become URLs, which are case-sensitive).

---

## Repository structure

    wine-quality-mlops/
    ├── .gitignore          # excludes .venv, derived data, models, credentials
    ├── README.md           # architecture, structure, roadmap with checkboxes
    ├── requirements.txt    # 119 pinned packages
    ├── docs/               # this file
    ├── data/raw/           # dataset goes here (Phase 1.B)
    ├── data/processed/     # gitignored contents
    ├── notebooks/          # exploration and narrative
    ├── src/                # reusable modules — what Lambda will see
    ├── models/             # gitignored contents; deployed via S3
    ├── deployment/         # Lambda packaging, trimmed requirements, IAM notes
    └── tests/

Guiding split: **notebooks are for thinking, `src/` is for code that has to run
somewhere else.**

---

## Known issues flagged for later

1. **Lambda 250 MB limit vs 581 MB venv.** Phase 1.G needs a second, trimmed
   requirements file with inference-only dependencies (no jupyterlab, matplotlib
   or seaborn). Training and serving need different dependency sets.
2. **arm64 vs Lambda.** Local wheels are `macosx_11_0_arm64`; Lambda runs Linux.
   Needs Linux wheels at packaging time (`pip --platform manylinux2014_*` with
   `--only-binary=:all:`, or Docker).
3. **pandas 3.x** is a major version — copy-on-write default, Arrow-backed
   strings. Older course idioms may differ slightly.
4. **macOS is case-insensitive; Linux and AWS are not.** Already caused one
   surprise (`~/projects` vs `~/Projects`). Watch filename casing in S3 and
   Lambda code.
5. **Commit email is public** on this repo. A GitHub `noreply` alias is
   available if preferred; undecided.
6. **Optional later:** add a LICENSE (MIT), and extract this repo into a GitHub
   template repository for reuse on future projects (~20 min, best done at the
   end of Phase 1).

---

## Session 1 log — 2026-08-11

Completed Phase 1.A end to end:

- Verified local toolchain; discovered Anaconda was auto-activating `base`
- Created `~/Projects/wine-quality-mlops` and a project-local `.venv`
- Renamed `~/projects` to `~/Projects`, rebuilt the venv (venvs hardcode
  absolute paths and are not relocatable)
- Installed 12 direct dependencies, pinned 119 total to `requirements.txt`
- Built the folder structure and a `.gitignore` written *before* `git init`,
  so ignore rules applied from the first commit
- Wrote the README with target architecture and a phase roadmap
- `git init`, first commit (`fde4e3b`), pushed to GitHub

Two mishaps worth remembering: a heredoc broke because nested code fences
truncated the paste, and a `YOUR_USERNAME` placeholder got pasted literally.
Both were caught by verifying output rather than assuming success — which is
the habit that matters, and the reason each step ended with a check command.

---

## Next session — Phase 1.B (est. 2–3 hrs)

Download the UCI Wine Quality data (red + white, ~6,500 rows, 11 features plus
a quality score) into `data/raw/`. Explore in JupyterLab: distributions,
correlations, missing values, target balance. **Decide regression vs binary
classification** — a real judgment call, to be worked through rather than
assumed. Relevant context: the quality target runs 0–10 but is heavily
concentrated at 5 and 6, with very few examples at the extremes, which is
precisely why the framing matters. Document findings in a notebook with
markdown commentary.
