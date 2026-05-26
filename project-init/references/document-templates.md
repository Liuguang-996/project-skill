# Project Init Templates

Use these templates as starting points. They are organized by project type — pick the matching template set based on the type detected in SKILL.md step 2. Trim sections that would be empty noise. The four universal files (README, TASKS, CHANGELOG, DECISIONS) are created for every project; the rest are type-specific.

---

# Universal Templates (all types)

## `User/README.md` — by project type

### 代码工程

```md
# {project_name}

## Goal

{one-sentence goal}

## Scope

**Do now**
- {mvp item 1}
- {mvp item 2}

**Do not do yet**
- {non-goal 1}
- {non-goal 2}

## Entry Points

```bash
{install_command}
{run_command}
```

## Stack

| Area | Choice |
|------|--------|
| Runtime | {value} |
| Framework | {value} |
| Data | {value} |
| Deploy | {value} |

## Status

- Phase: `{idea|setup|mvp|iteration|maintenance}`
- Last updated: `{date}`
```

### 写作工程

```md
# {project_name}

## Goal

{one-sentence goal}

## Type

{academic paper | competition entry | course design | business plan | blog post}

## Audience

{who will read this}

## Deadline

{date or N/A}

## Deliverable

{format, word count, submission method}

## Status

- Phase: `{outline|drafting|revising|final|submitted}`
- Last updated: `{date}`
```

### 设计工程

```md
# {project_name}

## Goal

{one-sentence description of what this design communicates or solves}

## Type

{PPT | workflow diagram | schematic | PCB | other}

## Tool

{PowerPoint | Visio | Altium Designer | KiCad | ...}

## Deliverables

- {deliverable 1} — {format}
- {deliverable 2} — {format}

## Review Process

{how feedback is collected and iterations are tracked}

## Status

- Phase: `{planning|drafting|review|final}`
- Last updated: `{date}`
```

### 嵌入式开发

```md
# {project_name}

## Goal

{one-sentence goal}

## Hardware

- MCU: {model}
- Board: {dev board or custom}
- Toolchain: {Keil | IAR | PlatformIO | ...}

## Key Peripherals

- {peripheral 1}
- {peripheral 2}

## Scope

**Do now**
- {mvp item 1}

**Do not do yet**
- {non-goal 1}

## Status

- Phase: `{design|schematic|pcb|coding|debugging|done}`
- Last updated: `{date}`
```

### Skill/提示词工程

```md
# {project_name}

## Goal

{one-sentence goal}

## Skills

| Skill | Purpose |
|-------|---------|
| `{skill-name}` | {one-line description} |

## Target Platforms

- {Claude Code | Reasonix Code | both | other}

## Distribution

{GitHub | direct copy | package manager}

## Status

- Phase: `{drafting|testing|released}`
- Last updated: `{date}`
```

## `User/CODEMAP.md`

```md
# Code Map

## Root Map

| Path | Purpose | Touch When |
|------|---------|------------|
| `src/` | {purpose} | {trigger} |
| `docs/` | {purpose} | {trigger} |
| `scripts/` | {purpose} | {trigger} |

## Main Entry Points

- App entry: `{path}`
- Build entry: `{path}`
- Test entry: `{path}`

## Module Index

| Module | Path | Responsibility | Related Docs |
|--------|------|----------------|--------------|
| `{module}` | `{path}` | `{responsibility}` | `User/modules/{module}.md` |
```

## `User/ARCHITECTURE.md`

```md
# Architecture

## System Shape

- Type: `{single app|multi-module app|multi-service system}`
- Main boundary: `{summary}`

## Layers

| Layer | Path | Owns | Does Not Own |
|------|------|------|---------------|
| UI | `{path}` | `{summary}` | `{summary}` |
| Service | `{path}` | `{summary}` | `{summary}` |
| Data | `{path}` | `{summary}` | `{summary}` |

## Data Flow

`{actor}` -> `{module}` -> `{module}` -> `{result}`

## Important Contracts

- `{contract 1}`
- `{contract 2}`
```

## `User/TASKS.md`

```md
# Tasks

> Last updated: `{date}`

## Blocked

- [ ] `{task_id}` `{title}` | owner: `{value}` | blocked by: `{value}`

## Doing

- [ ] `{task_id}` `{title}` | priority: `P1` | module: `{value}`

## Todo

- [ ] `{task_id}` `{title}` | priority: `P2` | module: `{value}`

## Done

- [x] `{task_id}` `{title}` | done: `{date}`

## Task Notes

| ID | Acceptance Signal | Dependencies | Notes |
|----|-------------------|--------------|-------|
| `{task_id}` | `{how to verify}` | `{ids}` | `{notes}` |
```

## `User/CHANGELOG.md`

