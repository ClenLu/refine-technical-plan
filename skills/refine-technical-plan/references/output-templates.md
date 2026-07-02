# Output Templates

Use these templates as response shapes. Keep all user-facing content in the user's language unless the user requests another language. Localize headings, field names, decision labels, severity labels, confidence labels, and review-mode names. Do not copy English labels into non-English output except for code identifiers, commands, file paths, API/model/product names, exact external terms, or machine-oriented handoff labels that must remain exact.

Select the lightest template that satisfies the request.

The tables and blocks here are copy shapes only. The authoritative semantics and pass/fail rules for each shared structure (gate matrix, review coverage, freshness gate, repository ledger, diff-to-plan mapping, continuation packet, and so on) live in their owning reference; see the Single Source of Truth map in `SKILL.md`. When a rule or column changes, edit the owner, not these copies. This file owns only the plain-language verdict and localized decision-label guidance below.

## Plain-Language Verdict (Non-Expert Lead)

Every formal review template must begin with this block. It is written for a user who cannot judge domain detail. Keep it to a few short lines, in the user's language, with no unexplained jargon. Place gate tables, coverage tables, ledgers, and stress tests after it as supporting evidence, never before it.

```markdown
**结论**
- 能不能开始做：能 / 能，但要把提示项记下来 / 现在只能做可回退的验证 / 还不行
- 现在可以安全做：<一句话>
- 现在先别做：<一句话>
- 最大的风险：<一句平实的话>
- 需要先确认/核对的一件事：<谁来确认，或去查什么；没有就写"无">
- 决策：<通过 / 通过（带提示）/ 有条件通过 / 需修改 / 阻塞>（<一句话说明含义>）
- 打磨后评分：<0-100 或不评分>（信心：低 / 中 / 高；有上一轮时写"打磨前 <0-100> → 打磨后 <0-100>"）；一句话说明分数被什么拉低，或说明为什么本轮不评分
```

The score is the Implementation Readiness Score defined in `references/quality-gates.md`; do not invent a second scale. It is required for implementation-readiness, pass-like, revise/blocked, multi-round, or user-requested scoring decisions. Localize confidence labels and "not scored" text. For tiny discovery or evidence-collection replies where scoring would add false precision, use a localized "not scored" value and say why. A plan with an unresolved blocker or major issue cannot be scored implementation-ready no matter how high the number looks. Show the before→after delta only when a prior-round score exists; otherwise show the current score alone.

## 下一轮打磨计划

Use this block when the decision is `Revise`, `Blocked`, `Pass under assumptions`, or the user asks how to continue refining. Keep it compact and concrete. It must come from the current findings, pass conditions, evidence gaps, or refined plan; do not give generic process advice.

```markdown
**下一轮打磨计划**
- 优先修改方向：<1-3 个最高杠杆改动，例如补 caller inventory、收窄 rollout、改 rollback、拆迁移步骤、找 owner 确认>
- 需要补的证据：<具体 artifact、命令、owner 确认、dry-run、测试、截图、日志；没有就写"无">
- 下一轮审查模式：仅审查 / 重写方案 / 增量复审 / 证据收集 / 验证证据审查 / 实现一致性审查
```

### Localized Decision Labels

Use localized labels in user-facing output. For Chinese output, use these labels:

- 通过：可以按方案开始实现，保留好要求的验证证据。
- 通过（带提示）：可以开始，但提示项必须记录跟踪或转成明确接受的风险。
- 有条件通过：现在只能做调研、试点、可回退验证，或找负责人确认；不能上线，不能做不可逆改动。
- 需修改：先按要求改方案或做降风险工作，不要照原方案全量实现。
- 阻塞：缺关键信息，需要先收集证据或找负责人确认，不能靠猜继续。

Keep canonical English labels for internal continuity only. In non-English user-facing output, do not make the canonical label the visible decision.

## Detail Levels

### Tiny

Use for low-risk, docs-only, local, reversible work.

Include only:

- plain-language verdict (always first);
- decision;
- material findings;
- required changes or final plan;
- next action.

