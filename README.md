# IIT414W · Lab 0 · Reproducible setup

Vicente Rodríguez · Individual submission · Unit I, Week 1

**Repository:** `https://github.com/remplenque/AI-Workshop` (public, deliberately — the package contains no tokens, credentials or personal data)
**Submitted commit:** `3276771564428a7d45f95e7edf735cb2993a2aac` — the commit containing all submitted code, notebooks and explanations. The only later commit on `main` is the one that writes this ID into the README, which cannot reference itself.

---

## 1 · Environment

| Item | Observed value |
|---|---|
| Python | 3.13.5 (Anaconda distribution, Linux) |
| Kernel selected | `base` |
| pandas | 2.2.3 |
| numpy | 2.1.3 |
| requests | 2.32.3 |
| fastf1 | 3.8.3 |
| ipykernel | 6.29.5 |
| Git | 2.53.0 |

These are the versions actually recorded by the notebooks, in `outputs/setup_evidence_*.json` (`package_checks`) and in each `outputs/w01_fri_*/run_manifest.json` (`versions`). They are the pins in **`requirements.txt`**, which is the dependency file for this submission.

The repository also contains the course-supplied `requirements_w01_fri_v1.txt`. That file is the lecturer's reference environment (Windows, Python 3.12.4, pandas 2.3.1, fastf1 3.5.3) and is kept for attribution only — it does **not** describe the environment used here. Where the two disagree, `requirements.txt` is the one that reproduces these runs.

### Setup

```bash
python -m pip install -r requirements.txt
python -m ipykernel install --user --name iit414w --display-name "Python (IIT414W)"
```

The notebooks never install anything themselves.

A note on the kernel name: I ran these notebooks on my existing Anaconda base environment, which my editor lists as `base` (full label `base (Python 3.13.5) ~/anaconda3/bin/python`), and that is the name recorded in `metadata.kernelspec` in both notebooks. The `ipykernel install` line above registers a separate, clearly named kernel for anyone reproducing this from scratch. Either works — what matters is that the interpreter matches the pins in `requirements.txt`.

## 2 · Layout and entry points

```
.
├── README.md                  <- you are here
├── PROMPTS.md                 <- AI-use record (single location)
├── requirements.txt           <- dependency file for this submission
├── requirements_w01_fri_v1.txt<- lecturer's reference pins; NOT this environment (see §1)
├── .gitignore
├── .iit414w-root              <- project root marker (hidden; required by both notebooks)
├── START_HERE_W01_Fri_v2.md   <- supplied course entry page
├── unit_I/week_01/
│   ├── W01_Thu_setup_and_reproducibility_v2.ipynb   <- run FIRST
│   ├── W01_Fri_f1_data_ecosystem_v1.ipynb           <- run SECOND
│   ├── w01_fri_support_v1.py                        <- supplied helper (lecturer's code)
│   ├── W01_Fri_DataDictionary_v1.md
│   └── W01_Fri_runbook_v1.md
├── data/samples/w01_fri_v1/   <- Monza 2021 snapshot CSVs + manifest (offline fallback)
├── outputs/                   <- all saved evidence
└── handouts/ templates/ guides/   <- supplied course materials
```

Both notebooks locate the project by walking up from the working directory until they find `.iit414w-root`, so every path is relative. Do not run them from outside this folder and do not delete the marker — it is hidden in most file explorers.

**Notebook order:** Thursday first (environment), Friday second (data checks). Friday does not depend on Thursday's variables, but the environment evidence comes from Thursday.

## 3 · Data mode

`MODE = "live"` in the Friday notebook. On a machine with network this attempts Jolpica over HTTPS and the FastF1 session loader. **Without network the same cell falls back to the verified snapshot in `data/samples/w01_fri_v1/`, prints the failure category, and continues** — the snapshot files ship with this package precisely so that path works. `fetch_jolpica()` always needs network, so the live route itself cannot be reproduced offline; the snapshot fallback is the offline route.

The FastF1 cache (`data/cache/`) is **not** included: 6.2 MB, of which 5.3 MB is an HTTP cache database. The brief excludes large caches. FastF1 rebuilds it on demand.

## 4 · Where outputs are saved

| Artefact | Path |
|---|---|
| Thursday evidence | `outputs/setup_evidence_<timestamp>.json` |
| Friday run folder | `outputs/w01_fri_<timestamp>_<id>/` — `results.csv`, `laps.csv`, `checks.csv`, `run_manifest.json` |

Every execution writes a **new** timestamped file or folder; nothing is overwritten. All runs are kept.

## 5 · Restart and run-all

I saved the notebooks, used **Restart Kernel and Run All Cells** on each in the order above, and confirmed every cell executed in sequence without an unexpected error. The saved outputs in both notebooks come from that run. No manifest can verify this for me — `run_manifest.json` records `"student_restart_run_all": "NOT VERIFIED BY THIS CELL"`, and the Thursday evidence records `"restart_and_run_all": "NOT CHECKED"`. This paragraph is the record.

### Run / repeat comparison

