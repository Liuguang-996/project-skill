# File Expectations

What each document file should capture. Not all files are created for every project type — see SKILL.md step 6 for the matrix.

## Layout Variants

Management files always live under `User/`, but the nesting depth varies:

| Layout | Path | When to use |
|---|---|---|
| Single-layer | `User/README.md`, `User/TASKS.md`, ... | Standalone projects; small/medium sub-projects |
| Nested | `User/<sub>/User/README.md`, ... | Multi-domain roots; large meta-projects where each sub-domain needs its own management layer |

In nested layout, the `User/` convention recurses — an agent inside `<sub>/` finds management at `<sub>/User/`, same as it would at root level.

## Universal Files (created for all project types)

### README.md

Project purpose and boundaries in one screenful. Keep it short enough to read every session.

For any type, capture:
- project name
- one-sentence goal
- current phase and last-updated date
- explicit scope (what's included now, what's excluded for now)

Type-specific README additions:
- **代码工程**: setup/run commands, stack summary table
- **写作工程**: audience, deadline, submission format
- **设计工程**: tool, deliverable list, review process
- **嵌入式开发**: target MCU, toolchain, hardware dependencies
- **Skill 工程**: target platforms, skill list, distribution method

### TASKS.md

Durable task tracking. Every task: ID, title, status (todo/doing/blocked/done), priority (P0-P3), acceptance signal.

Group by status: Blocked → Doing → Todo → Done. Include a task notes table.

Top-level metadata:
```markdown
> 巡检计数：0/8（上次巡检：未执行）
```

### CHANGELOG.md

What changed, when, why it mattered. Not the place for architectural reasoning.

Format each entry:
```markdown
### {date} - {title}
- Reason: {why}
- Change: {what}
- Impact: {who/what was affected}
- Related task: {task_id or none}
```

### DECISIONS.md

Key tradeoffs and their consequences. One record per meaningful architectural or process decision.

Format:
```markdown
### DEC-{nnn} {title}
- Date: {date}
- Status: proposed | accepted | superseded
- Context: what pressure created this decision
- Decision: what was chosen
- Alternatives considered:
  - {option A}
  - {option B}
- Consequences:
  - {tradeoff}
```

## Type-Specific Files

> **All type-specific files listed below live in `User/` — they are project-management documents, not deliverables.** Outlines, drafts, scripts, and other creative products belong at project root level, alongside `User/`. The project-init skill only creates management files; deliverables are created later by the user or by follow-up skills.

### CODEMAP.md（代码工程 / Skill 工程）

Directory-to-responsibility map. First navigation file.

For each directory/file: path, purpose, touch-when trigger. Include a "修改时需要同步的文件" section for code with replication constraints.

### ARCHITECTURE.md（代码工程）

System boundaries, layers, data flow, module responsibilities.

Sections: System Shape, Layers (table with owns/does-not-own), Data Flow (actor→module→result), Important Contracts.

### modules/{module}.md（代码工程 medium+）

One file per module. Responsibility, inputs, outputs, dependencies, boundaries, key files, active tasks.

### REFERENCES.md（写作工程，academic sub-type only）

Bibliography, citations, sources. Format depends on sub-type (GB/T 7714 for Chinese academic, APA/MLA for others).

### MATERIALS.md（设计工程）

Asset inventory — images, icons, fonts, templates, reference files. Paths and sources.

### DELIVERABLES.md（设计工程）

What to produce, format, reviewer, deadline per deliverable.

### PARTS.md（嵌入式开发）

Component list — part number, quantity, supplier, notes.

### SCHEMATIC-INDEX.md（嵌入式开发）

Index of schematic sheets — each sheet's purpose, key components, connections to other sheets.

### MINIMAL-SYSTEM.md（嵌入式开发）

Minimum system verification record — power, clock, reset, IO. Verification order and results.

### PLATFORM-ADAPT.md（Skill 工程）

Platform-specific adaption notes — which fields differ per platform (frontmatter, tool declarations, etc.), sync direction, what must never diverge.