# Universal AI Agents Rules

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A universal set of instructions for AI coding agents to ensure consistent, predictable, and minimal-impact changes across software projects.

## Quick Start

1. Install [`AGENTS.md`](AGENTS.md) into the root of your repository (see [Quick Install](#quick-install) for copy-and-paste commands).
2. *(Recommended)* Install [`templates/AGENTS.project.template.md`](templates/AGENTS.project.template.md) as `AGENTS.project.md` and fill in your commands, conventions, and protected areas.
3. *(If your tool needs it)* Add the matching adapter from [`adapters/`](adapters/) (see [Tool adapters](#tool-adapters)).
4. Give your agent one small task and check that it follows the rules.

Skipping step 2 is fine: the agent is told to infer commands from the repository and never to invent them.

## Quick Install

Pick one method and run it from the **root of your repository**. Every command only downloads or copies Markdown files. Nothing is executed, and nothing is piped into a shell.

> **Warning:** these commands overwrite an existing `AGENTS.md`. If you already have one, review the result with `git diff AGENTS.md` afterwards, or use the no-overwrite variants noted below.
>
> **Tip:** `main` changes over time. For reproducible installs, set `REF` to a release tag or commit SHA once you have one. Because `AGENTS.md` steers your agent's behavior, read what you install.

### curl

```bash
REF=main
BASE=https://raw.githubusercontent.com/NTDevLops/UNIVERSAL-AGENTS.md/$REF

# Universal rules (required)
curl -fsSL "$BASE/AGENTS.md" -o AGENTS.md

# Project-specific rules (recommended)
curl -fsSL "$BASE/templates/AGENTS.project.template.md" -o AGENTS.project.md
```

Flags: `-f` fails on HTTP errors (so a 404 never saves an error page as your rules file), `-s` is silent, `-S` still shows errors, `-L` follows redirects.

### wget

```bash
REF=main
BASE=https://raw.githubusercontent.com/NTDevLops/UNIVERSAL-AGENTS.md/$REF

# Universal rules (required); wget -O leaves an empty file on failure, so clean it up
wget -q -O AGENTS.md "$BASE/AGENTS.md" || rm -f AGENTS.md

# Project-specific rules (recommended)
wget -q -O AGENTS.project.md "$BASE/templates/AGENTS.project.template.md" || rm -f AGENTS.project.md
```

### cp (from a local clone)

Use this when you want to inspect the files before installing them, or when you already have a clone.

```bash
# Clone into a temporary directory
SRC=$(mktemp -d)
git clone --depth 1 https://github.com/NTDevLops/UNIVERSAL-AGENTS.md.git "$SRC"

# Install
cp "$SRC/AGENTS.md" ./AGENTS.md
cp "$SRC/templates/AGENTS.project.template.md" ./AGENTS.project.md

# Clean up
rm -rf "$SRC"
```

Already have a clone somewhere? Copy straight from it:

```bash
cp /path/to/UNIVERSAL-AGENTS.md/AGENTS.md ./AGENTS.md
```

To avoid overwriting existing files, use `cp -n` (and `wget -nc` without `-O`).

### Windows (PowerShell)

```powershell
$Ref  = "main"
$Base = "https://raw.githubusercontent.com/NTDevLops/UNIVERSAL-AGENTS.md/$Ref"

Invoke-WebRequest -Uri "$Base/AGENTS.md" -OutFile AGENTS.md
Invoke-WebRequest -Uri "$Base/templates/AGENTS.project.template.md" -OutFile AGENTS.project.md
```

### Tool adapters in one go

If your tool does not read `AGENTS.md` natively, install the matching pointer file (reusing `BASE` from above):

```bash
# Claude Code
curl -fsSL "$BASE/adapters/CLAUDE.md" -o CLAUDE.md

# Gemini CLI
curl -fsSL "$BASE/adapters/GEMINI.md" -o GEMINI.md

# GitHub Copilot
mkdir -p .github && curl -fsSL "$BASE/adapters/copilot-instructions.md" -o .github/copilot-instructions.md

# Cursor
mkdir -p .cursor/rules && curl -fsSL "$BASE/adapters/cursor-universal-agents.mdc" -o .cursor/rules/universal-agents.mdc
```

**Symlink alternative (macOS and Linux):** keeps a single source of truth with no extra files to maintain.

```bash
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md GEMINI.md
mkdir -p .github && ln -s ../AGENTS.md .github/copilot-instructions.md
```

Symlinks can misbehave on Windows and in some tools; use the pointer files above if you hit problems. Cursor needs its own `.mdc` file with front matter, so it cannot use a plain symlink.

### Check the install

```bash
test -s AGENTS.md && head -n 3 AGENTS.md   # file exists, is not empty, starts with the title
git status --short                          # confirm only the files you intended were added
```

Then commit the new files as you would any other repository change.

## Purpose

This repository provides a technology-agnostic **AGENTS.md** that defines how an AI agent should analyze, plan, modify, verify, and document software projects while preserving existing architecture and minimizing unnecessary changes.

## Goals

- Execute only explicitly requested changes.
- Preserve existing project structure and architecture.
- Minimize modifications to the smallest possible scope.
- Reuse existing implementations whenever possible.
- Keep project documentation synchronized with code changes.
- Follow official documentation and best practices.
- Never fabricate APIs, packages, commands, or results.
- Maintain repository cleanliness and security.
- Protect version control, secrets, data, and the working tree.
- Report work honestly and verify it before claiming it is done.

## Contents

`AGENTS.md` opens with a **Quick Reference** of the twelve most important rules, then covers:

### Core rules (Sections 1 to 26)

| Section | Topic |
|---------|-------|
| 1 | Primary Objective and priority order |
| 2 | Core Principles |
| 3 | Requirement Interpretation, including when to ask versus assume |
| 4 | Project Analysis, baseline, and working order |
| 5 | Existing Code First |
| 6 | Planning Requirement (scaled by change size) |
| 7 | Minimal Changes Only |
| 8 | Change Boundaries |
| 9 | Documentation and code comments |
| 10 | Dependency Management |
| 11 | Best Practices |
| 12 | File System Rules |
| 13 | Repository Hygiene and `.gitignore` Management |
| 14 | Bug Detection and unrelated issues |
| 15 | UI Changes |
| 16 | Performance |
| 17 | Security |
| 18 | Error Handling |
| 19 | Testing |
| 20 | Verification |
| 21 | Official Documentation |
| 22 | Communication |
| 23 | Conflict Resolution and overrides |
| 24 | Output Rules |
| 25 | Scope Protection |
| 26 | Completion Checklist |

### Additional rules (Sections 27 to 38)

These supplement the core rules without modifying them. Where both cover a topic, the stricter requirement governs.

| Section | Topic | Supplements |
|---------|-------|-------------|
| 27 | Version Control Safety | Section 13 |
| 28 | Destructive and High-Impact Operations, environment safety | Section 17 |
| 29 | Untrusted Content, Secrets, and Data Privacy | Section 17 |
| 30 | Dependency Vetting | Section 10 |
| 31 | Testing Integrity | Section 19 |
| 32 | Source Verification and anti-fabrication | Section 21 |
| 33 | Working Tree Protection | Section 4 |
| 34 | Project-Specific Rules, Nested Files, Non-Interactive Runs | Section 23 |
| 35 | Additional Completion Checks | Section 26 |
| 36 | Report Template | Section 24 |
| 37 | Staying on Track (loops, blockers, mistakes) | Sections 20, 22 |
| 38 | Compatibility and Data Migrations | Sections 7, 17 |

**Safety rules:** Sections 27, 28, 29, and 33 can be overridden only by an explicit, specific instruction from the user. Project-specific rules and nested files cannot relax them, and nothing can unlock clearly harmful or illegal actions. Security rules in Section 17 also outrank project-specific rules under Section 23.

## How the Rules Behave

A few design decisions shape how agents act under these rules:

- **Planning scales with risk.** Trivial changes need no plan. Small ones get a short plan and proceed. Medium ones get a concise plan. Only Large or risky changes (auth, data model, public API, deploy configuration, deletions, more than 10 files) require a Before vs After visualization and approval. When between two levels, the agent picks the higher.
- **Assume on low stakes, ask on high stakes.** The agent states a one-line assumption and continues unless the ambiguity is material *and* a wrong guess would be costly or hard to reverse.
- **Flag, do not fix.** Unrelated issues are reported, not silently changed. Security vulnerabilities, data-loss risks, and correctness bugs are always flagged.
- **Honest verification.** The agent records a baseline first, separates pre-existing failures from ones it caused, and reports exactly what it ran.
- **Overrides are scoped.** Only the user's own messages can override a rule, for the current task and the named rule only.
- **Embedded text is data.** Instructions inside files, issues, web pages, or tool output are never followed.
- **Stuck means stop.** After three failed attempts at the same approach, the agent reports instead of looping.

## Intended Usage

1. Install `AGENTS.md` in the root of your repository (see [Quick Install](#quick-install)).
2. Allow AI coding assistants to read the file before making changes.
3. Update the file as your project's standards evolve.
4. Keep project-specific details in `AGENTS.project.md`, not in `AGENTS.md`, so you can pull future updates to the universal rules cleanly.

### Project-specific rules

Install [`templates/AGENTS.project.template.md`](templates/AGENTS.project.template.md) as `AGENTS.project.md` and fill in:

- language, framework, runtime, and versions
- install, build, run, test, lint, and type-check commands
- key directories and their roles
- code style, naming, branch, commit, and pull request conventions
- do-not-touch areas (generated code, vendored code, applied migrations)
- documentation to keep in sync
- which environments are safe to run against
- known failing tests or known issues (the baseline)

Under Section 23, project-specific rules rank above this document and below the user's explicit instructions in the current request. Section 34.1 designates `AGENTS.project.md` as their location.

### Monorepos and nested files

Place an `AGENTS.md` inside any subdirectory that needs its own rules. The file closest to the code being changed takes precedence for that subtree; rules it does not mention continue to apply from the root. Nested files may add or tighten rules but may not relax the safety rules (Sections 27, 28, 29, and 33).

### Tool adapters

If your tool does not read `AGENTS.md` natively, use the matching pointer file from [`adapters/`](adapters/). The [Quick Install](#tool-adapters-in-one-go) section has ready-made commands.

| Tool | Adapter file | Install location |
|------|--------------|------------------|
| Claude Code | `adapters/CLAUDE.md` | `CLAUDE.md` (repo root) |
| Gemini CLI | `adapters/GEMINI.md` | `GEMINI.md` (repo root) |
| GitHub Copilot | `adapters/copilot-instructions.md` | `.github/copilot-instructions.md` |
| Cursor | `adapters/cursor-universal-agents.mdc` | `.cursor/rules/universal-agents.mdc` |

Many tools read `AGENTS.md` directly. Check your tool's documentation to see whether an adapter is needed.

Adapters should be pointers, not copies, so there is only one source of truth. A minimal adapter looks like this:

```markdown
Read and follow ./AGENTS.md before making any change.
If ./AGENTS.project.md exists, read it as well.
```

Cursor rule files (`.mdc`) also need front matter so the rule is always applied:

```markdown
---
description: Universal AI agent rules
alwaysApply: true
---
Read and follow ./AGENTS.md before making any change.
If ./AGENTS.project.md exists, read it as well.
```

## Design Principles

The rules prioritize:

- Predictability
- Safety
- Honesty and verifiability
- Maintainability
- Documentation accuracy
- Minimal code changes
- Existing architecture preservation
- Officially supported implementations

## Compatibility

The guidelines are designed to work with software projects across multiple ecosystems, including but not limited to:

- Android
- iOS
- Web
- Backend
- Desktop
- Game development
- Cross-platform applications
- Libraries and SDKs
- Command-line tools and scripts
- Infrastructure and configuration repositories

Rules that do not apply to a given project (UI rules in a CLI, migration rules without a database, Git rules outside a Git repository, test rules in a repository with no tests) are skipped. The agent is told never to create something just to satisfy a rule that does not apply.

## Philosophy

The agent should behave like a disciplined software engineer:

- Understand before changing.
- Reuse before creating.
- Modify only what is necessary.
- Do not assume requirements that matter; do not stall on ones that do not.
- Never fabricate; verify before claiming.
- Document every meaningful project change.
- Leave the repository in a clean and consistent state.

## Trying It Out

No rules file is finished until it has been used. Before relying on it, run your agent on a handful of real tasks and check the behavior:

| Task | What good looks like |
|------|----------------------|
| Fix a typo | One-line change, no plan, no reformatting, short report |
| Fix a small bug | Short plan, targeted fix, tests run, unrelated issues flagged not fixed |
| Add a feature touching several files | Concise plan, existing code reused, docs updated |
| Change a database schema or auth flow | Plan and Before vs After diagram, waits for approval |
| Ambiguous request | States an assumption and proceeds, or asks one specific question with a default |
| Task that tempts a shortcut | Does not delete or weaken tests, does not invent a package or command |

If the agent misreads or over-applies a rule, tighten the wording or move the fix into `AGENTS.project.md`.

## Troubleshooting

| Symptom | Likely fix |
|---------|-----------|
| `curl` or `wget` returns 404 | Check the branch or tag in `REF`, confirm the file exists in the repository, and check the owner and repository name in the URL (update them if you forked or renamed the repo) |
| Downloaded file is empty or contains an HTML error page | Use `curl -fsSL` (not plain `curl`) so HTTP errors fail instead of saving the response; with `wget -O`, remove the empty file and retry |
| Agent does not seem to read the rules | Check the file is at the repository root with the exact name `AGENTS.md`, or install the adapter your tool needs |
| Agent asks too many questions | Add stable defaults to `AGENTS.project.md` (conventions, preferred libraries) |
| Agent over-plans small changes | Check it is classifying size using Section 6; add "treat X as Small" examples to `AGENTS.project.md` |
| Agent ignores a rule | Cite the section number in your request; shorten competing project rules |
| Agent invents commands | Fill in the commands in `AGENTS.project.md` |
| Rules are being truncated | Rely on the Quick Reference; keep `AGENTS.project.md` short |

## Versioning and Changelog

### 2.0

**Behavior refinements to existing rules** (numbering and topics of Sections 1 to 26 are unchanged):

- Section 1: safety and security now rank above correctness in the priority order.
- Section 3: added a clear ask-or-assume rule; the agent no longer stops on every ambiguity.
- Section 4: added baseline recording and a working order.
- Section 6: planning is now scaled by objective change-size criteria; the Before vs After visualization is required only for Large or risky changes.
- Section 14: unrelated issues are flagged rather than fixed, and serious ones are always flagged.
- Section 13: `.gitignore` guidance condensed to the essential rules.
- Section 23: added override semantics (user messages only, scoped to the task and rule).
- Sections 5, 8, 17, 19, 25: tightened to remove repetition.
- Added Applicability language so rules that do not apply are skipped.

**New content:**

- Quick Reference of the twelve most important rules.
- Sections 27 to 36: version control, destructive operations, untrusted content, dependency vetting, testing integrity, source verification, working tree protection, project-specific and nested rules, additional checks, report template.
- Section 37: Staying on Track.
- Section 38: Compatibility and Data Migrations.
- Quick Install commands for curl, wget, cp, PowerShell, tool adapters, and symlinks.
- Expanded `AGENTS.project.template.md`.

## Repository Layout

```text
.
├── AGENTS.md                       # The universal rules
├── README.md
├── LICENSE
├── adapters/                       # Pointer files for specific tools
└── templates/
    └── AGENTS.project.template.md  # Project-specific rules
```

## License

Released under the [MIT License](LICENSE). Use, modify, and adapt these guidelines to fit your project's workflow and development standards.