| Run | Folder | Notes |
|---|---|---|
| Class run, 4 Sep | `outputs/w01_fri_20260904T153933_555050Z_f37be2/` | live route |
| Final run, 5 Sep | `outputs/w01_fri_20260905T221501_963380Z_1b4d38/` | live route, clean restart/run-all |

The `files_sha256` blocks of the two runs are identical for all three files (`results.csv`, `checks.csv`, `laps.csv`), across runs a day apart. Folder names and `created_utc` differ, as expected. The live endpoint returned the same data on both occasions, so no difference had to be reconciled. A live endpoint may legitimately revise its data between calls; a hash difference there is documented, not forced to match.

---

## 6 · Evidence notes

### Source

Both tables describe the **2021 Italian Grand Prix at Monza** — `season = 2021`, `round = 14`, `circuit_id = monza`. They came from two different routes and are reported separately:

- **Results** — `origin: JOLPICA_HTTP`, `api_access_confirmed: true`, one complete page, 20 rows, from `https://api.jolpi.ca/ergast/f1/2021/circuits/monza/results/`. **One row = one driver's classified result in one race**, keyed by `season + round + driver_id`.
- **Laps** — `origin: FASTF1_SESSION`, `api_access_confirmed: "NOT VERIFIED: library may use cache"`. **One row = one driver on one lap of the race session**, keyed by `season + round + driver_number + lap_number`. A successful FastF1 load is *not* evidence of a new network request; the library may have read its own cache.

The `data/samples/w01_fri_v1/` CSVs are a **stored copy** captured by the lecturer, verified here by SHA-256 — not live API access and not my own retrieval. The `synthetic` mode of this notebook is **invented teaching data** and was not used for any submitted evidence. These three things are kept distinct throughout.

### Check

**Check:** the independent check in the Friday notebook, cell "Your independent check" (the code cell immediately below it) — does every driver in `results` also appear in `laps`?

**Observed result:** `Drivers in results: 20` · `Drivers in laps: 19` · `In results but no laps: ['22']`. Driver `22` (tsunoda) has a result row with `grid = 0`, `position = 20`, `position_text = 'W'`, `status = 'Brakes'` — a withdrawal.

**What it establishes:** the lap driver set is a subset of the results driver set, so `driver_number` can be used to reach lap rows from a result row without producing unmatched lap records. It also explains one of the two entries behind the helper's `Grid zero` REVIEW count.

**What it does not establish:** that the lap table is complete. It never checks whether the other 19 drivers have all of their laps — gasly has only 3 rows, consistent with a retirement but unverified by this check. Re-running the same check against the supplied snapshot returns `[]` rather than `['22']`, because that capture used FastF1 3.5.3 and kept tsunoda's single empty-time lap row while my 3.8.3 does not return one. The answer therefore depends on the library version, not only on the race.

The gap is not introduced by my code: `fetch_fastf1` in the supplied helper copies `session.laps` into the frame without filtering. FastF1 3.8.3 logs `Finished loading data for 20 drivers` — `22` among them — and also `Failed to perform lap accuracy check - all laps marked as inaccurate (driver 22)`, yet returns no lap rows for that driver.

A WARNING or a REVIEW row is not a failure. `Missing lap times` (35) and `Grid zero` (2) are flags to investigate, and investigating them is what produced the note above.

### Decision and verification

**Decision:** I kept `MODE = "live"` for the submitted runs rather than switching to `MODE = "snapshot"`, because the live route is what my machine actually did and its provenance is recorded honestly. The snapshot files are shipped as the documented fallback so the package still works without network.

**Correction made after self-review:** my first answer to the source question pasted the `JOLPICA_HTTP` provenance and stopped there. It gave no limitation, and it said nothing about where the lap table came from, so a reader could reasonably take `api_access_confirmed: true` as covering both tables. It does not — the laps came from `FASTF1_SESSION`, whose provenance reads `"NOT VERIFIED: library may use cache"`. I added a limitations block that reports the two origins separately and states what a single complete page does and does not establish. In the same pass I made explicit that Bottas's 16 classified places gained are not 16 overtakes: the table records no overtakes, and five of the twenty entries did not finish normally.

**How I verified it:** I re-ran the independent check against the supplied snapshot and compared the two lap tables. My live run: 891 rows, 19 drivers. Snapshot: 892 rows, 20 drivers. The extra row is tsunoda's lap 1 with an empty `lap_time_s` and `is_accurate = False`. My FastF1 is 3.8.3; the snapshot manifest was captured on 3.5.3. **Limit:** this identifies the library version as the difference between the two tables; it does not establish which of the two is the more faithful record of the session, and neither was checked against an official classification.

---

## 7 · AI use

See **[PROMPTS.md](PROMPTS.md)** — single location, not duplicated here.

## 8 · Attribution

`unit_I/week_01/w01_fri_support_v1.py`, both notebook scaffolds, the data dictionary, runbook, handouts, templates, guides and `data/samples/w01_fri_v1/` are course-supplied materials by the lecturer; their attribution is preserved unchanged. My own contribution is the written answers, the independent check, `requirements.txt`, `.gitignore`, this README, `PROMPTS.md` and the executed outputs.
