# AGENTS.md

Guidance for AI coding agents working in this repository.

## Project Overview

This repo holds the **data** behind Zitec's Technology Radars (based on the [Thoughtworks Technology Radar](https://www.thoughtworks.com/radar)). There is no application code: the work here is editing data files and keeping them valid.

Changes pushed to the private GitLab repo are synced automatically to the public [GitHub repo](https://github.com/zitec/Zitec-Tech-Radar), which the radars read from. A sync can take up to 5 minutes. Anything you commit becomes public.

## Repository Layout

| Path | Purpose |
| --- | --- |
| `TechRadar.csv` | Dev radar ([zit.ec/techradar](https://zit.ec/techradar)) |
| `TechRadar_QA.csv` | QA radar ([zit.ec/qatechradar](https://zit.ec/qatechradar)) |
| `tech-radar.json` | Unified Dev + QA radar for the Backstage platform |
| `schemas/tech-radar-schema.json` | JSON Schema that `tech-radar.json` must satisfy |
| `.gitlab-ci.yml` | CI job that validates the JSON on merge requests |

## Data Formats

### CSV radars (`TechRadar.csv`, `TechRadar_QA.csv`)

These follow the [build-your-own-radar CSV format](https://github.com/thoughtworks/build-your-own-radar?tab=readme-ov-file#using-csv-data).

```
name,ring,quadrant,isNew,description
```

- `ring`: `adopt`, `trial`, `assess`, or `caution` (lowercase). `caution` replaced `hold`, following the Thoughtworks rename.
- `quadrant`: `languages & frameworks`, `platforms`, `techniques`, or `tools` (lowercase).
- `isNew`: `TRUE` or `FALSE` (uppercase).
- `description`: optional. Wrap it in double quotes if it contains commas.

### JSON radar (`tech-radar.json`)

- `quadrant` is a string id: `"1"` Languages & frameworks, `"2"` Platforms, `"3"` Techniques, `"4"` Tools.
- `key` is usually the lowercase, hyphenated form of `title` (for example `infrastructure-as-code`). The schema doesn't require unique keys, and some keys already appear twice because the file merges the Dev and QA radars.
- `timeline[].ringId` is one of `adopt`, `trial`, `assess`, or `caution`. `date` uses the `YYYY-MM-DD` format. `moved` is optional (`-1`, `0`, or `1`).
- The schema sets `additionalProperties: false`, so unknown fields fail validation.

### Keeping the files in sync

The CSV files and the JSON file are maintained separately and can drift apart. When you change an entry in one of them, ask whether the same change should also go into the others. Don't assume.

## Validation

CI runs this on every merge request:

```sh
ajv validate -s schemas/tech-radar-schema.json -d tech-radar.json
```

To run it locally (requires Node.js):

```sh
npx ajv-cli validate -s schemas/tech-radar-schema.json -d tech-radar.json
```

Nothing validates the CSV files automatically. After editing one, make sure it still parses, has the same 5 columns, and only uses the allowed values listed above.

## Conventions

- Keep edits minimal and preserve the existing order of entries and the existing formatting (2-space indentation in the JSON).
- Use commit messages that describe what changed on the radar, for example `Moved TypeScript to adopt` or `Added LangChain`.

---

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.
