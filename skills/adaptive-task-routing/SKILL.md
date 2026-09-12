---
name: adaptive-task-routing
description: Primary routing entrypoint for substantial multi-step coding, debugging, architecture, validation, research, analysis, audits, and scans. Use after a requested analysis or plan identifies actionable next work, or before executing a substantial phase, when both conversation context and model/reasoning should be assessed. Prefer this coordinator over either child router for general tasks. Skip brief explanations, status checks, tiny edits, and plugin-only questions.
---

# Adaptive Task Routing

Coordinate two independent decision skills. Do not choose a context, model, or reasoning effort yourself. Host-native startup context or prompt hooks may remind the model to invoke this entrypoint, but all eligibility and routing decisions remain in this Skill. Mentioning the plugin is not evidence that this Skill ran.

Read [references/zh-TW.md](references/zh-TW.md) when Chinese guidance is needed.

## Gate and controls

Route a substantial next phase only after its plan or preceding analysis is ready and before that phase begins. Substantial multi-step analysis, inspection, audits, scans, research, and planning qualify even when the user requested findings only and did not authorize implementation. Complete that authorized deliverable first; when its findings identify actionable changes, validation, or follow-on research, treat those actions as a concrete substantial next phase and append routing advice. A cross-file release-flow, cross-platform consistency, or test-gap scan is not a merely informational query. When the user already authorized execution, first prepare a concise actionable plan, using bounded non-mutating discovery when needed, then route before mutation or other substantial execution. Do not invent extra work after a complete answer with no concrete follow-on phase. Reuse a gate already completed for the same phase, context, catalog, preferences, and capabilities.

Read [shared policy](../../shared/runtime-routing-policy.md) and [defaults](../../shared/defaults.yaml), except when the generated Gemini dependency appendix is present: that appendix is the self-contained runtime contract and no external shared read or sibling activation is needed. Outside that generated package, resolve these paths from the directory containing this `SKILL.md`: the plugin root is three directories above this file, and shared resources are under `<plugin-root>/shared`, never `<plugin-root>/skills/shared`. Resolve the two user modes independently: `off`, `ask`, or `auto`. There is no third coordinator mode or separate fixed-routing policy. If both are `off`, skip routing, capability probing, and routing output. For one disabled router, skip its decision and continue with the other; the context stays current when context routing is off.

## Sequence

1. Establish the plan that the routing decision will govern. For a plan-only or analysis-only request, finish and present the requested findings and plan before the routing note. For an execution request, present a concise actionable plan first, but do not begin mutation or substantial execution.
2. For an enabled context gate, load and follow [task-context-router](../task-context-router/SKILL.md) as a coordinator-delegated call. The generated Gemini package appends a dependency appendix to this file because one Skill activation grants access only to that Skill directory; when that appendix is present, use its complete context-router instructions directly and do not attempt a second Skill activation. Otherwise read the packaged child file or use the host-provided Skill resource. Supply the fact that the full coordinator is active so the child's direct-selection guard does not dispatch back. It alone owns `CURRENT`, `HANDOFF`, `CLEAN`, and any handoff. A later model-only phase transition can reuse the resolved context.
3. Resolve the effective working context under that router's mode. In `ask`, continue immediately when the recommendation is `CURRENT`; when a change is recommended, pause and ask whether the user wants it. If declined, retain the current context and continue. If accepted, perform callable and verifiable operations; for `user_only` parts, provide the necessary handoff and let the user resume naturally in the destination without requiring a confirmation word. If a destination is awaiting confirmation, combine recommendations only when its model options are known; otherwise report model routing as deferred pending destination confirmation, with currently observable model/effort or `unknown`. Recheck in the destination before execution.
4. For an enabled model gate, load and follow [research-model-router](../research-model-router/SKILL.md) as a coordinator-delegated call, explicitly supplying the resolved effective context and completed or proposed plan so its direct-selection guard does not dispatch back. When the generated Gemini dependency appendix is present, use its complete model-router, host, registry, and shared-policy instructions directly. It alone owns model/effort judgment. Load the child even if the host did not independently select it. If neither packaged content, generated appendix, nor a host Skill resource is accessible, report that component as unavailable rather than inventing its result. Never synthesize a child result from the coordinator description or memory.
5. Present the requested findings and plan before one compact routing note. Do not narrate Skill internals or repeat the plan inside the note. Start the note with a Markdown horizontal rule, then a localized level-three heading meaning `Adaptive Task Routing | Task resource guidance`, followed by one localized sentence explaining that the recommendations assess conversation, model, and reasoning resources for the next phase based on the preceding plan. Keep the product name `Adaptive Task Routing` unchanged. Put the context result next in its own localized conversation-setting block with a plain-language recommendation and an explicit yes/no answer for whether to switch windows. Keep `CURRENT`, `HANDOFF`, and `CLEAN` only in structured evidence; never show those English enum tokens in normal user-facing output. Then show the model router's minimum-sufficient setting, recommended setting, upgrade value, and actual next action. Omit unreadable current model/effort fields. An enabled model gate must not disappear just because both pairs match the current configuration. Explicitly report deferred or unavailable components. Do not label such a gate complete.
6. If model mode is `ask`, stop after the routing note and wait for the user's natural response, even when the current pair appears suitable. Do not require a fixed confirmation word. If model mode is `auto`, apply callable, authorized, verifiable changes and continue; when switching is unavailable, retain the current setting and continue as the model router specifies. A plan-only request never authorizes implementing the plan. Revalidate before execution after a manual switch, changed environment, or materially revised plan.