### Standard

Use for normal technical plans with moderate risk.

Include:

- plain-language verdict (always first);
- decision;
- non-expert control summary when relevant;
- context and evidence summary;
- material findings;
- gate summary;
- final plan or required changes;
- next action.

### Full

Use for high-risk, unfamiliar-domain, repository-backed, data-changing, security-sensitive, public-contract, production-impacting, AI/agent, or implementation-conformance reviews.

Include (plain-language verdict first, heavy tables below as supporting evidence):

- plain-language verdict (always first);
- decision and allowed next action;
- non-expert control summary;
- repository inspection ledger when material;
- review rounds or regression review;
- severity table with stable issue IDs;
- evidence collection plan when needed;
- gate evidence matrix;
- final plan or required changes;
- adversarial review and pre-pass stress test;
- falsification / kill-shot for high-risk plans (see `references/falsification-gate.md`);
- implementation readiness;
- implementation conformance requirement;
- freshness / external authority check when material;
- review coverage and biggest blind spot;
- owner / SME escalation status when triggers apply;
- remaining risks and next step;
- next review plan when further refinement is needed;
- continuation packet when the decision is `Revise`, `Blocked`, `Pass under assumptions`, or stops with open evidence.

## Automatic Multi-Round Review

```markdown
<先使用上方的结论块。>

**决策摘要**
- 决策：通过 / 有条件通过 / 通过（带提示） / 需修改 / 阻塞
- 是否可以开始实现：可以 / 不可以 / 只能做调研或验证 / 只能在完成列出的验证或负责人确认后开始
- 允许的下一步：...
- 暂时禁止：...
- 理由：...
- 当前最高风险：...
- 如果决策是“有条件通过”：这不是完整实现许可；现在只允许调研、试点、验证、负责人确认或可回退的小范围工作。

**非专业用户控制摘要**
- 最需要盯住的 3 件事：...
- 不能接受的红线：...
- 可以接受但必须有边界的取舍：...
- 需要代码、生产、业务、负责人、安全、隐私、合规或外部伙伴确认的问题：...
- 需要升级给负责人或专家的触发条件：...
- 后续实现必须保留的证据：...

**上下文与证据**
- 审查对象：...
- 变更类型：...
- 影响面：...
- 实现状态：...
- 已使用证据：...
- 假设或未知项：...
- 外部技术依赖：...
- 影响范围：低 / 中 / 高
- 可回退性：可回退 / 部分可回退 / 不可回退

**外部依据与时效性检查**
有实质影响时使用。
| 外部事实或依赖 | 需要或已使用的来源 | 状态 | 对决策的影响 |
| --- | --- | --- | --- |
|  | 官方当前依据 / 官方版本依据 / 仓库锁定版本 / 负责人确认 / 假设支撑 / 过期或未验证 |  |  |

**负责人或专家确认**
触发升级条件时使用。
- 必需确认方：无 / 领域负责人 / 运维负责人 / 安全 / 隐私 / 合规 / 产品 / 数据 / 计费 / 外部伙伴 / 其他
- 为什么需要：...
- 当前状态：已确认 / 缺失 / 不适用
- 对决策的影响：...

**审查覆盖度**
| 覆盖领域 | 覆盖程度 | 证据或缺口 | 对决策的影响 |
| --- | --- | --- | --- |
| 仓库覆盖 | 高 / 中 / 低 / 不适用 |  |  |
| 领域覆盖 | 高 / 中 / 低 |  |  |
| 外部依据与时效性覆盖 | 高 / 中 / 低 / 不适用 |  |  |
| 验证覆盖 | 高 / 中 / 低 / 无 |  |  |
| 负责人或生产确认覆盖 | 已有 / 部分 / 缺失 / 不适用 |  |  |
| 实现一致性覆盖 | 高 / 中 / 低 / 不适用 |  |  |

- 最大盲点：...
- 最能提高信心的动作：...
- 结论依据类型：仓库支撑 / 验证支撑 / 负责人支撑 / 生产支撑 / 外部依据支撑 / 假设支撑

**仓库检查记录**
| 检查领域 | 路径 / 命令 / 搜索词 | 检查原因 | 发现 | 信心 |
| --- | --- | --- | --- | --- |

**审查轮次**
- 第 1 轮：<发现摘要> -> <已做调整>
- 第 2 轮：<发现摘要> -> <已做调整>
- 第 3 轮：<最终门禁结果>

**问题分级**
| 编号 | 严重程度 | 发现 | 证据或假设 | 必需修改 | 状态 |
| --- | --- | --- | --- | --- | --- |
| B-001 / M-001 / A-001 / E-001 / R-001 | 阻塞 / 主要 / 次要 / 已接受风险 |  |  |  | 未解决 / 已解决 / 已接受 |

**证据收集计划**
| 缺失证据 | 为什么重要 | 如何收集 | 负责人或来源 | 对决策的影响 | 是否能开始实现 |
| --- | --- | --- | --- | --- | --- |

**分歧处理**
只有关键角色建议冲突时使用。
- 冲突建议：...
- 每个建议保护的风险或不变量：...
- 影响范围 / 可回退性 / 验证强度：...
- 选择的路径：...
- 拒绝的选项或接受的风险：...

**门禁证据矩阵**
| 门禁 | 证据 / 假设 / 验证 | 状态 |
| --- | --- | --- |
| 范围 |  | 通过 / 需修改 / 不适用 |
| 边界 |  | 通过 / 需修改 / 不适用 |
| 契约 |  | 通过 / 需修改 / 不适用 |
| 状态或迁移 |  | 通过 / 需修改 / 不适用 |
| 失败行为 |  | 通过 / 需修改 / 不适用 |
| 可观测性 |  | 通过 / 需修改 / 不适用 |
| 验证 |  | 通过 / 需修改 / 不适用 |
| 发布与回滚 |  | 通过 / 需修改 / 不适用 |
| 安全与隐私 |  | 通过 / 需修改 / 不适用 |
| 外部依据与时效性 |  | 通过 / 需修改 / 不适用 |
| 负责人或专家升级 |  | 通过 / 需修改 / 不适用 |
| 审查覆盖 |  | 通过 / 需修改 / 不适用 |
| 反证检查（高风险） |  | 已证伪 / 已存活 / 当前不可测试 / 不适用 |
| 技术债控制 |  | 通过 / 需修改 / 不适用 |

**最终方案**
<打磨后的方案或必需修改方向>

**对抗审查与通过前压力测试**
- 最可能失败的场景：...
- 代价最高或用户最明显感知的失败：...
- 最难发现的技术债失败：...
- 会推翻方案的假设：...
- 可能误导人的测试通过风险：...

**反证检查**
高风险且接近通过时使用。规则来源：`references/falsification-gate.md`。
- 预先登记的致命失败：...
- 发生机制：...
- 可观察信号：...
- 可证伪预测：如果运行 <探针>，会看到 <证明失败真实存在的结果>
- 最便宜的反证探针：仓库 / 数据 / 行为 / 独立子任务
- 实际尝试：<具体命令、查询、测试或子任务>
- 原始结果：...
- 结论：已证伪 / 已存活（有实证） / 当前不可测试
- 独立性：独立子任务 / 自己运行（存在相关性失败风险）
- 对决策的影响：...

**实现准备度**
- 评分：<0-100>
- 信心：低 / 中 / 高
- 理由：...

**实现一致性要求**
- 实现后是否需要一致性审查：是 / 否
- 实现时必须保留的证据：...

**剩余风险**
- ...

**通过条件或下一步**
- ...

**下一轮打磨计划**
决策有条件、阻塞、需修改，或用户想继续打磨时使用。
- 优先修改方向：...
- 需要补的证据：...
- 下一轮审查模式：...

**继续交接包**
决策有条件、阻塞、需修改，或仍有证据缺口时，使用 `SKILL.md` 中的规范字段；如果本轮评分适用，保留打磨后评分。
```

