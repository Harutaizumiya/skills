---
name: architect
description: Analyze an existing codebase or project materials and generate `docs/structure.md` in the current project root. Use when the user asks to generate a structure document, analyze a project, summarize project modules, infer module relationships, or explain how the project is organized from source files and configuration.
---

# Architect

## Overview

Inspect the current project, extract its core capabilities, partition them into complete modules, infer the relationships between those modules, and write a concise `docs/structure.md`.

Work from source artifacts first. Prefer repository files, configuration, entrypoints, and directory structure over guesses.

## Workflow

1. Identify the project entrypoints and the main product surface.
2. Extract the core functions the project implements.
3. Group those functions into modules with non-overlapping responsibilities.
4. Verify that the module list fully covers the identified functions.
5. Infer the data flow and dependency direction between modules.
6. Write `docs/structure.md` in the required format.

## Analysis Rules

- Read the smallest set of files needed to establish architecture. Start with `README*`, package manifests, build files, routing or startup files, and top-level source directories.
- Prefer `rg` and targeted file reads over broad scans.
- Base module boundaries on responsibilities, not folders alone.
- Merge tiny implementation details into a parent module unless they represent a distinct architectural role.
- Avoid creating modules that overlap in purpose.
- If the repository is incomplete, infer conservatively from the available code and keep assumptions minimal.
- Do not add unrelated commentary, history, or speculative redesign advice.

## Output Requirements

Write exactly these sections in `docs/structure.md`:

```markdown
# Project Structure

## Overview
...

## Modules
- 名称：职责

## Data Flow
...

## Next Steps
...
```

## Content Requirements

- In `Overview`, describe the project goal and the main architectural shape in a short paragraph.
- In `Modules`, list the complete module set needed to cover the identified functions.
- In `Data Flow`, describe how requests, data, or control move across modules.
- In `Next Steps`, list only immediate, high-value follow-up items such as missing documentation, unclear boundaries, or validation points.
- Keep the document concise and concrete.

## Writing Rules

- Create the `docs/` directory if it does not exist.
- Overwrite `docs/structure.md` with the newly generated version.
- Use clear Chinese for module duties when the user writes in Chinese, unless the repository is clearly English-only and consistency matters more.
- Ensure every listed module is referenced or implied by the observed project artifacts.
- Do not include sections outside the required format.

## Minimal Example

```markdown
# Project Structure

## Overview
这是一个以 Web API 为核心的项目，负责接收请求、执行业务规则、持久化数据，并向外部系统同步结果。

## Modules
- 接入层：处理路由、参数校验和请求分发
- 业务层：实现核心业务规则和用例编排
- 数据访问层：负责数据库读写和持久化
- 集成层：对接第三方服务与异步任务

## Data Flow
请求从接入层进入，业务层执行用例并调用数据访问层读写数据；涉及外部依赖时由集成层完成同步或异步交互，结果再返回接入层输出。

## Next Steps
- 核对模块边界是否与实际目录结构一致
- 补充关键入口文件与主要依赖说明
```
