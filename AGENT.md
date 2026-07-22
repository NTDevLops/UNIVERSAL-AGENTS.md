# AGENT.md

## Purpose

This document defines universal rules for AI agents working on any software project. Follow these instructions exactly unless the user explicitly overrides them.

---

# Core Principles

- Execute **only** the explicitly requested changes.
- Never infer additional requirements.
- Never add features that were not requested.
- Never redesign, refactor, optimize, or reorganize code unless explicitly instructed.
- If any request is ambiguous, incomplete, or conflicts with the existing project, **stop immediately and request clarification** before making changes.

---

# Project Analysis

Before making any modification:

1. Analyze the existing project structure.
2. Understand the current architecture.
3. Search the codebase for existing implementations.
4. Reuse existing code whenever possible.
5. Avoid duplicate functionality.

Do not implement anything until the analysis is complete.

---

# Planning Requirement

Before any implementation:

1. Create a concise implementation plan.
2. Explain what will change.
3. Present a **Before vs After** visualization.

Possible visualization formats:

- Flowchart
- Screen flow
- State diagram
- Sequence diagram
- Architecture diagram
- Algorithm flow
- Component interaction

Example:

```text
Before

User
  │
  ▼
Submit
  │
  ▼
No Validation


After

User
  │
  ▼
Submit
  │
  ▼
Input Validation
  │
  ▼
Processing
  │
  ▼
Success
```

No implementation should begin until this explanation has been presented.

---

# Existing Code First

Always search the project before writing code.

Reuse existing:

- modules
- components
- classes
- functions
- utilities
- services
- repositories
- managers
- adapters
- models
- helpers

Never duplicate business logic.

If equivalent functionality already exists, extend or reuse it instead of creating a new implementation.

---

# Minimal Changes Only

Modify only the code necessary to satisfy the request.

Preserve everything else, including:

- formatting
- whitespace
- indentation
- ordering
- comments
- naming
- capitalization
- imports (unless required)

Do not perform:

- refactoring
- cleanup
- formatting-only changes
- style changes
- renaming
- code movement
- unrelated fixes

unless explicitly requested.

---

# Documentation

Documentation must always reflect the current project state.

Whenever changes affect:

- features
- configuration
- dependencies
- APIs
- setup
- behavior
- architecture

update the relevant documentation accordingly.

If the project contains a README or equivalent documentation, keep it synchronized with the implementation.

Never leave documentation inconsistent with the code.

---

# Dependency Management

When modifying dependencies:

- Maintain compatibility with the existing project.
- Prefer official, stable releases.
- Verify compatibility using official documentation.
- Avoid unnecessary upgrades.
- Do not replace libraries unless requested.

---

# Best Practices

Follow official best practices for the project's technology stack.

Respect:

- existing architecture
- coding conventions
- dependency injection patterns
- project organization
- error handling
- logging strategy
- state management
- concurrency model

Do not introduce unnecessary abstractions.

---

# File System Rules

Do not create, modify, rename, move, or delete files unless required by the requested change.

Never introduce temporary artifacts such as:

- log files
- scratch files
- debug files
- generated outputs
- cache files
- notes
- backups

Remove any accidental temporary files before finishing.

---

# Repository Hygiene

Never commit or generate:

- build outputs
- generated code
- binaries
- archives
- logs
- IDE caches
- temporary files
- screenshots
- recordings

Respect the project's ignore rules.

---

# Bug Detection

While working on the requested change:

- inspect nearby code for obvious bugs directly related to the modified area
- fix only issues necessary to prevent regressions
- avoid unrelated fixes

Do not expand the scope.

---

# UI Changes

When modifying user interfaces:

- preserve the existing design language
- maintain accessibility
- preserve localization
- preserve RTL support where applicable
- preserve responsive behavior
- follow platform design guidelines

Do not redesign interfaces unless requested.

---

# Performance

Do not introduce:

- unnecessary allocations
- unnecessary rendering
- blocking operations
- excessive queries
- memory leaks
- performance regressions

Prefer efficient implementations consistent with the existing architecture.

---

# Security

Never:

- hardcode secrets
- expose credentials
- disable security mechanisms
- weaken authentication
- bypass authorization
- disable encryption
- ignore input validation

Always follow security best practices for the technology stack.

---

# Testing

If existing tests are affected:

- update only the impacted tests
- preserve existing coverage
- do not remove tests unless explicitly instructed
- do not create new tests unless requested

---

# Official Documentation

Base implementation decisions on official documentation whenever applicable.

Prefer official sources over community examples or assumptions.

---

# Communication

If clarification is required:

- ask only the minimum number of questions
- do not guess missing requirements
- wait for clarification before proceeding

---

# Output Rules

Unless the user requests otherwise:

- provide only the requested output
- avoid unrelated explanations
- avoid unnecessary commentary
- avoid speculative improvements

---

# Scope Protection

Do not change anything outside the requested scope.

Specifically avoid:

- architecture changes
- dependency upgrades
- code cleanup
- formatting
- naming changes
- project restructuring
- feature expansion

unless explicitly requested.

---

# Primary Objective

Preserve the existing project while implementing the smallest, safest, fully documented, maintainable, and officially supported change necessary to satisfy the user's explicit request.
