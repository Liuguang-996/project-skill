---
name: project-init
description: Use when starting a new project, standardizing an ad hoc project, or repairing a repo that lacks durable project-management docs before implementation begins.
runAs: inline
allowedTools:
  - read_file
  - write_file
  - create_directory
  - glob
  - ask_choice
---

# Project Init

## Overview

Initialize a durable project-management document system before execution starts. Supports two structural modes, each with two layout variants:

**Structural modes:**
1. **Standalone project**: creates `User/` directory directly in the project root
2. **Sub-project under a meta-project**: when a `User/README.md` index already exists in a parent directory, appends a new sub-project

**Layout variants:**
- **Single-layer (default)**：management files flat in `User/` or `User/<sub-name>/` — right for small/medium projects where the index won't bloat
- **Nested**：each sub-project gets its own `User/` subdirectory (`User/<sub-name>/User/`) — right for multi-domain roots where each domain needs independent management docs. The `User/` convention recurses at every level so agents always find management docs at `User/` relative to their working directory.

## Core Rule

Detect project type first, then collect only the context that type actually needs. Write files only after the user has confirmed the framing is accurate and explicitly signals that creation can begin.

## Initialization Workflow

### 0. Check for existing User/ index (meta-project detection)

Before anything else, check `User/README.md` in the target root. If it exists and contains a "子项目列表" table:

- This is a **meta-project** — the target directory manages multiple independent domains
- Read the existing index to understand what sub-projects already exist
- Ask the user: is this a **new sub-project** or an **update to an existing one**?
- If new sub-project: ask for a short kebab-case sub-folder name, then proceed to type detection. Management docs will go in `User/<sub-name>/User/` (nested layout) or `User/<sub-name>/` (single-layer), depending on the nesting strategy determined in step 5.5
- If existing: navigate to that sub-folder and fill gaps (skip to Special Case: "Existing project with partial docs")
- Do NOT overwrite the top-level `User/README.md` index — only append to it

If `User/README.md` does NOT exist, or exists but has no "子项目列表" table, proceed as standalone project (step 1).

### 1. Confirm this is the correct root

Check whether the current directory is the project root. For standalone projects, `User/` goes directly here. For sub-projects, docs go into `User/<sub-name>/`.

If the repo already contains a competing documentation system, do not overwrite it silently. Instead:

- identify the existing system
- explain how the new `User/` structure would overlap with it
- ask whether to merge, replace, or keep both

### 2. Detect project type

Before asking any questions, classify the project into one of these types:

| Type | Typical signals | Key questions to confirm |
|------|-----------------|--------------------------|
| **代码工程** | source directories, `package.json`, build configs, existing code files | runtime, deploy target, entry points |
| **写作工程** | drafts, outlines, `.md` / `.docx` files, mentions of "论文"/"报告"/"商业计划书"/"公众号" | sub-type (academic / competition / course design / business plan / blog post), audience, submission format |
| **设计工程** | mentions of "PPT"/"工作流"/"原理图"/"PCB", design files, schematics | deliverable format, tool (PPT/Visio/AD/...), review process |
| **嵌入式开发** | `.c`/`.h` in embedded context, mentions of "MCU"/"单片机"/"开发板", schematic files | MCU model, toolchain (Keil/IAR/...), hardware dependencies |
| **Skill/提示词工程** | `.md` skill files, mentions of "skill"/"prompt"/"AI助手", no executable code | target platforms, distribution method |

### 3. Fast path check

If the user's very first message already covers name, goal, rough scope, and the type is obvious, skip separate Q&A. Summarize your understanding in 3-4 lines and jump directly to step 5 (draft the documentation plan).

### 4. Collect type-specific context

Only ask the questions relevant to the detected type. Scale the depth to project size:

| Type | small | medium | large |
|------|-------|--------|-------|
| 代码工程 | name + goal + stack + deploy form | + module boundaries + non-goals | + collaboration needs + contract boundaries |
| 写作工程 | name + sub-type + audience + deadline | + outline structure + reference needs | + team roles + multi-draft process |
| 设计工程 | name + tool + deliverables | + review process + iteration plan | + team + asset pipeline |
| 嵌入式开发 | name + MCU + toolchain | + peripheral list + verification plan | + hardware-software contract + team |
| Skill 工程 | name + goal + target platforms | + skill list + distribution method | + multi-platform sync strategy |

**Rule for unknowns:** If the user cannot answer a dimension, mark it as `<!-- TODO: 待补充 -->`. Do not keep probing or invent.

### 5. Classify project scale

- `small`: single artifact, one person, straightforward workflow
- `medium`: multiple modules / chapters / deliverables, possibly repeated cycles
- `large`: multi-person, multi-artifact, shared contracts, long-lived

### 5.5 Determine nesting strategy

**Default: single-layer.** Management files go directly in `User/` (standalone) or `User/<sub-name>/` (meta-project sub-project).

