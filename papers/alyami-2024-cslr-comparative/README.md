# Alyami & Luqman (2024) — Comparative Study of CSLR Techniques: reproduction

**Paper ID:** `8d2b672d06ddb561f890798d7c1e434389c34202`

**Citation:** Sarah Alyami and Hamzah Luqman. *A Comparative Study of Continuous Sign Language Recognition Techniques.* arXiv:2406.12369v1, 2024. A revised version appeared in Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV) Workshops, 2025, pp. 4923–4932.

**Paper:** https://arxiv.org/abs/2406.12369v1 · **Code/artifacts:** the study did not publish its own code. Each evaluated model's official repository is used (see Source provenance).

**Preference level:** 2

The preference level is expected rather than confirmed; see `status.preference_level_basis`.

**Pipeline status:** not yet set. The attempt is paused at the open compute gate `compute-full-table`. AGENTS.md forbids `blocked_on_compute` before the `#repro-sign-team-r` discussion, and no run has been launched.

**Numerical agreement:** `not_assessed` — no target has been produced.

**Attempt date:** 2026-10-26

## Reproduction agents

| Agent ID | Model and version | Agent application | Contribution | Attribution evidence / unknowns |
| --- | --- | --- | --- | --- |
| `claude-opus-5-5-claude-code` | Claude Opus 5.5 (`claude-opus-5-5`) | Claude Code 2.1.310 | Assignment intake, version resolution, source search, dataset inspection on Modal `repro-sign`, target/protocol/compute gates | Session system context names the model and ID. `claude --version` returned 2.1.310. No runs executed. |

## Scope and target contract

The assignment is a single "reproductions" export record together with the arXiv v1 PDF. The record's `what_to_reproduce` is "Table 1".

The record has paper `status: final` but no `confirmation` field, and it is not a top-level candidate array. `ingest_candidate.py` therefore cannot accept it. It is recorded as a `direct_user_request` that preserves the export with emails redacted, following the precedent in `papers/pu-2024-siformer`.

**Target version (gate `target-table-version`, resolved).** The two paper versions have materially different Table 1s:

- **arXiv v1 Table I:** 5 models × 6 settings.
- **ICCVW 2025 Table 1:** 7 models × 8 settings. It adds Swin-MSTP, SlowFastSign and CSL-Daily, uses different epochs, and changes some values.

The operator chose the arXiv v1 Table I, which is the supplied PDF. That gives 30 targets: VAC, SMKD, TLP, SEN and CorrNet, each evaluated on six settings:

- Phoenix2014 Signer-Dep
- Phoenix2014 Signer-Indep (SI5)
- ArabSign Signer-Dep
- ArabSign Signer-Indep (mean of 6 leave-one-signer-out folds, Table II)
- GrSL Signer-Indep (signer 3)
- GrSL Unseen-Sent

The metric is test WER in percent: sclite corpus-level (S+D+I)/N, Eq. 1. None of the values is a copied baseline. The paper re-ran the Phoenix Signer-Dep column itself, and the other columns are new benchmarks. The paper states no seeds or checkpoint rule.

**Protocol (gate `protocol-lr-epochs`, resolved).** The paper says it "followed the same experimental settings described in each study". The same paragraph then states Adam, lr 1e-3, 40 epochs, ×0.2 at epochs 20 and 35, and batch 2 for all models. Every pinned upstream config instead uses lr 1e-4, and TLP/SEN train for 80 epochs (steps 40/60). The operator chose the paper-stated protocol for every model: lr 1e-3 and 40 epochs with steps 20/35. Everything else is each repo's unchanged config.

| Target ID | Paper location | Published WER |
| --- | --- | ---: |
| vac-phoenix2014-signer-dep / -signer-indep | Table I | 21.94 / 40.40 |
| vac-arabsign-signer-dep / -signer-indep | Table I (SI = Table II avg) | 0.2787 / 6.97 |
| vac-grsl-signer-indep / -unseen-sent | Table I | 29.60 / 74.92 |
| smkd-phoenix2014-signer-dep / -signer-indep | Table I | 22.31 / 43.08 |
| smkd-arabsign-signer-dep / -signer-indep | Table I | 0.2091 / 4.24 |
| smkd-grsl-signer-indep / -unseen-sent | Table I | 30.80 / 61.10 |
| tlp-phoenix2014-signer-dep / -signer-indep | Table I | 22.00 / 35.70 |
| tlp-arabsign-signer-dep / -signer-indep | Table I | 0.2439 / 2.90 |
| tlp-grsl-signer-indep / -unseen-sent | Table I | 6.40 / 75.50 |
| sen-phoenix2014-signer-dep / -signer-indep | Table I | 20.20 / 34.40 |
| sen-arabsign-signer-dep / -signer-indep | Table I | 0.2962 / 7.22 |
| sen-grsl-signer-indep / -unseen-sent | Table I | 8.09 / 74.80 |
| corrnet-phoenix2014-signer-dep / -signer-indep | Table I | 19.90 / 33.60 |
| corrnet-arabsign-signer-dep / -signer-indep | Table I | 0.2265 / 7.65 |
| corrnet-grsl-signer-indep / -unseen-sent | Table I | 1.50 / 64.70 |

## Source provenance