Remove empty sections when they are not relevant. For non-English output, localize section headings and option values. Do not include a repository inspection ledger if no repository context exists; instead state in the user's language that the decision is not repository-backed.

## Single-Round Review

```markdown
**决策**
需修改 / 有条件通过 / 通过（带提示） / 通过 / 阻塞

**理由**
...

**专家视角审查**
- 架构：...
- 领域：...
- 实现：...
- 可靠性与安全：...
- 产品与运维：...

**必需修改**
- [阻塞/主要] <发现>（证据/假设：...）

**建议改进**
- [次要] ...

**通过条件**
- ...
```

## Optimized Plan

```markdown
**目标**
...

**非目标**
...

**证据、假设与未知项**
...

**目标设计**
...

**实现阶段**
1. ...
2. ...

**验证与验收**
...

**发布、回滚与清理**
...

**风险与已接受取舍**
...

**实现一致性要求**
...
```

## Evidence Collection Mode

```markdown
**决策**
阻塞 / 需修改 / 有条件通过

**为什么需要证据**
...

**证据收集计划**
| 缺失证据 | 为什么重要 | 如何收集 | 负责人或来源 | 对决策的影响 | 是否能开始实现 |
| --- | --- | --- | --- | --- | --- |

**证据收集前允许做的事**
- ...

**证据收集前禁止做的事**
- ...

**什么会提高决策等级**
- ...
- 需要当前官方依据、锁定版本证据、负责人确认、验证证据或生产观察，才能从假设支撑升级到接近通过。

**继续交接包**
使用 `SKILL.md` 中的规范字段，重点保留未解决证据缺口和证据收集前禁止做的事。
```

