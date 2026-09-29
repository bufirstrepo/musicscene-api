# AI Dev Team Kit

A reusable prompt that turns an AI coding agent into a full engineering team for any of your projects. The agent:

- writes itself a rulebook (`devteam/PLAYBOOK.md`) and works from it;
- maps the codebase and reviews every file through 10+ expert lenses;
- writes down every problem it finds without stopping to fix it;
- then goes back and fixes the confirmed problems in priority order, proving each fix with a test;
- and hands you a report.

| File | What it is |
|---|---|
| `MASTER_PROMPT.md` | The full prompt: boot steps, the Playbook the agent creates, the 14-role team, protocols, templates, and profiles for your four projects. |
| `README.md` | This page: how to run it, how to resume, and the settings you can change. |

## Run it (first session)

**Option 1 (recommended): works with Claude Code, Cursor, Codex, or any agent that can read files.**
1. Copy this `ai-dev-team/` folder into the root of the project you want reviewed.
2. Tell the agent:
   ```text
   Read ai-dev-team/MASTER_PROMPT.md in full and execute it, starting with Part A.
   ```
   If your tool can run sub-agents (Claude Code can), add: `Use sub-agents for the review lanes (Mode A).`

**Option 2: paste.** Paste the whole of `MASTER_PROMPT.md` as your message.

## Resume in a later session

Paste this:

```text
Continue the devteam review program in this repo.
1. Read devteam/PLAYBOOK.md in full. It is your operating manual and overrides your defaults.
2. Read devteam/STATUS.md and the last 60 lines of devteam/JOURNAL.md.
3. Reconcile with `git status` and `git log --oneline -15`.
4. Continue from "Next actions" in STATUS.md, following the Playbook's phases, gates and protocols exactly.
If I answered questions, my answers are in devteam/QUESTIONS.md. Apply them first.
```

## What you get: a `devteam/` folder in your project

| File | What's in it |
|---|---|
| `PLAYBOOK.md` | The agent's rulebook for your project: how it goes through the code and how it looks at it. |
| `STATUS.md` | Where things stand right now, and what happens next. |
| `ISSUES.md` | Every problem found, with file:line evidence, severity, and fix history. |
| `MAP.md` | How your codebase is laid out and how the pieces connect. |
| `ARCHITECTURE.md` | Design review, the structure the code *should* have, and a step-by-step plan to get there. |
| `COVERAGE.md` | Proof of what was actually read, file by file, with line ranges. |
| `BASELINE.md` | Install/build/test results before and after the fixes. |
| `QUESTIONS.md` | Decisions only you can make. Answer them right in the file. |
| `IDEAS.md` | Feature and improvement opportunities spotted along the way. |
| `REPORT.md` | The final report: grades before → after, what was fixed, what's left. |
| `JOURNAL.md`, `notes/`, `findings/`, `repro/` | The team's working notes. |

## How the run works

| Phase | What happens |
|---|---|
| 0 Boot | Creates the workspace and the Playbook. |
| 1 Recon | Maps every file and records whether it installs, builds and tests. |
| 2 Flows | Traces the critical user journeys end to end. |
| 3 Deep review | Every specialist reviews its lens across the code. Problems are logged, never fixed here. |
| 4 Triage | Removes duplicates, double-checks every critical finding, and builds the fix queue. |
| 5 Fix | Goes back through the ledger: failing test → fix → all tests → commit → independent verification. |
| 6 Regression | Re-runs everything and compares with the baseline. |
| 7 Report | Writes REPORT.md and hands off. |

## Settings you can change

Edit `devteam/PLAYBOOK.md`, section **B1**:

| Setting | Default | Options |
|---|---|---|
| `FIX_MODE` | `auto` | `approve-first` stops after triage so you can review the fix plan first. |
| `FIX_SCOPE` | `S0-S3` | For example `S0-S1` to fix only critical and high issues this run. |
| `REFACTOR_MODE` | `propose` | `incremental` lets it restructure the code in approved small steps; `none` skips it. |
| `NEEDS APPROVAL` | file deletions, API changes, DB changes, auth model changes, … | Anything on this list waits for your yes in `QUESTIONS.md`. |

## Your projects

| Project | Repo | Profile in the prompt |
|---|---|---|
| Music scene API (mau5trap Intelligence Platform) | `bufirstrepo/mau5trap-repo` | **P1**, with 17 pre-scanned leads. |
| In the Red (video game) | `bufirstrepo/inthered` (had no commits on 2026-09-29) | **P2**. The agent detects the engine on first run. |
| Decentralflix | not found under this GitHub account | **P3**. The agent builds the profile on first run. |
| Hype Lab | not found under this GitHub account | **P4**. The agent builds the profile on first run. |

## Good to know

- **It's a long run.** A full review plus fixes can take hours of agent time. The agent saves progress to `devteam/` and git as it goes, so you can stop and resume anytime.
- **Public repos:** the agent never writes secret values into its notes. It also describes security problems as what/where/impact/fix, never as step-by-step exploit instructions.
- **Your answers steer it.** Check `QUESTIONS.md` after Phase 4. Each question has the agent's recommendation and the default it's using until you answer.
