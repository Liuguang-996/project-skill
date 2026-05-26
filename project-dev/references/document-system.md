# Project Dev Document System

Reference for the `project-dev` skill. Covers sync matrices, inspection checklists, and verification guidance — organized by project type.

---

# Universal (all types)

## Task Schema

Keep task entries stable and grep-friendly.

```text
ID: PS-XXX (or PM-XXX, any prefix unique to the project)
Title: one-line summary
Status: todo | doing | blocked | done
Priority: P0 | P1 | P2 | P3
Module: {module or section name}
Dependencies: {task IDs}
Acceptance: {how to verify completion}
```

## Decision Schema

```text
ID: DEC-{nnn}
Status: proposed | accepted | superseded
Context: why this decision became necessary
Decision: what was chosen
Alternatives: what else was considered
Consequences: what this enables and what it costs
```

## Track A: Self-Check After Sync (all types)

After every sync, answer three questions:

1. Does the new content contradict existing content in the same file?
2. Does the new content contradict content in other `User/` files?
3. Is there now-obsolete information that should have been removed but wasn't?

Fix contradictions immediately. Mark uncertain obsolescence with `<!-- TODO: 待核实 -->`.

## Track A: Rollback Point (all types)

Before modifying any `User/` file, copy the pre-edit content into the current CHANGELOG entry under `**同步前快照：**`.

## Document Inspection (on request)

Triggered when user says "巡检文档" / "文档体检" / "检查 User 文档". Universal checks for all types:

| # | Check | Method |
|---|-------|--------|
| U1 | **Stale references** | Compare doc paths against actual files. Flag paths that no longer exist. |
| U2 | **Internal contradictions** | Cross-compare all `User/` files for conflicting descriptions. |
| U3 | **Redundancy bloat** | CHANGELOG >30 entries → suggest archiving to `User/CHANGELOG-archive.md`. TASKS done section >20 items → suggest archiving. |
| U4 | **Recoverability test** | If a new session only reads `User/`, can it understand the project's purpose, structure, current state, and how to start? |

---

# 代码工程

## Track A: Sync Matrix

| Impact | Signal | Sync target |
|--------|--------|-------------|
| Behavior or output changed | New/removed feature, different result | `CHANGELOG.md` + `TASKS.md` (if task status changed) |
| Module boundary changed | New/deleted module, responsibility shift | above + affected `User/modules/*.md` |
| Navigation or ownership changed | Directory renamed, file moved, ownership reassigned | above + `CODEMAP.md` |
| Data flow or layering changed | New pipeline, layer boundary moved, contract changed | above + `ARCHITECTURE.md` |
| Strategic decision made | Tradeoff chosen, alternative rejected | above + `DECISIONS.md` |

## Type-Specific Inspection

| # | Check | Method |
|---|-------|--------|
| C1 | **CODEMAP coverage** | Scan project root. Directories/files not in CODEMAP? Flag them. |
| C2 | **Module card coverage** | Modules without cards in `User/modules/`? Flag them. |
| C3 | **CODEMAP vs ARCHITECTURE consistency** | Do both describe the same module structure and locations? |
| C4 | **Entry points documented** | Are app/build/test entry paths in CODEMAP still accurate? |

## Verification Options

- Unit/integration tests
- Build or lint (`tsc --noEmit`, `eslint`, etc.)
- Runtime smoke test
- Manual validation against acceptance signal

---

# 写作工程

## Track A: Sync Matrix

> **交付物在项目根目录，不在 `User/` 内。** 大纲、初稿、脚本等文件位于项目根目录（与 `User/` 同级）。同步时提醒用户检查这些交付物是否需要同步修改，不在 `User/` 内自动更新它们。

| Impact | Signal | Sync target |
|--------|--------|-------------|
| Content or output changed | New/revised text, section rewritten | `CHANGELOG.md` + `TASKS.md` (if task status changed) + remind user to update root-level deliverable files |
| Structure changed | New/deleted/reordered sections | above + remind user to update root-level outline/structure file |
| Draft updated | Content written or revised | above + remind user to update root-level draft file |
| Citations changed | New/removed/updated references | above + `REFERENCES.md` (academic sub-type, in `User/`) |
| Strategic decision made | Tone, audience, or structural tradeoff chosen | above + `DECISIONS.md` |

## Type-Specific Inspection

| # | Check | Method |
|---|-------|--------|
| W1 | **Citation completeness** (academic) | Are all in-text citations present in REFERENCES.md? Any "to-read" still unresolved? |
| W2 | **Deadline tracking** | Is the deadline in README.md still relevant? Is progress on track? |
| W3 | **Deliverable ↔ User/ consistency** | Do the root-level deliverables match what README.md and TASKS.md describe? |

## Verification Options