During an active coordinated run, child routers do not call this coordinator or each other. A child selected directly by the host may dispatch once to this coordinator under its direct-selection guard; the coordinator-delegated marker prevents recursion. Read only the children needed for the gate, and reuse an already loaded policy without repeating identical work. Keep gate state in the current session and persist only capability/catalog cache records allowed by the shared policy; do not create persistent activity logs.

Identify the effective host surface once and carry that evidence into both children. For OpenAI surfaces, never default to Codex App merely because the prompt does not name the interface. If host metadata or the user does not identify CLI versus App, the model router must use the bounded automatic surface check in the OpenAI host guide before choosing a probe scope.

When combining results, preserve the model router's task requirements, discovery limitation and evidence scope in structured evidence. In the compact user-facing note, never surface the probe, fallback/registry source, freshness, account/surface applicability, or unreadable current values unless the user asks for diagnostics. Show only the branded resource-guidance introduction, localized conversation-setting block, the two AI-setting blocks, and the actionable capability outcome. Unknown current settings do not erase its capability recommendation. Distinguish a completed provisional assessment from a missing child or unresolved destination; do not convert the latter into `CURRENT`. Do not trigger model discovery when only context routing is enabled, and do not duplicate a child's probe.

For Traditional Chinese, use this branded introduction and compact conversation format before the two AI-setting blocks:

```text
---

### Adaptive Task Routing｜任務資源建議

以下建議是根據上述計畫的下一階段，評估適合的對話環境、模型與推理設定。

【對話設定】
* 建議：留在目前對話
* 是否切換視窗：否
目前對話保留了完成下一階段所需的需求與證據，因此直接繼續。
```

Replace the values and explanation with the actual decision. Localize every label and description to the user's language. Render `CURRENT` as the local equivalent of “stay in this conversation,” `HANDOFF` as “switch to a new conversation with a concise handoff,” and `CLEAN` as “start a new conversation without the current context.” Do not append the enum token in parentheses. `HANDOFF` and `CLEAN` normally mean switching windows; `CURRENT` normally means staying, unless the host's concrete context operation requires a different presentation.

## Later phases and limitations

Revisit model routing when implementation becomes validation, a pilot expands, mechanical processing becomes interpretation, or difficult evidence synthesis begins. Revisit context routing only when there is also a genuine context boundary. Do not repeat a routing note on every tool call or unchanged follow-up.

Resolve every host operation separately. A desktop/web/mobile App may allow automatic context creation while keeping current-model or effort changes user-only; a CLI can also expose mixed capabilities. Act only through available, authorized operations and verify the outcome. An interactive command intended for the user is not agent capability. Shared settings and injected reminders are instructions, not proof that the host invoked the Skill; the packaged host integrations make the reminder deterministic where those integrations are supported.

## Generated Gemini dependency appendix
This build-generated appendix is authoritative for this invocation. It keeps the complete compact Gemini routing contract inside the one directory authorized by activating this Skill. Follow it as the coordinator-delegated result of both child routers. Do not output until both enabled decisions are complete.

### Embedded dependency: `shared/gemini-coordinator-runtime.md`

# Gemini coordinator runtime projection

This compact projection is appended both to the generated coordinator Skill and to the extension
startup context. It is the complete runtime contract for the coordinated gate. The startup context
applies it directly for automatic routing, so automatic behavior does not depend on Gemini CLI's
`activate_skill` executor. Do not activate sibling Skills and do not infer rules from memory.

## Sequence

