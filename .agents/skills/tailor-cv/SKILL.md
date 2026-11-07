---
name: tailor-cv
description: Tailors Lasse's LaTeX CV (CV_Lasse_D_Skaalum.tex / CV_English_Lasse_D_Skaalum.tex) to a pasted job description (JD) — reorders and rewords skills and bullets, mirrors JD keywords, adjusts skill weights, languages, title and profile, on a per-application git branch. Use when the user pastes a job posting / JD or asks to target, tailor, or adapt the CV to a job.
---

# Tailor CV to a Job Description

Produce a targeted version of the CV for one job application. Every choice is confirmed with the user through the `grilling` skill before any file is edited.

## Ground rules

- **Never invent.** Rephrase existing facts with JD vocabulary and reorder freely, but never fabricate projects, employers, numbers, titles, results, or skills. New facts come only from the user's answers (recorded in the inventory).
- **Keep length and layout.** Page count of the built PDF must not grow; nothing may overflow. Keep the skills list roughly the same length.
- **Respect the LaTeX style.** Tabs for indentation, existing `\cv*` macros, escape special chars (`\#`, `\%`, `\&`). Hide content by commenting it out with `%`, never by deleting it.
- **Language of the CV text.** Danish file gets Danish text, English file gets English text.

## Workflow

### 1. Intake

1. The user pastes the JD as text. Ask for it if missing.
2. Detect JD language. Danish JD → `CV_Lasse_D_Skaalum.tex`; otherwise → `CV_English_Lasse_D_Skaalum.tex`. Confirm in the first grilling round.
3. Ensure the repo is on `main` with a clean working tree (`git status`). If not, stop and ask.
4. Read the chosen `.tex` file and `references/skills-inventory.md`.

### 2. Analyse the JD

- Extract company, role title, and location.
- Classify JD terms (technologies, practices, domains, soft skills) as **must-have** or **nice-to-have**. Note the JD's exact spelling of each.
- Map each term to: present in CV / present in inventory (incl. commented-out skills) / unknown.

### 3. Grill (load the `grilling` skill)

Run grilling rounds, one area per round, each question numbered with a recommended answer. Ask only what the current round's frontier allows; later rounds depend on earlier answers.

1. **Basics + missing skills** — confirm CV language, company/role slug for the branch name, and the must-have/nice-to-have split. For every JD term marked *unknown*, ask whether the user has experience with it (level, where, when). No experience → omit it from the CV entirely. Experience → plan where to weave it in (skills section, a job's technologies line, or a bullet).
2. **Skills section** — proposed order (most JD-relevant first) and weights (see *Skill weights*).
3. **Job entries** — per `\cvevent`: order of task bullets, result bullets, and technologies; rewordings using JD vocabulary; bullets to hide; the "also delivered" summary bullet (see *Bullets*).
4. **Languages** — propose additions only on a clear signal (see *Languages*).
5. **Header, profile, hobbies** — header subtitle, profile text diff, hobby order.

### 4. Persist new facts on `main`

If grilling produced new facts (skills the user has or lacks, levels, contexts), update `references/skills-inventory.md` on `main` and commit **only that file**:

```
git add .agents/skills/tailor-cv/references/skills-inventory.md
git commit -m "docs(tailor-cv): update skills inventory"
```

Do this before branching so every future branch inherits the knowledge.

### 5. Branch and edit

1. `git switch -c tailor/<company>-<role>` (lowercase kebab-case, e.g. `tailor/novo-nordisk-senior-backend-engineer`).
2. Save the pasted JD verbatim as `jd.md` at the repo root.
3. Apply exactly the changes confirmed in grilling to the chosen `.tex` file.
4. Do **not** commit, push, or merge. Leave everything uncommitted on the branch.

### 6. Verify

1. Build: `latexmk -pdf <file>.tex`.
2. Check page count is unchanged versus `main` (e.g. `pdfinfo <file>.pdf | grep Pages`; compare to the count before editing).
3. Scan the log for overfull boxes introduced by the edits; fix by trimming wording.
4. Check every must-have the user actually has appears verbatim (JD spelling) at least once.

### 7. Report

Short summary only: branch name, file edited, and a few lines on what changed.

## Tailoring rules

### Keywords

- Every must-have the user actually has must appear **verbatim** (JD spelling) at least once — in the skills section, a technologies line, a bullet, or the profile.
- Prefer the JD's terms over synonyms when rewording (e.g. JD says "event-driven architecture" → use that over "Kafka-based messaging" where truthful).
- No keyword stuffing: one natural occurrence per term is enough; never list skills the user lacks.

### Skill weights (`\cvskill{name}{}{weight}`)

Weights are not static. The user can read up before an interview, so a JD-critical skill may be boosted.

- Boost JD-relevant skills by **at most +0.2** over the inventory/CV value, capped at `1`.
- Lower or comment out skills irrelevant to the JD to make room.
- Restore commented-out skills (and inventory skills) when the JD makes them relevant, using their inventory weight as the baseline.
- Order: most JD-relevant first; among equally relevant, higher weight first.

### Bullets and technologies (per `\cvevent`)

- Reorder task bullets, result bullets, and the comma-separated technologies line so the most JD-relevant items come first.
- Reword bullets with JD vocabulary, keeping the same facts.
- Comment out bullets that are noise for this JD.
- When **2 or more** bullets in a list are hidden, add one final compact bullet in that same list summarising them, to signal breadth and productivity without taking attention, e.g. `\item Also delivered: X for DSB, Y for Banedanmark, and Z.` (Danish: `Leverede desuden: ...`). Keep it to one line where possible.
- Add a technology to a job's technologies line only if the user confirmed using it in that job.

### Languages

The user has these additional languages (see inventory for exact labels). Add them to the Languages section **only on a clear signal**: the JD mentions the language, the company/team/customers are in that country or region, or the JD stresses international/multilingual work.

| English label | Danish label |
| --- | --- |
| Norwegian (Conversational) | Norsk (Samtaleniveau) |
| Swedish (Conversational) | Svensk (Samtaleniveau) |
| German (Basic) | Tysk (Grundlæggende) |
| Chinese (Basic) | Kinesisk (Grundlæggende) |
| Spanish (Basic) | Spansk (Grundlæggende) |
| Arabic (Basic) | Arabisk (Grundlæggende) |

List them after Danish and English, most relevant first.

### Header subtitle

The header subtitle (currently `Senior Software Engineer`) may be adapted to the JD's title (e.g. `Senior Backend Engineer`) only if it truthfully describes the user's experience.

### Profile text

- Subtle but noticeable: weave in **2–4** JD terms/themes.
- Keep structure, voice, and paragraphs; word count within **±10%** of the original.
- Never change factual claims (e.g. years of experience).

### Other sections

- Hobbies: may be reordered when it helps culture fit (e.g. Home automation first for IoT roles). Do not add or remove hobbies.
- Education, certification, contact: do not touch.
