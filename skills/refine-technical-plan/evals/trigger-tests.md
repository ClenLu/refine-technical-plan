# Trigger Tests

Use these prompts to check skill activation behavior after editing the description or `When to Use` section.

## Should trigger

- `帮我打磨这个技术方案，直到可以安全实现。`
- `Review this backend migration plan and tell me if implementation can start.`
- `方案已经更新，检查之前的 blocker 是否解决。`
- `Here is an approved plan and the PR diff; run conformance review.`
- `Convert this PRD into an implementation-ready engineering plan.`
- `Check this agent-generated proposal for hidden assumptions and rollback gaps.`
- `We are in a repo. Review and rewrite this architecture plan based on current code.`
- `这个方案依赖第三方 SDK 当前行为，帮我确认能不能安全实现。`
- `方案还没过，请输出 continuation packet 给下一轮 agent。`
- `这是高风险权限方案，检查是否需要真实 security owner 确认。`

## Should not trigger by default

- `Explain what OAuth is.`
- `Fix this TypeError in my Python script.`
- `Brainstorm five startup ideas.`
- `Write the React component now; no plan review needed.`
- `Translate this paragraph to English.`

## Conditional trigger

- `Can you help with this architecture?` should trigger only if the user wants plan review, hardening, or implementation readiness.
- `What do you think of this idea?` should trigger only if the idea is a technical implementation proposal or the user asks for risk review.