- Word count against target
- Citation format check (GB/T 7714, APA, etc.)
- Deliverable file existence and content check
- Manual re-read for flow and consistency

---

# 设计工程

## Track A: Sync Matrix

| Impact | Signal | Sync target |
|--------|--------|-------------|
| Deliverable created/revised | New or updated output file | `CHANGELOG.md` + `TASKS.md` (if task status changed) |
| Asset changed | New/removed/modified asset | above + `MATERIALS.md` |
| Deliverable status changed | Review passed, finalized | above + `DELIVERABLES.md` |
| Strategic decision made | Design tradeoff chosen | above + `DECISIONS.md` |

## Type-Specific Inspection

| # | Check | Method |
|---|-------|--------|
| D1 | **MATERIALS completeness** | Are all assets in design files listed in MATERIALS.md? |
| D2 | **DELIVERABLES status accuracy** | Do the statuses in DELIVERABLES.md match what's on disk? |
| D3 | **Tool consistency** | Is the tool in README.md still the one being used? |

## Verification Options

- Deliverable format check (file extension, dimensions, export settings)
- Asset list vs actual files
- Reviewer sign-off
- Manual visual inspection

---

# 嵌入式开发

## Track A: Sync Matrix

| Impact | Signal | Sync target |
|--------|--------|-------------|
| Firmware behavior changed | New/fixed functionality | `CHANGELOG.md` + `TASKS.md` (if task status changed) |
| Component changed | New/removed/changed part | above + `PARTS.md` |
| Schematic changed | Sheet added/modified | above + `SCHEMATIC-INDEX.md` |
| Verification run | Test results obtained | above + `MINIMAL-SYSTEM.md` |
| Strategic decision made | Architecture or component tradeoff | above + `DECISIONS.md` |

## Type-Specific Inspection

| # | Check | Method |
|---|-------|--------|
| E1 | **PARTS vs SCHEMATIC consistency** | Are all components in the schematic listed in PARTS.md? |
| E2 | **MINIMAL-SYSTEM currency** | Do verification results reflect the current hardware revision? |
| E3 | **MCU/toolchain documented** | Are MCU model and toolchain in README.md accurate? |
| E4 | **Power budget** | Is the power budget in PARTS.md populated and consistent with the schematic? |

## Verification Options

- Build/compile (Keil, IAR, PlatformIO)
- Flash to target and run
- Oscilloscope / logic analyzer measurements
- Minimum system checks (power → clock → reset → IO)

---

# Skill/提示词工程

## Track A: Sync Matrix

| Impact | Signal | Sync target |
|--------|--------|-------------|
| Skill body changed | Text, workflow, or rules modified | `CHANGELOG.md` + `TASKS.md` (if task status changed) |
| Platform adaptation changed | Frontmatter or tool declarations changed | above + `PLATFORM-ADAPT.md` |
| Reference files changed | `references/` content changed | above + sync affected refs to all platforms |
| Directory structure changed | New/moved skill files | above + `CODEMAP.md` |
| Strategic decision made | Design tradeoff chosen | above + `DECISIONS.md` |

## Type-Specific Inspection

| # | Check | Method |
|---|-------|--------|
| S1 | **Platform sync status** | Does each platform version's SKILL.md body match shared master? Frontmatter different but body identical? |
| S2 | **Reference file consistency** | Are `references/` files identical across all platform versions? |
| S3 | **CODEMAP accuracy** | Do the paths in CODEMAP match the actual directory structure? |
| S4 | **Sync direction respected** | Any evidence of platform-version body edits? (Check modification times or diff.) |

## Verification Options

- Test the skill in-session (does it behave as expected?)
- Frontmatter consistency: body identical across all 3 variants, only frontmatter differs
- Sync check: `diff` shared master vs each platform version's body

---

# Inspection Report Format (all types)

```markdown
## 文档巡检报告（YYYY-MM-DD）

### 🔴 矛盾（需立即修复）
- CODEMAP 写 `src/services/` 存在，实际目录已改为 `src/api/`

### 🟡 过时（建议更新）
- TASKS 中 PM-003 标记"进行中"，但 CHANGELOG 显示已上线

### 🟢 建议（可选优化）
- CHANGELOG 累积 47 条，建议归档 30 天前的条目

确认后执行修复？回复"修复全部"或逐条确认。
```

# Acceptance Signals

Prefer explicit acceptance signals over vague completion language.

Good signals (by type):

- 代码工程: test name passed, build succeeds, endpoint returns expected response
- 写作工程: word count target met, outline matches draft structure, all citations resolved
- 设计工程: deliverable exported in correct format, reviewer approved
- 嵌入式开发: firmware compiles, flashes, passes minimum system check
- Skill 工程: skill test in-session produces expected behavior, sync diff is clean

Weak signals (avoid):

- "looks fine"
- "should work"
- "probably fixed"