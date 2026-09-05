# PROMPTS.md · AI-use log

Activity: IIT414W Lab 0 · Reproducible setup · Dates: 2026-09-04 to 2026-09-05 · Author: Vicente Rodríguez

AI use: **Used.** Recorded here in one location; the README links to this file and does not repeat it.

## Tools used

| Tool | Model/version, if known | Purpose |
|---|---|---|
| Claude Code (CLI, in VS Code) | Reported as Claude Opus 5. I could not independently verify the model version, so I record it as reported, not as confirmed. | (1) Explaining the Lab 0 brief and rubric so I knew which fields were still empty. (2) Drafting the independent-check cell in the Friday notebook and the packaging files. (3) Reviewing the finished package against the rubric before I committed it. |

## Interaction log

### Entry 1 · Understanding what Lab 0 actually requires

1. **Context:** After Friday's class I had two partly-completed notebooks and no README, dependency file or repository. I wanted to be sure I understood what each rubric criterion asks for before writing anything, rather than guessing from the notebook prompts alone.
2. **Prompt sent:** Faithful summary — I asked it to read the Lab 0 brief, rubric and self-checklist in `handouts/`, plus the assignment PDF, and to tell me explicitly which fields I still had to complete and what each rubric criterion needed as evidence.
3. **Output received:** A mapping of the four criteria (C1 environment 30, C2 repository 30, C3 reproducibility 25, C4 AI disclosure 15) to specific missing artefacts, and a list of the unanswered fields in both notebooks.
4. **What I accepted and why:** The identification of what was missing — no `requirements.txt`, no Git repository, no README, no AI-use record, and unanswered written fields in both notebooks. Each of these I confirmed myself by looking at my own folder; the folder had none of those files.
5. **What I modified and why:** The suggested data route. It first proposed switching to `MODE = "snapshot"` for portability. I kept `MODE = "live"`, because that is the route my machine actually ran in class and the outputs were already saved; the rubric states that live access earns no bonus and snapshots earn no penalty, so the honest choice is to report what I did.
6. **What I rejected and why:** A suggestion to make the repository private and invite the lecturer as a collaborator. I chose a public repository instead, having checked that the package contains no tokens, credentials or personal data, because a public link is simpler to submit and verify.
7. **Verification:** I checked the claimed gaps directly against my own folder listing before acting on any of them, and I read the rubric descriptors in `handouts/Lab0_Rubric.md` myself rather than relying on the summary.
8. **Limitations:** Understanding the rubric is not the same as satisfying it. Nothing in this entry produced evidence; the evidence comes from my own executed notebooks.

### Entry 2 · Drafting the packaging files

1. **Context:** I had no README, dependency file or `.gitignore`, and the brief requires a runbook with three specific evidence notes.
2. **Prompt sent:** Faithful summary — I asked it to draft `requirements.txt`, `.gitignore` and a README containing the setup commands, kernel, notebook order, data mode, output locations and the three evidence notes, using my observed versions.
3. **Output received:** Drafts of the three files, with the version pins taken from my own `run_manifest.json` and `setup_evidence` file.
4. **What I accepted and why:** The structure and the exclusion list (`.git`, `__pycache__`, `data/cache/`, virtual environments), which matches what the brief forbids in the ZIP.
5. **What I modified and why:** Every factual claim in the README — repository URL, commit ID, kernel name, run folder names and the hash verdict are mine to fill from my own runs, and I did not leave any of them as a generated guess.
6. **What I rejected and why:** Nothing substantive in this entry.
7. **Verification:** I compared each pin in `requirements.txt` against the `versions` block of my own `run_manifest.json` and the `package_checks` block of my `setup_evidence` file: Python 3.13.5, pandas 2.2.3, numpy 2.1.3, requests 2.32.3, fastf1 3.8.3, ipykernel 6.29.5. They match.
8. **Limitations:** The dependency file records what worked on my Linux/Anaconda machine. It is not evidence that these exact pins install cleanly on another operating system.

### Entry 3 · Review pass before committing

1. **Context:** Before packaging I wanted the whole submission checked against the rubric, rather than trusting that the pieces still agreed with each other after several rounds of editing.
2. **Prompt sent:** Faithful summary — I asked it to verify every factual claim in the README, this file and both notebooks against my saved outputs, and to flag anything inconsistent, stale or missing.
3. **Output received:** Four problems. My answer to the source question had no limitation and never said where the lap table came from. The README described a correction my notebook no longer contained. Three intermediate evidence runs were clutter. The README's run table pointed at a folder I was about to delete.
4. **What I accepted and why:** All four. I confirmed each against the files before changing anything — the source answer really did stop at the pasted provenance, and the run table really did name a folder that no longer exists.
5. **What I modified and why:** It drafted the limitations block for the source answer; I reworded it and kept the facts, because the wording had to match the rest of my notebook.
6. **What I rejected and why:** A suggestion to identify the Bottas row by `driver_id` and `driver_number` instead of by position and team. I asked the lecturer, who said that was not the point of that question, so I kept my own row description.
7. **Verification:** After the fixes I confirmed both notebooks still show execution counts `1`–`6`, that `outputs/` holds exactly the first and the latest run of each notebook, and that the README's run table names folders that exist on disk.
8. **Limitations:** A review against the published rubric is not the lecturer's assessment, and it cannot tell me whether my explanations actually read as my own understanding.

## Summary reflection

- **Most useful assistance:** having the rubric descriptors mapped to concrete missing artefacts, which is what showed me that my source note covered only the results table and not the laps table.
- **Least reliable assistance:** the initial recommendation to switch data route. It optimised for portability and ignored that I already had honest live outputs; the rubric explicitly gives no bonus for either route.
- **A decision I made myself:** keeping `MODE = "live"`, and treating the 891-vs-892 lap-row difference as evidence to document rather than a discrepancy to eliminate.
- **Next verification or improvement:** check whether the FastF1 3.5.3 and 3.8.3 lap tables differ anywhere other than the tsunoda row, before reusing this data in Lab 1.
