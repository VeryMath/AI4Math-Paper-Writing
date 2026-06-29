# Paper Writing

English guide: [README.md](README.md)

`paper-writing` 是面向数学论文写作的 AI4Math Skill adapter。它让 coding
agent 基于已经提供的笔记、证明材料、实验日志、参考文献和审稿意见，完成论文
段落起草、修改和证据审查。
它也覆盖早期论文骨架和最终提交就绪检查，但不把不受支持表述审查拆成单独公开 Skill。

这个 package 只保留一个公开的论文写作 workflow。对不受支持表述的检查属于写作
审查步骤，不单独拆成另一个公开 Skill。

## 什么时候用

适合用于：

- 起草或修改摘要、引言、相关工作、定理解释、实验叙述、结论或回复审稿人；
- 把论文精读结果、证明草稿、实验日志或项目笔记整理成论文文本；
- 在写正文前搭建 rapid paper skeleton、contribution spine、section map 或
  result dependency map；
- 检查草稿中是否有缺引用、缺证据、夸大贡献或条件表述不清的问题。

不适合用于凭空生成引用、定理、实验结果、数值、证明状态或作者观点。

## 输出什么

agent 应输出目标论文文本或论文骨架，并附上 claim-evidence 说明、proof
obligation notes、不受支持的表述、需要补引用的位置，以及下一步需要核查的来源。

## Skill 入口

| 文件 | 作用 |
| --- | --- |
| `SKILL.md` | 顶层论文写作 workflow |
| `skills/registry.yaml` | 嵌套 subskill 路由 |
| `skills/paper-skeleton-and-logical-architecture/SKILL.md` | rapid paper skeleton、section map 和 result dependency 审查 |
| `skills/claim-evidence-ledger/SKILL.md` | claim 到来源证据的审查 |
| `skills/proof-obligation-and-assumption-audit/SKILL.md` | 定理假设、证明义务和外部结果适配审查 |
| `skills/notation-and-variable-consistency/SKILL.md` | 符号、变量、作用域和记号一致性审查 |
| `skills/formula-environment-and-readability/SKILL.md` | 展示公式、推导和公式环境可读性 |
| `skills/latex-build-and-layout-audit/SKILL.md` | LaTeX 编译、引用、标签、排版和提交风险审查 |
| `templates/` | source packet、论文骨架、ledger 和提交就绪模板 |
| `README.md` | 英文说明 |
| `README.zh-CN.md` | 中文说明 |
| `agents/openai.yaml` | OpenAI/Codex skill 元数据 |

## 快速开始

```text
Use this repository's paper-writing workflow.

Read:
- SKILL.md
- 如果 source packet 还没整理，先读 templates/source-packet.md
- 如果任务需要骨架、证明、claim、符号、公式或 LaTeX 检查，再读 skills/registry.yaml

Goal:
帮我写这篇数学论文的 introduction。

Target:
<笔记、定理陈述、相关论文、实验日志或草稿文件>

Constraints:
- 先检查 source packet；
- 不要编造引用或结果；
- 明确标记缺证据的表述；
- 返回正文草稿和 claim-evidence notes。
```

## 协作方式

```text
human goal
  -> agent 读取 SKILL.md
  -> agent 按需加载 focused subskill
  -> agent 清点来源材料
  -> 如果论文结构还不稳定，agent 先搭建骨架
  -> agent 提出写作计划
  -> human 回复 approve / revise / reject / skip
  -> agent 起草或修改
  -> agent 汇报证据缺口和下一步核查
```

常用 checkpoint：

| 决策词 | 含义 |
| --- | --- |
| `approve` | 执行 agent 提出的下一步 |
| `revise` | 先修改计划 |
| `reject` | 停止当前路线 |
| `skip` | 跳过当前阶段 |
| `stop` | 结束会话并总结状态 |

## 维护检查

在整理仓库根目录运行：

```bash
python3 -m unittest discover -s tests -v
python3 scripts/validate_skill_repo.py
```