1. Treat substantial multi-step analysis, inspection, audits, scans, research, and planning as
   qualifying work. Complete and present the requested findings or plan first. When that deliverable
   identifies actionable changes, validation, or follow-on research, those actions are the concrete
   substantial next phase even if implementation was not requested. A cross-file release-flow,
   cross-platform consistency, or test-gap scan is not merely informational. If execution is already
   requested, present a concise actionable plan first without starting mutation or substantial execution.
2. Assess conversation placement for the substantial next phase before model choice.
3. Assess minimum-sufficient and recommended Gemini model settings for that next phase. Both
   setting blocks must evaluate the same concrete next phase, not the analysis or planning work
   that has already finished.
4. Render the localized routing note after the requested plan or findings. Begin it with a Markdown
   horizontal rule, a localized level-three `Adaptive Task Routing` resource-guidance heading, and
   one sentence explaining that the following recommendations assess resources for the planned
   next phase.
5. In `ask`, end the note with the applicable model-control and hold paragraph defined below, then
   stop and wait for the user's natural response without requiring a fixed keyword. The note is incomplete if that final paragraph is omitted. In `auto`, apply any callable, authorized and
   verifiable setting, or retain the current setting when switching is unavailable, then continue
   authorized execution.

Both routers default to `ask`. Skip a router only when its mode is explicitly `off`. Reuse a
completed gate for an unchanged phase. Never claim a context or model change unless the host
operation was callable, authorized, performed, and verified.

## Conversation decision

- Stay in the current conversation when it is focused and contains useful requirements or evidence.
  A fresh one-prompt session is focused; task complexity alone is not a reason to switch.
- Switch with a concise handoff when relevant evidence exists but accumulated unrelated history,
  conflicting instructions, or context pressure makes continued work materially less reliable.
- Start clean only when carrying current content is harmful and no task-specific history is needed.

For Traditional Chinese, render exactly this structure with task-specific values and reason:

```text
---

### Adaptive Task Routing｜任務資源建議

以下建議是根據上述計畫的下一階段，評估適合的對話環境、模型與推理設定。

【對話設定】
* 建議：留在目前對話
* 是否切換視窗：否
目前對話保留了完成下一階段所需的需求與證據，因此直接繼續。
```

Never show `CURRENT`, `HANDOFF`, or `CLEAN` in ordinary output.

## Gemini model decision

The bundled Gemini CLI fallback aliases are `Auto`, `Pro`, `Flash`, and `Flash-Lite`. Their actual
backend versions and account availability are runtime-dependent. Use only a live observed option or
one of these aliases. Never output Gemini 1.5. Use `Reasoning: model default`, localized as
`Reasoning：使用模型預設` in Traditional Chinese, unless the current session exposes an exact
configurable `thinkingBudget` or `thinkingLevel`. Never invent Codex-style low, medium, or high
Reasoning values for Gemini.

Choose the minimum setting that can complete the phase reliably, then a recommended setting that
offers meaningful value:

- `Flash-Lite`: narrow extraction, classification, or simple mechanical checks.
- `Flash`: ordinary bounded coding, analysis, validation, and structured review.
- `Pro`: broad cross-file or cross-platform reasoning, architecture, difficult debugging, high-cost
  error review, evidence reconciliation, or final synthesis.
- `Auto`: use only when delegating model choice to Gemini CLI is itself the recommendation.

For substantial cross-platform release, CI, manifest, testing, or supply-chain analysis, use at
least `Flash` and normally recommend `Pro`. Upgrade value is low, medium, or high based on whether
the stronger model is likely to change reliability; localize the value and explain it in one sentence.

For Traditional Chinese, render both blocks exactly in this order:

```text
【最低足夠 AI 設定】
* Model：Flash
* Reasoning：使用模型預設
<one task-specific sentence>

【建議 AI 設定】
* Model：Pro
* Reasoning：使用模型預設
* 升級價值：中。<one task-specific sentence>
```

Omit unreadable current settings, diagnostics, confidence, registry details, and internal schema.

## Model action

Gemini CLI does not expose an agent-callable, verifiable operation for changing the current model
through this Skill. Whenever the recommended model may differ from the current model or the current
model is unreadable, present `/model` as the user control. In Traditional Chinese `ask` mode, the
routing note must end with:

```text
目前環境無法代為切換模型；Reasoning 使用模型預設。如需採用建議，可用 /model 選擇模型；我先停在這裡，等你決定是否調整，或沿用目前設定開始下一階段。
```

Stop after the note in `ask`; the user may respond naturally with a changed setting or a request to
continue with the current one. In `auto`, show `/model` only as an optional control, retain the
current setting and continue authorized work. In another user language, translate the same action
and keep `/model` unchanged.
