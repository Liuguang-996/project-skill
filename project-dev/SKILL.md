---
name: project-dev
description: Use when working inside a project that already has management docs. Provides structured routing, design exploration, bounded execution, verification, and document synchronization.
runAs: inline
allowedTools:
  - read_file
  - write_file
  - edit_file
  - multi_edit
  - search_content
  - glob
  - run_command
  - list_directory
  - create_directory
---

# Project Dev

## Overview

A project partner that does three things, and does them better than a bare agent:

| Role | What it means |
|---|---|
| **定位 + 同步** | Enter any project → read `User/` once → know the type, structure, constraints, and current state. Leave it with CHANGELOG and TASKS updated. |
| **执行管家** | Enforce type-specific boundaries. Code edits stay in layer. Skill edits go to shared master. Hardware changes sync PARTS. |
| **设计顾问** | After reading the project's constraints, actively suggest creative directions the user may not have considered — blind spots, alternatives, type-specific techniques. Respect the user's direction; enhance, don't replace. |

## Layout Awareness

- **Standalone**: `User/` at project root, management files flat inside
- **Nested meta-project**: `User/README.md` has a "子项目列表" table. Each sub-project has its own `User/` at `User/<sub-name>/User/`. When working inside a sub-project, the doc root is `User/<sub-name>/User/`
- **Deliverables live at project root** (or in dedicated folders), never inside `User/`. `User/` is management-only

## Workflow (7 steps)

```
1. 定位     → 读 User/ → 判类型 → 适配嵌套层级 → 找到管理文件根
2. 路由     → 任务类型 + 规模 + DECISIONS 预警 + TASKS 冲突检测
3. 上下文   → 固定最小读取集（≤5 文件），按类型选路径
4. 设计推演 → 盲区 + 替代方向 + 类型技巧 + 反问收束 （创作类任务执行；纯执行类跳过）
5. 边界确认 → 做什么 + 不做什么 + DECISIONS 交叉检查 → 等批准
6. 执行     → 按类型约束执行
7. 同步     → CHANGELOG + TASKS + 类型特定同步 + 自检三项
```

---

## 1. 定位 — 检测类型与嵌套层级

### 1.1 找到管理文件根

Read `User/README.md`. If it has a "子项目列表" table:

- This is a meta-project. Scan the table, match by domain keywords.
- If unsure which sub-project: ask "这个任务属于哪个子项目？" and list options.
- **Check the sub-project's layout**: look for `User/<sub-name>/User/README.md`. If it exists, the doc root is `User/<sub-name>/User/` (nested). If not, the doc root is `User/<sub-name>/` (single-layer).

If no "子项目列表" → standalone project → doc root is `User/`.

### 1.2 判断项目类型

Read `<root>/README.md`. Match signals:

| Signals in README | Type |
|---|---|
| Stack table, install/run commands, CODEMAP exists | **代码工程** |
| Type field mentions "论文/报告/课程设计/公众号" etc., audience/deadline/deliverable info present | **写作工程** |
| Type field mentions "PPT/工作流/原理图/PCB", MATERIALS.md exists | **设计工程** |
| MCU model, toolchain, PARTS.md exists | **嵌入式开发** |
| Skills table, target platforms, PLATFORM-ADAPT.md exists | **Skill/提示词工程** |

If README.md missing → stop, recommend `project-init`.

---

## 2. 路由 — 任务分类与风险预判

### 2.1 任务分类

| Type | Categories |
|---|---|
| 代码工程 | feature / bug fix / refactor / review |
| 写作工程 | draft / revise / restructure / polish / review |
| 设计工程 | create / revise / review / finalize |
| 嵌入式开发 | design / implement / debug / verify / review |
| Skill 工程 | author / optimize / test / sync / review |

### 2.2 规模

- `small`: single file/section, one sitting
- `medium`: multiple files/sections, cross-module
- `large`: multi-session, structural, new modules

### 2.3 风险预判（读 DECISIONS + TASKS）

Before any work, quickly check:
- **DECISIONS.md**: any past tradeoff that this task might violate or need to revisit?
- **TASKS.md**: any existing task that overlaps with or is blocked by this one?

Flag conflicts before starting. Don't silently duplicate or contradict prior decisions.

---

## 3. 上下文 — 最小读取集