## Validation Evidence Review

```markdown
**验证结论**
验证支撑通过 / 仍由假设支撑 / 需修改 / 阻塞

**证据审查**
| 证据产物 | 验证了什么主张 | 没有验证什么 | 结果 | 对决策的影响 |
| --- | --- | --- | --- | --- |

**剩余缺口**
- ...

**更新后的门禁结论**
- ...
```

## Implementation Conformance Review

Use `references/implementation-conformance.md` for the full template.

```markdown
**实现一致性结论**
- 决策：实现通过 / 实现有条件通过 / 需修改实现 / 实现阻塞
- 是否允许合并或发布：是 / 否 / 只能在完成验证后
- 理由：...

**变更与方案映射**
| 变更区域或文件 | 已批准方案章节或门禁 | 是否匹配 | 证据 | 备注 |
| --- | --- | --- | --- | --- |

**未计划变更**
| 变更 | 风险 | 严重程度 | 必需动作 |
| --- | --- | --- | --- |

**验证证据检查**
| 必需证据 | 实际产物 | 验证的主张 | 结果 | 剩余缺口 |
| --- | --- | --- | --- | --- |

**发布、回滚与清理检查**
- 发布路径是否保持：...
- 回滚是否仍可信：...
- 是否有可观测性：...
- 是否有清理触发条件：...
```


## 审查覆盖度与继续交接包

Use this compact add-on for conditional or multi-round reviews.

```markdown
**审查覆盖度**
- 仓库覆盖：高 / 中 / 低 / 不适用 — ...
- 领域覆盖：高 / 中 / 低 — ...
- 外部依据与时效性覆盖：高 / 中 / 低 / 不适用 — ...
- 验证覆盖：高 / 中 / 低 / 无 — ...
- 负责人或生产确认覆盖：已具备 / 部分 / 缺失 / 不适用 — ...
- 实现一致性覆盖：高 / 中 / 低 / 不适用 — ...
- 最大盲点：...
- 最能提高信心的动作：...

**继续交接包**
使用 `SKILL.md` 中的规范字段，保留未解决阻塞项、主要问题、假设、证据缺口、负责人确认、下一轮审查模式，以及适用时的打磨后评分。
```

## Document Update Response

After updating a document, respond with:

```text
已更新：<path>
决策：通过 / 有条件通过 / 通过（带提示） / 需修改 / 阻塞
主要变更：
- <change>
剩余风险：
- <风险，或写"无阻塞风险">
下一步：
- <next step>
```
