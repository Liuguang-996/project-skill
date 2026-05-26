# project-skill

两个 Reasonix Code 的 Skill，覆盖从项目初始化到日常开发的管理文档工作流。

## Skill 概览

| Skill | 用途 | 触发场景 |
|-------|------|---------|
| **project-init** | 初始化项目管理文档系统 | 新建项目、标准化已有项目、补全缺失的管理文档 |
| **project-dev** | 在已有管理文档的项目中执行开发 | 功能开发、Bug 修复、内容创作、设计迭代——任何需要定位→执行→同步的修改任务 |

## 工作流关系

```
project-init（一次）  →  project-dev（反复）
   初始化 User/ 文档        每次会话读 User/ → 执行 → 同步回 User/
```

- **project-init**：建立 `User/` 目录及管理文件（README、TASKS、CHANGELOG、DECISIONS），按项目类型创建专属文件（CODEMAP、ARCHITECTURE、REFERENCES 等）
- **project-dev**：每次工作时读取 `User/` 了解项目类型和约束，执行任务后在 CHANGELOG 和 TASKS 中留下记录

## 支持的项目类型

两个 Skill 覆盖五种项目类型：

- 代码工程（源码、构建配置）
- 写作工程（论文、报告、公众号文章）
- 设计工程（PPT、原理图、PCB）
- 嵌入式开发（MCU、Keil/IAR、硬件验证）
- Skill/提示词工程（跨平台 Skill 同步）

## 安装

将 `project-dev/` 和 `project-init/` 复制到 Reasonix Code 的 skills 目录：

```
~/.reasonix/skills/project-dev/
~/.reasonix/skills/project-init/
```

## 使用

在 Reasonix Code 会话中：

```
/project-init    # 初始化项目管理文档
/project-dev     # 在日常开发中使用
```

或者直接在对话中描述你的需求，Agent 会自动判断并调用对应 Skill。

## 目录结构

```
project-dev/
├── SKILL.md                       # Skill 定义（7 步工作流）
└── references/
    └── document-system.md         # 同步矩阵、巡检清单、验收标准

project-init/
├── SKILL.md                       # Skill 定义（初始化工作流）
└── references/
    ├── document-templates.md      # 各类型项目的文档模板
    └── file-expectations.md       # 每个管理文件的职责说明
```

## 设计原则

- **类型驱动**：先判类型，再取上下文，不做无意义的全量读取
- **最小改动**：只改任务相关的文件，不顺手优化无关代码
- **文档即契约**：User/ 管理文档是 Agent 和人的共享记忆，每次会话以读取 User/ 开始、以同步 User/ 结束
- **交付物分离**：创作产物（代码/文档/设计文件）放在项目根目录，管理文档（TASKS/CHANGELOG/DECISIONS）放在 User/，两者不混杂