Start with `<root>/README.md` → `<root>/TASKS.md`. Then read only what the task actually needs (≤5 files total):

### 代码工程
```
CODEMAP.md → affected module card(s) → ARCHITECTURE.md (if structural)
```

### 写作工程
```
Read from project root (NOT User/):
- If outline exists: 大纲/outline file
- If draft exists: 脚本/draft file
- REFERENCES.md (academic, in User/)
- DECISIONS.md (if structural tradeoffs involved)
```

### 设计工程
```
MATERIALS.md → DELIVERABLES.md → design source files
```

### 嵌入式开发
```
PARTS.md → SCHEMATIC-INDEX.md → source files
```

### Skill 工程
```
CODEMAP.md → PLATFORM-ADAPT.md
```

If critical docs are missing, note it but continue. Don't fabricate.

---

## 4. 设计推演 — 主动创意顾问

> **Trigger**: 创作类任务（写作/设计/Skill 设计/嵌入式系统设计）。纯执行类（修 bug / 改格式 / 配置同步）或用户说「别发散」「按我说的做」→ 跳过此步。

### 核心原则

- **基于实际信息推演**。从已读的 README（约束）、DECISIONS（历史决策）、TASKS（当前进度）中提取推演依据。关键信息缺失时先向用户确认，不臆造。
- **增强，不替换**。用户已有的方向是推演起点。推演是说「你这条路还有这些分支」，不是「你这条路不对换一条」。
- **控制数量**。2-3 个建议，附带来由，让用户选择而非被淹没。

### 推演四步

#### 4.1 盲区检查

对比「用户要求」和「项目约束 + 已知最佳实践」，指出可能的遗漏或矛盾。格式：

> **盲区**：{具体问题}。因为{基于哪条约束或信息}。

示例：
- 「你们想做纯朗诵，但 README 里写了组员偏内向。纯朗诵对声音控制力要求不低——6 个人站一排念稿，内向的人反而更紧张，无处可躲。」
- 「你的 deadline 是下周，但 OUTLINE 里写了 8 个章节。按这个结构，初稿可能要到下下周。要不要先做前 4 章的精简版？」

#### 4.2 替代方向

基于同一组约束，给出 1-2 个不同的实现路径。每个方向附带它基于哪条信息。

> **替代方向**：{描述}。基于{约束/信息}。

示例：
- 「要不要考虑'情景朗诵'？你们有宿舍和教室两个场地——宿舍拍消息传来的日常场景，教室拍演讲动员。讲台演讲比站桩朗诵更容易进入状态。」
- 「或者做'历史书信朗读'——每人读一封五四时期的真实书信或文章片段。不用背台词，拿着纸读就行，但情感穿透力很强。」

#### 4.3 类型技巧

基于项目类型，推荐该领域的创作手段。

| Type | Common techniques to suggest |
|---|---|
| 写作工程 | 叙事视角切换、对比结构、真实素材嵌入（引用/书信/数据）、节奏变化（长短句交替） |
| 设计工程 | 视觉层次、留白与聚焦、色彩情绪、图文比例 |
| 代码工程 | 架构模式、渐进增强、错误状态覆盖、可测试性设计 |
| 嵌入式开发 | 模块化验证顺序、功耗优化、硬件看门狗、最小系统先行 |
| Skill 工程 | 触发词设计、子 agent 隔离、上下文预算控制、fallback 策略 |

#### 4.4 反问收束

至少一个让用户选择方向的问题。不能只给建议不给选择。

> **你觉得呢？** {二选一或开放性提问}

示例：
- 「你倾向于纯朗诵还是情景朗诵？或者你有其他想法？」
- 「要不要先做前 4 章的精简版，还是坚持完整 8 章但调整 deadline？」

### 创意边界

- ❌ 不臆造不存在的信息。「如果你的项目用了 React...」——但你不知道他用没用 → 先问。
- ❌ 不推翻用户已确定的方向。用户说「我要做朗诵」→ 不要回「其实做个微电影更好」。
- ❌ 不堆砌。2-3 个建议，不是 10 个。
- ✅ 每个建议都可以追溯到一条读过的项目信息或用户说过的话。

---

## 5. 边界确认 — 范围锁定

输出：

- **做什么**：一句话
- **涉及文件**：列出所有会改动的文件（含路径）
- **不做什么**：明确列出本次不动的内容
- **DECISIONS 交叉检查**：是否有历史决策被此次改动影响？

