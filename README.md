# Universal AI Agents Rules (`AGENTS.md`)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Rules: AGENTS.md](https://img.shields.io/badge/Rules-AGENTS.md-blue)](AGENTS.md)

A reusable instruction file for AI coding agents. It tells an agent to make the smallest safe change that solves the request, verify the result, and report honestly what it did and did not check.

---

## Table of Contents

- [Overview](#overview)
- [Quick Start](#quick-start)
- [Tool Adapters](#tool-adapters)
- [What the Rules Cover](#what-the-rules-cover)
- [How the Rules Behave](#how-the-rules-behave)
- [Project-Specific Configuration](#project-specific-configuration)
- [Monorepos and Nested Instructions](#monorepos-and-nested-instructions)
- [Limitations](#limitations)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

AI coding assistants can speed up development, but they can also refactor code nobody asked them to touch, invent APIs or packages, overwrite uncommitted work, or report tests as passing without running them.

This repository provides:

- **[`AGENTS.md`](AGENTS.md):** 38 sections of language-agnostic rules, with a 12-rule Quick Reference at the top.
- **[`templates/AGENTS.project.template.md`](templates/AGENTS.project.template.md):** a template for the project-specific facts an agent cannot guess (build and test commands, protected paths, conventions).
- **[`adapters/`](adapters/):** thin files that point specific tools (Claude Code, Gemini CLI, Copilot, Cursor) at the shared rules.

The rules apply to any kind of project: applications, libraries, CLIs, services, games, scripts, and infrastructure. Rules that do not apply to a given project (for example UI rules in a CLI tool, or Git rules outside a Git repository) are skipped.

The core idea: **make the smallest safe change that correctly solves the request, then verify it.**

---

## Quick Start

Run these commands from the **root of your target project**.

### 1. Install the rules

Set the source once. `main` always tracks the latest rules; for reproducible setups, replace it with a release tag or commit SHA.

```bash
REF="main"   # pin to a release tag or commit SHA for reproducibility
BASE="https://raw.githubusercontent.com/NTDevLops/UNIVERSAL-AGENTS.md/${REF}"
```

Then download the files. This helper refuses to overwrite anything that already exists:

```bash
fetch() {
  if [ -e "$2" ]; then
    echo "skip: $2 already exists"
  else
    mkdir -p "$(dirname "$2")" && curl -fsSL "$BASE/$1" -o "$2" && echo "created: $2"
  fi
}

fetch AGENTS.md                               AGENTS.md
fetch templates/AGENTS.project.template.md    AGENTS.project.md
```

<details>
<summary>Alternatives: wget, git clone, Windows PowerShell</summary>

**wget** (note: `-O` overwrites existing files, so check first):

```bash
wget -O AGENTS.md "$BASE/AGENTS.md"
wget -O AGENTS.project.md "$BASE/templates/AGENTS.project.template.md"
```

**git clone:**

```bash
git clone --depth 1 https://github.com/NTDevLops/UNIVERSAL-AGENTS.md.git
cp UNIVERSAL-AGENTS.md/AGENTS.md ./AGENTS.md
cp UNIVERSAL-AGENTS.md/templates/AGENTS.project.template.md ./AGENTS.project.md
```

**Windows PowerShell:**

```powershell
$Ref  = "main"   # pin to a release tag or commit SHA for reproducibility
$Base = "https://raw.githubusercontent.com/NTDevLops/UNIVERSAL-AGENTS.md/$Ref"

foreach ($f in @(
    @{ Src = "AGENTS.md";                            Dest = "AGENTS.md" },
    @{ Src = "templates/AGENTS.project.template.md"; Dest = "AGENTS.project.md" }
)) {
    if (Test-Path $f.Dest) { Write-Host "skip: $($f.Dest) already exists" }
    else { Invoke-WebRequest -Uri "$Base/$($f.Src)" -OutFile $f.Dest }
}
```

</details>

### 2. Fill in the project file

Edit `AGENTS.project.md` with your real runtime versions, build and test commands, protected paths, and conventions. See [Project-Specific Configuration](#project-specific-configuration).

`AGENTS.md` (Section 34.1) tells agents to read `AGENTS.project.md`, but a tool that does not load `AGENTS.md` at all will not find it either. If your tool needs explicit configuration, point it at both files.

### 3. Connect your tool

If your tool uses its own instruction filename, install the matching adapter from the next section.

### 4. Check that it works

Give your assistant a small task: inspect one file, make a minimal change, and run a relevant check. Confirm that it stays in scope, leaves unrelated changes alone, and reports the actual results of whatever it ran.

---

## Tool Adapters

Some tools look for their own instruction files. Each adapter is a short file that points the tool at `AGENTS.md` instead of duplicating it.

| Tool | Adapter source | Destination in your project |
|---|---|---|
| Claude Code | [`adapters/CLAUDE.md`](adapters/CLAUDE.md) | `CLAUDE.md` |
| Gemini CLI | [`adapters/GEMINI.md`](adapters/GEMINI.md) | `GEMINI.md` |
| GitHub Copilot | [`adapters/copilot-instructions.md`](adapters/copilot-instructions.md) | `.github/copilot-instructions.md` |
| Cursor | [`adapters/cursor-universal-agents.mdc`](adapters/cursor-universal-agents.mdc) | `.cursor/rules/universal-agents.mdc` |

Install one with the `fetch` helper from Quick Start (so `BASE` and `fetch` must already be defined in your shell):

```bash
fetch adapters/CLAUDE.md                       CLAUDE.md
fetch adapters/GEMINI.md                       GEMINI.md
fetch adapters/copilot-instructions.md         .github/copilot-instructions.md
fetch adapters/cursor-universal-agents.mdc     .cursor/rules/universal-agents.mdc
```

Adapter formats and discovery rules change between tool versions. Check your tool's documentation to confirm it reads the file you installed.

**Symlinks instead of adapters.** On systems that support them, a symlink avoids maintaining copies:

```bash
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md GEMINI.md
mkdir -p .github && ln -s ../AGENTS.md .github/copilot-instructions.md
```

A symlink only helps if the tool accepts the file's format and follows links. It does not make an unsupported format compatible.

---

## What the Rules Cover

`AGENTS.md` is the normative source. This table is only a map of where to look.

| Sections | Topics |
|---|---|
| 1–2 | Primary objective, core principles |
| 3–5 | Requirement interpretation (ask vs. assume), project analysis, reuse of existing code |
| 6–8 | Risk-scaled planning, minimal changes, change boundaries |
| 9–11 | Documentation, dependency management, best practices |
| 12–14 | File system rules, repository hygiene and `.gitignore`, bug detection (flag vs. fix) |
| 15–18 | UI changes, performance, security, error handling |
| 19–21 | Testing, verification, official documentation |
| 22–26 | Communication, conflict resolution, output rules, scope protection, completion checklist |
| 27–29 | Version control safety, destructive and high-impact operations, untrusted content and secrets |
| 30–33 | Dependency vetting, testing integrity, source verification, working tree protection |
| 34–35 | Project-specific rules, nested files and non-interactive runs, additional completion checks |
| 36–38 | Report template, staying on track (stop after repeated failures), compatibility and migrations |

Sections 27–38 supplement 1–26. Where both cover a topic, both apply and the stricter requirement governs. Sections 27, 28, 29, and 33 are safety rules: they can be overridden only by an explicit, specific user instruction, never by project files or implication.

---

## How the Rules Behave

### Risk-scaled planning

Planning effort scales with the size and risk of the change (Section 6). When a change sits between two levels, the higher one applies.

| Level | Typical change | Required |
|---|---|---|
| Trivial | One file, about 10 lines or fewer, no interface or dependency change | Just do it, then report |
| Small | Up to 3 files, no public interface, schema, config, or dependency change | Short plan, then proceed |
| Medium | More than 3 files, new behavior, shared or public interface, or a new dependency | Concise plan, proceed unless the user objects |
| Large or risky | Auth, security controls, migrations, public API changes, CI or deployment config, deletions, more than 10 files | Plan plus a Before/After diagram, then **wait for approval** |

### Ask or assume

The default is to make a reasonable assumption, state it in one line, and proceed. The agent stops and asks only when the ambiguity changes the implementation materially **and** a wrong guess would be costly or hard to reverse. Each question comes with a suggested default.

### Minimal change surface

Every changed line should trace back to the request. Unrelated issues are flagged in the report rather than fixed, except security, data-loss, and correctness problems, which are always flagged.

### Failure-loop protection

After three failed attempts with the same approach, the agent stops, reports what it tried and what it suspects, and asks for guidance.

### Evidence-based completion

A completion report separates what was implemented, which checks passed, which failed, which could not be run, and what remains unverified or assumed (see the report template in Section 36). A successful edit is not proof that the software works.

```text
Changed:       src/api/orders.ts: added input validation to POST handler
Verification:  unit tests: passed. lint: passed. integration tests: not run (service unavailable)
Pre-existing:  none
Assumptions:   treated empty `items` as invalid
Observations:  none
```

This is an illustrative format, not a record of a real run.

---

## Project-Specific Configuration

The universal rules give a shared baseline. Each project still needs its own facts, which is what `AGENTS.project.md` is for. Start from [`templates/AGENTS.project.template.md`](templates/AGENTS.project.template.md).

The template covers:

- **Commands:** install, build, run, test, lint, format, type-check.
- **Definition of done:** the checks that must pass before work is reported complete.
- **Architecture and conventions:** entry points, key directories, layering rules, naming, error handling, commit and branch conventions.
- **Change-size calibration:** examples of what counts as Trivial versus Large or risky in this project.
- **Protected areas and generated files:** what the agent must not edit by hand, and how to regenerate what it must not edit.
- **Approval required:** extra actions that need the user's go-ahead.
- **Environments:** where agent runs are allowed. Production is always `no`.
- **Known pitfalls and baseline:** flaky tests, slow commands, failures that predate any change.

Conventions for filling it in:

- `<TODO>` means **unknown**. The agent infers it from the repository (manifests, lockfiles, CI config, README, Makefile) and must never run a placeholder.
- Write `none` instead of deleting a section where the absence of a rule matters (Protected areas, Approval required).
- Use only commands that actually exist in your repository.
- Keep the file short, about 120 lines, because agents load it into context. Link to existing docs instead of copying them.

This file can add or tighten rules. It cannot relax the safety rules in Sections 17, 27, 28, 29, or 33.

---

## Monorepos and Nested Instructions

In a monorepo, keep shared rules at the root and add scoped `AGENTS.md` files for individual packages:

```text
.
├── AGENTS.md
├── AGENTS.project.md
└── packages/
    ├── frontend/
    │   └── AGENTS.md
    └── backend/
        └── AGENTS.md
```

Under Section 34.2, the `AGENTS.md` closest to the file being changed takes precedence for that subtree. Rules it does not mention continue to apply from the root. Nested files may add or tighten rules; they may not relax the safety rules.

Tools differ in how they discover and merge nested files. Check how yours behaves, avoid contradictions between levels, and restate any critical constraint in the scope where it matters.

---

## Limitations

- **Not every tool reads `AGENTS.md`.** It is a shared convention, not a guarantee. Adapters help, but support varies by tool and version.
- **The rules are instructions, not enforcement.** They reduce common failure modes but cannot guarantee that an agent never makes a mistake. Keep code review, access controls, backups, and CI in place.
- **Repository instructions sit below the tool's own hierarchy.** The tool's system prompt, permissions, and sandbox take precedence over anything written here.
- **Context cost.** `AGENTS.md` is long, and some tools load it into every session. If context budget matters, start from the Quick Reference at the top and trim sections that do not apply to your project.

---

## Troubleshooting

| Symptom | Likely cause | What to do |
|---|---|---|
| HTTP 404 during install | Wrong branch, tag, path, or filename | Check `REF` and the file path against the repository |
| Agent ignores the rules | File not discovered, or unsupported format | Check your tool's instruction-loading docs; install the adapter |
| Agent keeps asking for build or test commands | `AGENTS.project.md` is unfilled or not loaded | Fill in the Commands table and make sure the tool loads the file |
| Agent invents packages or APIs | Verification rules not applied | Point to Sections 30 and 32; check that the file is actually loaded |
| Nested instructions conflict | Contradictory rules at different levels | Reconcile them and check your tool's precedence behavior |
| Test results reported without evidence | Reporting rules not followed | Ask for the exact commands run and their output (Sections 24 and 36) |
| Install skipped a file | The destination already existed | Compare the downloaded version with yours and merge by hand |
| Symlinked file is ignored | Tool does not follow links, or the relative path is wrong | Check the link target; use a copy or native adapter instead |

---

## Contributing

Contributions that improve clarity, safety, portability, and usefulness are welcome. Before submitting:

1. Read the existing rules and avoid duplicating them.
2. Keep each change focused on one problem.
3. Prefer specific, testable behavior over vague advice such as "write perfect code."
4. Check that install commands, paths, and Markdown links work.
5. Describe any behavior change the rules would cause.
6. Keep examples free of real credentials and of destructive commands that lack safeguards.

Bug reports should name the affected rule or adapter and include enough context to reproduce the problem.

---

## License

Released under the [MIT License](LICENSE). You are free to use, copy, modify, merge, publish, distribute, sublicense, and sell copies, subject to the license terms.