| Artifact | Canonical source | Pinned revision / SHA-256 | Role |
| --- | --- | --- | --- |
| Paper PDF (target) | https://arxiv.org/pdf/2406.12369v1 | `db652a81563f9e5c7901ba37df4b69164c28c9acfae858ffff682441ae9eecf6` | Target and protocol |
| Paper PDF (ICCVW 2025) | openaccess.thecvf.com, ICCV2025W/MSLR | `922a2038faac61b3eacbefb5be417e5dbf126b3655814005189f06391dd5aa63` | Version comparison only |
| VAC + SMKD code | https://github.com/VIPL-SLP/VAC_CSLR | `71f3e0334fbc8cecc7ce9816ec69781068abaac0` | VAC; SMKD via `share_classifier`/`weight_norm` (README) |
| TLP code | https://github.com/hulianyuyy/Temporal-Lift-Pooling | `21be215c6ba749d98ce6a51f4b84cc9d5029bfe6` | TLP |
| SEN code | https://github.com/hulianyuyy/SEN_CSLR | `c9a4c7ebd0489e22496c00f9f31d930c0d968ad8` | SEN |
| CorrNet code | https://github.com/hulianyuyy/CorrNet | `a812814f4deefda0b75ab1dcc90b106c62912263` | CorrNet |
| ArabSign split CSVs | https://github.com/Hamzah-Luqman/ArabSign `ArabSign-EncoderDec.zip` | repo `8f6e127`, zip `6980aec2…eb0` | Per-signer train/test lists |

Locations searched on 2026-10-26:

- The arXiv abs page, which has only v1 and no code link.
- The ICCVW open-access page, which has no code or supplement.
- The dataset author's GitHub repositories.
- GitHub searches for SMKD, self-mutual distillation, ArabSign and GrSL CSLR.
- An exact-title web search.

None of these turned up a repository for the comparative study itself.

## Results

No target has been produced. All 30 targets are open pending the compute gate.

## How to repeat this

Nothing is executable yet. The container, data population (`data.sh` for GrSL and ArabSign extraction) and Modal entry points will be added after the compute gate is decided.

## Data provenance and permissions

| Dataset | Splits | License / cloud basis | Path in Volume `datasets` | State |
| --- | --- | --- | --- | --- |
| Phoenix2014 v3 | multisigner 5672/540/629; SI5 4376/111/180 | CC BY-NC-SA (RWTH), non-commercial research | `rwth-phoenix-2014/raw/phoenix2014-release` | Present. All SI5 ids resolve to multisigner frame folders. Corpus CSV hashes are in `reproduction.json`. |
| ArabSign (RGB) | Signer-Dep test 1843 (union of per-signer test CSVs); train+val 7492; 6 LOSO folds | Non-commercial; author-granted permission via Team S | `arab-sign` | 7z archives present, not extracted. The paper's 709-sample validation subset is unpublished. |
| Continuous GSL | SI (signer 3) 8821/588/881; unseen-sentence 8189/1063/1043 | CC BY 4.0 (Zenodo 3941811) | `gsl-continuous` (planned) | Absent. Acquisition is permitted but deferred. Split files from the dataset's Drive are hashed in `reproduction.json`. |

The GSL Drive folder labelled "GSL SD" actually holds the `*_unseen` files. None of its 1,043 test sentences occurs in training, and they cover 45 unique sentences. This folder matches the paper's Unseen-Sent split. "GSL SI" holds signer 3 out with seen sentences, matching the paper's Signer-Indep split.

## Environment and patches

Nothing has been built yet. The plan is to start from `ghcr.io/sign-language-processing/reproduction:latest`, reusing the patches from `papers/yu-2025-cvsign` that are needed on current PyTorch: `ctcdecode` built as C++17, and the CTC loss computed without cuDNN. Each patch will be re-justified by this paper's own preflight.

## Execution evidence

No runs.

## Guesses and deviations

| Detail | Paper/evidence says | This attempt will use | Rationale |
| --- | --- | --- | --- |
| Learning rate, TLP/SEN epochs | Paper: lr 1e-3, 40 epochs for all; upstream: lr 1e-4, TLP/SEN 80 epochs | Paper-stated values | Operator decision at gate `protocol-lr-epochs` |
| Seed | Not stated | 0 (upstream default) | Single reported number per cell |
| Checkpoint selection | Not stated | Dev-best per upstream `main.py`; the rule for ArabSign LOSO folds (no dev set) is to be fixed before launch | Upstream behavior |
| ArabSign Signer-Dep validation | 709 samples, unpublished | To be decided before launch | Split not released |

## Attempts, failures, and dead ends

None yet. The operator chose to stop before data population and preflight until the compute plan is approved.

## Candidate flags, ethics, and human evaluation

The queue record flags no human evaluation and no ethical concerns, and `copied_scores` is `no`. The text confirms the paper re-ran every model and copied no baselines. All three datasets are existing recordings released for research, and no participants are involved.

## Author and team contact

None. The ArabSign access shown in the queue record was coordinated earlier by Team S.

## Open gate

`compute-full-table` covers about 55 single-GPU trainings: Phoenix 10, ArabSign Signer-Dep 5, ArabSign LOSO 30, GrSL 10.

The paper reports about 10 h per Phoenix/GrSL run on an RTX A6000, and about half that on ArabSign. The sibling `yu-2025-cvsign` run measured about 33 min/epoch on an L40S for a heavier CorrNet variant.

The projection is roughly 400–800 GPU-hours, or about USD 800–1,600. This needs a decision in `#repro-sign-team-r` before any full run.
