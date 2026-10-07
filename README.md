# Universal AI Agents Rules

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
![Version](https://img.shields.io/badge/AGENTS.md-v1.1.0-green)

A universal set of instructions for AI coding agents to ensure consistent, predictable, and minimal-impact changes across software projects.

## Purpose

This repository provides a technology-agnostic **AGENTS.md** that defines how an AI agent should analyze, plan, modify, and document software projects while preserving existing architecture and minimizing unnecessary changes.

## Goals

- Execute only explicitly requested changes.
- Preserve existing project structure and architecture.
- Minimize modifications to the smallest possible scope.
- Reuse existing implementations whenever possible.
- Keep project documentation synchronized with code changes.
- Follow official documentation and best practices.
- Maintain repository cleanliness and security.
- Protect version control, secrets, and the working tree.

## Contents

The `AGENTS.md` includes guidance for:

- Core operating principles
- Project analysis
- Implementation planning
- Existing code reuse
- Minimal-change policy
- Documentation maintenance
- Dependency management
- Best practices
- File system rules
- Repository hygiene
- Bug detection
- UI modification guidelines
- Performance considerations
- Security requirements
- Testing expectations
- Official documentation usage
- Communication guidelines
- Output rules
- Scope protection

### Original rules (Sections 1–26 and Final Rule)

Same rules, same numbering, same order. The wording was polished for clarity and consistency, and no rule was added to, removed from, or changed in purpose.

### Additional rules (Sections 27–36)

New sections that supplement the original rules without modifying them:

| § | Topic | Supplements |
|---|-------|-------------|
| 27 | Version Control Safety | §13 Repository Hygiene |
| 28 | Destructive and High-Impact Operations | §17 Security |
| 29 | Untrusted Content, Secrets, and Data Privacy | §17 Security |
| 30 | Dependency Vetting | §10 Dependency Management |
| 31 | Testing Integrity | §19 Testing |
| 32 | Source Verification | §21 Official Documentation |
| 33 | Working Tree Protection | §4 Project Analysis |
| 34 | Project-Specific Rules, Nested Files, and Non-Interactive Runs | §23 Conflict Resolution |
| 35 | Additional Completion Checks | §26 Completion Checklist |
| 36 | Report Template | §24 Output Rules |

Sections 27, 28, 29, and 33 are safety rules and can be overridden only by an explicit, specific user instruction.

## Intended Usage

1. Copy `AGENTS.md` into the root of your repository.
2. Allow AI coding assistants to read the file before making changes.
3. Update the file as your project's standards evolve.
4. Keep the instructions concise, explicit, and project-specific when necessary.

### Project-specific rules

Copy [`templates/AGENTS.project.template.md`](templates/AGENTS.project.template.md) to `AGENTS.project.md` and fill in your commands, conventions, and protected areas. Project-specific rules rank above this document (see Section 23).

### Tool adapters

If your tool does not read `AGENTS.md` natively, copy the matching pointer file from [`adapters/`](adapters/):

| Tool | Copy this file | To |
|------|----------------|----|
| Claude Code | `adapters/CLAUDE.md` | `CLAUDE.md` (repo root) |
| Gemini CLI | `adapters/GEMINI.md` | `GEMINI.md` (repo root) |
| GitHub Copilot | `adapters/copilot-instructions.md` | `.github/copilot-instructions.md` |
| Cursor | `adapters/cursor-universal-agents.mdc` | `.cursor/rules/universal-agents.mdc` |

Many tools read `AGENTS.md` directly. Check your tool's documentation to see whether an adapter is needed.

## Design Principles

The rules prioritize:

- Predictability
- Safety
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

## Philosophy

The agent should behave like a disciplined software engineer:

- Understand before changing.
- Reuse before creating.
- Modify only what is necessary.
- Never assume missing requirements.
- Document every meaningful project change.
- Leave the repository in a clean and consistent state.

## Repository Layout

```
.
├── AGENTS.md                       # The universal rules
├── README.md
├── LICENSE
├── adapters/                       # Pointer files for specific tools
├── templates/
│   └── AGENTS.project.template.md  # Project-specific rules
```

## License

Released under the [MIT License](LICENSE). Use, modify, and adapt these guidelines to fit your project's workflow and development standards.