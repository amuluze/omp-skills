# omp-skills

面向 [OMP](https://github.com/can1357/oh-my-pi) 的可复用技能集，源自 axiom 项目内置的 SDD 工作流技能，已按 OMP 的工具与运行语义适配。

## 技能列表

| Skill | 用途 | 依赖 |
|---|---|---|
| `domain` | 把领域要求收敛成可验证的 Domain Spec（`.specs/domain/`） | 无 |
| `brainstorm` | 一次一问澄清需求，产出 Task Spec 并同步 Domain Spec | 无 |
| `diagnose` | 复现 → 二分假设 → 根因 → 修复 Task Spec，不做诊断期改码/提交 | 无 |
| `plan` | 把定稿 Task Spec 拆成带证据映射的实施方案（`.plans/`） | 与 `implement` 成对 |
| `implement` | 按 Plan 测试驱动实现，含测试双检、真实表面验证、INCONCLUSIVE 出口 | 与 `plan` 成对 |
| `finish` | 收口：文档对齐、归档、证据摘要与遗留项；提交仅在你明确要求时发生 | 无 |
| `greeting` | 问候语/开场白写作，按关系-场合-渠道校准，拒绝模板化套话 | 无 |

六个 SDD 技能（`domain`/`brainstorm`/`diagnose`/`plan`/`implement`/`finish`）构成一条链：Spec → 需求澄清/诊断 → Plan → 实现 → 收口。单独使用某一环可以，但链式使用时效果最好。

## 安装

布局为 `<skill>/SKILL.md`，符合 OMP 的 `skills/` 或 `customDirectories` 一层扫描约定。

### 方式一：全局（所有项目可用）

```bash
mkdir -p ~/.omp/agent/skills
cp -R brainstorm diagnose domain finish greeting implement plan ~/.omp/agent/skills/
omp skill list
```

### 方式二：项目级（只在该仓库可用）

```bash
mkdir -p .omp/skills
cp -R /path/to/omp-skills/{domain,brainstorm,diagnose,plan,implement,finish,greeting} .omp/skills/
```

### 方式三：作为自定义目录源（不复制文件）

克隆本仓库后，在 `~/.omp/agent/config.yml` 或项目 `.omp/config.yml` 中加入：

```yaml
skills:
  customDirectories:
    - ~/src/omp-skills
```

自定义目录中的技能优先级高于默认 provider 的同名技能。

## 使用

- 让模型按描述自动触发；或显式调用 `/skill:<name>`（如 `/skill:diagnose`）。
- 新装技能需在**新会话**生效；当前会话的 skill 清单是启动时快照。
- 若不希望某技能被自动触发，在对应 `SKILL.md` 的 frontmatter 加：

```yaml
disable-model-invocation: true
```

该技能会从系统提示词清单中隐藏，仍可通过 `/skill:<name>` 手动调用。

## 相对 axiom 原文的适配

- `inspect_subagent` / `examine_subagent` / `review_subagent` → OMP `task`（`agent: reviewer`）。
- `<available_docs>` 索引 → 直接读取 `AGENTS.md`、`.docs/`、`.specs/`。
- `finish`：移除「系统提示词 Git 分支规则」引用与默认 commit/push；提交改为仅在用户明确要求时执行，推送单独确认。
- 移除 Axiom 专属的 `test-authoring` / `tests/verify.md` 依赖。
- 「宿主审批约定」→ OMP 审批流程。

## 约定

这些技能默认使用 `.specs/`（Spec）与 `.plans/`（Plan）目录。若项目不使用该约定，可忽略对应步骤，技能不强制创建文件。

## 来源

技能正文源自 [axiom](https://github.com/amuluze/axiom) 的内置 SDD 技能（`BUILTIN_SKILL_BODIES_VERSION` 43），按 OMP 18.16.0 适配。`greeting` 来自 axiom 项目级技能示例。