**Auto-detect — suggest nested architecture when any of these are true:**
- The project root is a home/user directory (`C:\Users\xxx\`, `/home/xxx/`) containing multiple independent tools, config repos, or project domains
- The root contains 3+ clearly independent project-like subdirectories (each with their own config files, source trees, or distinct purposes)
- The user describes the project as managing multiple independent domains or a "中枢"/"管理"/"索引" role
- Scale is `large` and spans multiple loosely-coupled areas that would each benefit from independent management docs

When these signals fire, tell the user: "这个目录下包含多个独立领域。建议使用嵌套架构——每个子项目有自己的 `User/` 子目录，避免顶层索引越来越长。用嵌套还是单层？"

**Manual override:**
- User says "嵌套" / "多级" / "嵌套架构" / "递归" → use nested layout
- User says "单层" / "简单" / "flat" / "不用嵌套" → use single-layer layout

**Nested layout structure:**
```
User/
├── README.md                — top-level index + 子项目列表
└── <sub-domain-1>/
    └── User/                — this sub-domain's own management layer
        ├── README.md
        ├── TASKS.md
        ├── DECISIONS.md
        └── CHANGELOG.md
```

The `User/` convention recurses at every level. An agent working inside `<sub-domain-1>/` finds its management docs at `User/` — identical convention, just one level down.

### 6. Determine required outputs

**Always created (universal):**

- `User/README.md` — project purpose, boundaries, current status (standalone) or top-level index + 子项目列表 (meta-project)
- `User/TASKS.md` — task tracking with ID, status, priority, acceptance signal
- `User/CHANGELOG.md` — what changed, when, why
- `User/DECISIONS.md` — key tradeoffs and their consequences

For **nested** sub-projects under a meta-project, these four files go in `User/<sub-name>/User/`. For **single-layer** sub-projects, they go directly in `User/<sub-name>/`.

**Created by type:**

| Type | Additional files |
|------|------------------|
| 代码工程 | `CODEMAP.md`, `ARCHITECTURE.md`, `modules/` (medium+) |
| 写作工程 | `REFERENCES.md` (academic) |
| 设计工程 | `MATERIALS.md`, `DELIVERABLES.md` |
| 嵌入式开发 | `PARTS.md`, `SCHEMATIC-INDEX.md`, `MINIMAL-SYSTEM.md` |
| Skill 工程 | `CODEMAP.md` (simplified), `PLATFORM-ADAPT.md` |

**Deliverables go at project root, not inside `User/`.** `User/` is exclusively for project-management architecture. When the project produces an outline, draft, script, shot list, PPT, schematic, or any other deliverable, create it at the project root level — either as a standalone file or inside a deliverables folder — alongside `User/`. Never nest deliverables under `User/`.

Use the templates in `references/document-templates.md`.

### 7. Draft the documentation plan

Before creating files, tell the user:

- detected project type and scale
- whether this is a sub-project (under `User/<sub-name>/`) or standalone
- which files will be created
- which sections will be lightweight placeholders
- what future tasks these docs will support

Wait for explicit confirmation such as "开始", "创建", "可以生成".

### 8. Create the files

Write concise, useful first-pass content. Avoid generic boilerplate.

**For nested sub-projects:** create `User/<sub-name>/User/` and place the four universal files inside. Then append a row to the top-level `User/README.md` "子项目列表" table.

**For single-layer sub-projects:** place the four universal files directly in `User/<sub-name>/`. Then append a row to the top-level `User/README.md` "子项目列表" table.

### 9. Report the result

After creation, report: created files, any sections left lightweight, next task (use `project-dev` skill).

## Writing Rules

- Write for future agents and humans who were not present in the current conversation.
- Keep docs stable and grep-friendly.
- Use exact paths, directory names, and command names when known.
- Mark assumptions explicitly.

## Special Cases

### Meta-project with existing User/ index

When `User/README.md` has a "子项目列表" table:
- **Nested layout**: create `User/<sub-name>/User/` with the four universal files inside. Append one row to the index table.
- **Single-layer layout**: create docs directly in `User/<sub-name>/`. Append one row to the index table.
- Do NOT create a new top-level README.md — only modify the existing one.

### Standalone project (no existing index)

Create `User/` directly in the project root with all management docs flat inside. This is the classic mode. If the project root qualifies for nesting (see step 5.5), suggest it before creating.

### Existing project with partial docs

Preserve existing material. Fill only missing pieces. Normalize structure without rewriting everything.

### Repo too large to document in one pass

Create top-level files now. Add module cards only for currently visible modules. Record remaining modules as follow-up tasks.

### User wants to skip documentation

Warn that future execution quality and recovery will drop, then follow the user's instruction.

## Do Not

- do not create files outside the project root
- do not invent stack details, deployment details, or module boundaries
- do not ask questions irrelevant to the detected project type
- do not keep probing when the user says they don't know — mark it as a placeholder
- do not move into implementation work after initialization unless the user explicitly asks
- do not overwrite an existing `User/README.md` index — only append to its table
- do not force nested layout on small/vertical projects — single-layer is the default and should only be upgraded when the auto-detect signals fire or the user explicitly requests it