---
title: "feat: Set up personal skills repository with auto-generated .skill files"
type: feat
status: completed
date: 2026-04-10
origin: docs/brainstorms/2026-04-10-skills-repo-requirements.md
---

# Set Up Personal Skills Repository

## Overview

Create a public GitHub repo at `github.com/kaelig/skills` hosting custom Claude Code skills. Skills are installable via `npx skills add kaelig/skills` and downloadable as `.skill` files. A GitHub Action auto-generates `.skill` ZIP archives whenever skill source files change on `main`.

## Problem Frame

Kaelig needs a single place to maintain and distribute personal Claude Code skills. The repo should work with both the `skills` CLI (for developers) and `.skill` file downloads (for Claude Desktop drag-and-drop). The first skill is a company-evaluator for employer research. (see origin: docs/brainstorms/2026-04-10-skills-repo-requirements.md)

## Requirements Trace

- R1. Flat root structure: `skill-name/SKILL.md` directories at repo root, `.skill` ZIPs alongside
- R2. Flat naming (no namespace prefix)
- R3. Compatible with `npx skills add kaelig/skills`
- R4. GitHub Action generates `.skill` files on push to `main`, commits them back
- R5. Action only commits when changes exist, cleans up orphaned `.skill` files
- R6. First skill: `company-evaluator`
- R7. README with install instructions and skill listing

## Scope Boundaries

- No plugin manifest, MCP servers, or agents
- No npm publishing
- No versioning/releases initially
- No custom "create skill" skill

## Context & Research

### Relevant Patterns

- **montagao/skills** (reference repo): Skill directories at root, `.skill` files at root, minimal README with `npx skills add montagao/skills`. No GitHub Action for auto-generation.
- **compound-engineering `package_skill.py`**: Uses `zipfile.ZIP_DEFLATED`, archives with `relative_to(skill_path.parent)` to preserve directory structure inside the ZIP.
- **Verified `.skill` format**: ZIP archive containing `{skill-name}/SKILL.md` (and optional reference files). The directory name inside the ZIP must match the skill name.

### SKILL.md Frontmatter Convention

```yaml
---
name: skill-name
description: When to trigger this skill and what it does
---
```

Optional fields: `allowed-tools`, `argument-hint`, `metadata.author`, `metadata.version`.

## Key Technical Decisions

- **Shell-based packaging in GitHub Action** (resolves deferred Q1): `zip -r` is sufficient. No Python/Node runtime needed. Validation is just checking `SKILL.md` exists, which the directory scan already ensures.
- **`github-actions[bot]` identity** (resolves deferred Q2): Standard bot identity (`41898282+github-actions[bot]@users.noreply.github.com`). Commits are clearly distinguishable from human commits.
- **Regenerate all, commit only if changed** (resolves deferred Q3): Delete all `.skill` files, regenerate from current directories, commit only if `git status` shows changes. Simpler than diff-parsing. With <20 skills, cost is negligible.
- **`[skip ci]` in auto-commit message**: Prevents infinite Action loops when the bot pushes generated `.skill` files.

## Open Questions

### Resolved During Planning

- **Should the Action trigger on all pushes or only when skill files change?** Trigger on all pushes to `main`. The Action is fast (just zipping small files) and the "regenerate all, commit if changed" approach naturally handles no-ops.
- **Does `npx skills add` need a specific directory structure?** No — it recursively scans for `SKILL.md` files. Root-level directories work fine.

### Deferred to Implementation

- None — all questions resolved.

## Implementation Units

- [x] **Unit 1: Repository scaffold and first skill**

  **Goal:** Create the repo structure with `company-evaluator` skill, `.gitignore`, and README.

  **Requirements:** R1, R2, R3, R6, R7

  **Dependencies:** None

  **Files:**
  - Create: `company-evaluator/SKILL.md`
  - Create: `README.md`
  - Create: `.gitignore`

  **Approach:**
  - `company-evaluator/SKILL.md` contains the full skill content already written in the brainstorm. Frontmatter: `name: company-evaluator`, `description: ...`
  - `README.md` follows montagao/skills pattern: repo title, install command, skill table with name + one-line description, note about `.skill` files
  - `.gitignore`: standard ignores (`.DS_Store`, `node_modules/`, etc.). Do NOT ignore `.skill` files — they're committed artifacts.

  **Patterns to follow:**
  - montagao/skills README structure
  - SKILL.md frontmatter convention from existing installed skills (`find-skills`, `un-ai-writing`)

  **Verification:**
  - `company-evaluator/SKILL.md` exists with valid YAML frontmatter
  - README shows install command and skill listing

- [x] **Unit 2: GitHub Action for .skill file generation**

  **Goal:** Auto-generate `.skill` ZIP files on push to `main` and commit them back.

  **Requirements:** R4, R5

  **Dependencies:** Unit 1

  **Files:**
  - Create: `.github/workflows/generate-skill-files.yml`

  **Approach:**
  - Trigger: `push` to `main`, but skip if commit message contains `[skip ci]`
  - Steps:
    1. Checkout repo with write permissions
    2. Remove all existing `*.skill` files at root
    3. Find all directories containing `SKILL.md` at root level (exclude `.github`, `docs`, `node_modules`)
    4. For each, run `cd .. && zip -r {skill-name}.skill {skill-name}/` (or equivalent) to create archive with directory structure preserved
    5. Check `git status` — if no changes, exit cleanly
    6. Configure git as `github-actions[bot]`
    7. `git add *.skill` and commit with message containing `[skip ci]`
    8. Push to `main`
  - The Action needs `contents: write` permission on the default `GITHUB_TOKEN`

  **Patterns to follow:**
  - The ZIP must contain `{skill-name}/SKILL.md` (not just `SKILL.md` at archive root) — matching the format verified from existing `.skill` files
  - `[skip ci]` in commit message to break the Action loop

  **Test scenarios:**
  - Push a new skill directory -> `.skill` file appears in next commit
  - Modify an existing `SKILL.md` -> corresponding `.skill` file is updated
  - Delete a skill directory -> orphaned `.skill` file is removed
  - Push a non-skill change (e.g., README edit) -> no `.skill` commit generated

  **Verification:**
  - After pushing Unit 1, the Action runs and commits `company-evaluator.skill`
  - `unzip -l company-evaluator.skill` shows `company-evaluator/SKILL.md`
  - No infinite Action loop occurs

## Risks & Dependencies

- **GitHub Actions `GITHUB_TOKEN` permissions**: The default token needs `contents: write` to push commits. This is configurable in the workflow file with `permissions:` block. If the repo has branch protection rules requiring PR reviews, the bot push will be blocked — but for a personal repo without branch protection this isn't a concern.

## Sources & References

- **Origin document:** [docs/brainstorms/2026-04-10-skills-repo-requirements.md](docs/brainstorms/2026-04-10-skills-repo-requirements.md)
- Reference repo: github.com/montagao/skills
- Verified `.skill` format: `/Users/kaelig/Downloads/company-evaluator.skill`
- Packaging reference: compound-engineering `package_skill.py`