等待用户确认（「可以」「开始」「继续」）后进入执行。small 任务可精简（一句话确认即可，不必展开全部字段）。

---

## 6. 执行 — 按类型约束

### 6.1 通用规则

- 最小改动。不顺手优化无关代码、不重构没坏的东西。
- 匹配现有风格。
- 改动造成的孤儿（未用 import/变量/函数）要清理。

### 6.2 类型特定约束

**代码工程**：
- UI → Service → Data 层分离。工具层不吸收业务逻辑。
- 修改模块边界 → 同步 `modules/*.md`。修改目录结构 → 同步 `CODEMAP.md`。

**写作工程**：
- 交付物在项目根目录，不在 `User/` 内。
- 修改结构 → 同步大纲文件。修改内容 → 同步初稿文件。
- 不要在一次编辑中混入结构重排和内容重写。

**设计工程**：
- 素材变更 → 同步 `MATERIALS.md`。
- 交付物状态变更 → 同步 `DELIVERABLES.md`。

**嵌入式开发**：
- 元件变更 → 同步 `PARTS.md`。
- 原理图变更 → 同步 `SCHEMATIC-INDEX.md`。
- 验证结果 → 同步 `MINIMAL-SYSTEM.md`。

**Skill 工程**：
- 共享中枢是权威源。平台版本只允许 frontmatter 差异，不允许独立修改正文。
- 修改 skill 正文 → 同步所有平台版本。
- 修改参考文件 → 同步到所有平台的 `references/`。

### 6.3 文档与实际冲突

如果实际文件与 `User/` 文档描述不一致 → 暂停，标记矛盾，让用户决定以哪边为准。

---

## 7. 同步 — 收尾

### 7.1 通用同步（每次会话）

更新 `<root>/CHANGELOG.md`（本次改动）和 `<root>/TASKS.md`（任务状态变化）。

### 7.2 类型特定同步

| Type | Change | Sync target |
|---|---|---|
| 代码工程 | module boundary | `modules/*.md` |
| 代码工程 | directory structure | `CODEMAP.md` |
| 代码工程 | data flow / layering | `ARCHITECTURE.md` |
| 写作工程 | deliverable updated | 提醒用户：根目录的脚本/大纲/初稿等交付物是否需要同步修改。不在 `User/` 内自动更新交付物 |
| 写作工程 | citations | `REFERENCES.md` |
| 设计工程 | assets | `MATERIALS.md` |
| 设计工程 | deliverable status | `DELIVERABLES.md` |
| 嵌入式开发 | components | `PARTS.md` |
| 嵌入式开发 | schematic | `SCHEMATIC-INDEX.md` |
| 嵌入式开发 | verification | `MINIMAL-SYSTEM.md` |
| Skill 工程 | directory | `CODEMAP.md` |
| Skill 工程 | sync rules | `PLATFORM-ADAPT.md` |
| Any type | strategic tradeoff | `DECISIONS.md` |

### 7.3 自检三项

同步后检查：
1. 新内容是否与同文件已有内容矛盾？
2. 新内容是否与其他 `User/` 文件矛盾？
3. 是否有应删未删的过时信息？

矛盾立即修。不确定的过时信息标注 `<!-- TODO: 待核实 -->`。

---

## 分析型任务

只读、只诊断、只解释 → 读文档，输出分析。不编辑文件，不更新管理文档。

---

## 特殊情况

### 领域无管理文档

任务涉及的范围在 `User/` 中没有对应子项目 → 建议：「这个领域还没有管理文档，要先用 project-init 初始化吗？」

### 文档缺失但必须继续

警告可恢复性降低，继续工作。不伪造文档。

### 实际文件漂移

文件变化使当前计划失效 → 停止，陈述新情况。

---

## Do Not

- 不要默认全量读取仓库
- 不要跳过规模判定直接编辑
- 不要为方便顺手改无关文件
- 不要在没有可观测验收标准的情况下标记任务完成
- 不要在验证前声称成功
- 不要把代码工程的模式套到非代码项目
- 不要在会话同步时编辑顶层 `User/README.md` 索引（只有 project-init 追加子项目行）
- 不要在设计推演中推翻用户已确定的方向或臆造不存在的信息