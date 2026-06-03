---
name: karpathy-guidelines
description: Behavioral guidelines to reduce common LLM coding mistakes. Use when writing, reviewing, or refactoring code to avoid overcomplication, make surgical changes, surface assumptions, and define verifiable success criteria.
license: MIT
---

# Karpathy Guidelines

Behavioral guidelines to reduce common LLM coding mistakes, derived from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 项目特定指南

**项目中编写代码需要遵循的特定规范**

- **强制** 禁止使用魔法值。有限的业务状态/类型优先使用枚举，物理常量和协议字符串使用常量。
- **强制** 禁止在 Controller 中写业务逻辑，业务逻辑下沉到 Service / ServiceImpl。
- **强制** 使用 Objects、CollectionUtils 等现有工具类判断对象和集合，新增依赖前先确认 Maven 中是否已存在依赖。
- **强制** 项目中如果有 Swagger、Knife4j，Controller、DTO、VO 上要使用注解。
- **强制** 代码中要加注释，符合 Google 的 Javadoc 标准，核心逻辑也要加注释，注释使用中文。
- **强制** 使用 Lombok 简化代码。
- **推荐** 数据校验优先使用统一校验和断言能力，并在全局异常处理中统一返回。

## 兜底规则

**项目编码应遵循的兜底规则**

- 必须严格遵循 CLAUDE.md，不要为了快速实现采用临时方案。修改前先搜索同类实现并给出方案；实现后按规范自检清单报告。若规范实现需要更大改动，先询问我，不允许自行降级实现质量。