```md
# Changelog

## Format

### `{date}` - `{title}`
- Reason: `{why}`
- Change: `{what changed}`
- Impact: `{who or what was affected}`
- Related task: `{task_id or none}`

## Entries
```

## `User/DECISIONS.md`

```md
# Decisions

## Record Format

### DEC-{nnn} `{title}`
- Date: `{date}`
- Status: `{proposed|accepted|superseded}`
- Context: `{what pressure created this decision}`
- Decision: `{what was chosen}`
- Alternatives considered:
  - `{option A}`
  - `{option B}`
- Consequences:
  - `{tradeoff 1}`
  - `{tradeoff 2}`
```

## `User/modules/{module}.md`

```md
# {module_name}

## Responsibility

{what this module owns}

## Inputs

- `{input 1}`
- `{input 2}`

## Outputs

- `{output 1}`
- `{output 2}`

## Dependencies

- `{dependency 1}`
- `{dependency 2}`

## Boundaries

- Must do: `{summary}`
- Must not do: `{summary}`

## Key Files

- `{path}`
- `{path}`

## Active Tasks

- `{task_id}`
```

---

# 写作工程 Templates

> **交付物在根目录。** 大纲、初稿、脚本等创作产物放在项目根目录（与 `User/` 同级），不在 `User/` 内。`User/` 仅保留项目管理文件。init 阶段不创建交付物占位——在后续工作中按需创建。

## `User/REFERENCES.md`（academic only）

```md
# References — {project_name}

## Citation Format

{GB/T 7714 | APA | MLA | ...}

## Bibliography

| # | Citation | Status |
|---|----------|--------|
| 1 | {full citation} | {to-read | read | cited} |

## Source Notes

{key sources, data, quotes with page numbers}
```

---

# 设计工程 Templates

## `User/MATERIALS.md`

```md
# Materials — {project_name}

## Assets

| File | Type | Source | License |
|------|------|--------|---------|
| `{path}` | {image | icon | font | template} | {URL or "original"} | {license} |

## To Acquire

- {asset description} — needed by {date}
```

## `User/DELIVERABLES.md`

```md
# Deliverables — {project_name}

| # | Deliverable | Format | Reviewer | Deadline | Status |
|---|-------------|--------|----------|----------|--------|
| 1 | {name} | {.pptx | .pdf | .png | ...} | {who} | {date} | {todo | drafting | reviewing | done} |
```

---

# 嵌入式开发 Templates

## `User/PARTS.md`

```md
# Parts List — {project_name}

| # | Part | Spec | Qty | Supplier | Notes |
|---|------|------|-----|----------|-------|
| 1 | {MCU model} | {package, speed} | 1 | {link or name} | |
| 2 | {component} | {value, tolerance} | {qty} | {link or name} | {notes} |

## Power Budget

{voltage rails, current estimates}
```

## `User/SCHEMATIC-INDEX.md`

```md
# Schematic Index — {project_name}

| Sheet | Purpose | Key Components | Connects To |
|-------|---------|---------------|-------------|
| 1 | {power supply} | {regulator, caps} | sheets 2, 3 |
| 2 | {MCU minimum system} | {MCU, crystal, reset} | sheets 1, 3, 4 |
```

## `User/MINIMAL-SYSTEM.md`

```md
# Minimal System Verification — {project_name}

## Verification Order

1. Power: {expected voltage, actual}
2. Clock: {expected frequency, observed}
3. Reset: {behavior}
4. IO: {pin states}

## Results

| Step | Expected | Actual | Pass? |
|------|----------|--------|-------|
| 1. Power | {V} | {V} | ✅ / ❌ |
| 2. Clock | {Hz} | {Hz} | ✅ / ❌ |
```

---

# Skill/提示词工程 Templates

## `User/CODEMAP.md`（simplified for Skill project）

```md
# Code Map

## Root Map

| Path | Purpose | Touch When |
|------|---------|------------|
| `{shared-master}/` | canoncial content source | when editing skill body text |
| `{platform-1}/` | {platform} adapted version | when syncing from shared master |
| `{platform-2}/` | {platform} adapted version | when syncing from shared master |

## Sync Direction

{shared-master} → {platform-1}, {platform-2} (one-way, never reverse)
```

## `User/PLATFORM-ADAPT.md`

```md
# Platform Adaption — {project_name}

## Platforms

| Platform | Frontmatter Fields | Notes |
|----------|-------------------|-------|
| {Claude Code} | `tools` | tool names use PascalCase |
| {Reasonix Code} | `runAs`, `allowedTools` | tool names use snake_case |

## What Never Diverges

- SKILL.md body text
- `references/` files
- skill logic and workflow

## Sync Process

1. Edit in shared master
2. Copy body text to each platform version
3. Preserve each platform's frontmatter
```
