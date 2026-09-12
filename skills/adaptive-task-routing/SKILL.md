---
name: adaptive-task-routing
description: Primary routing entrypoint for substantial multi-step coding, debugging, architecture, validation, research, analysis, audits, and scans, and for conversational commands that inspect or change both routing modes. Use after a requested analysis or plan identifies actionable next work, or before executing a substantial phase, when both conversation context and model/reasoning should be assessed. Prefer this coordinator over either child router for general tasks. Skip brief explanations, status checks, tiny edits, and ordinary plugin-only questions.
---

# Adaptive Task Routing

Coordinate two independent decision skills. Do not choose a context, model, or reasoning effort yourself. Host-native startup context or prompt hooks may remind the model to invoke this entrypoint, but all eligibility and routing decisions remain in this Skill. Mentioning the plugin is not evidence that this Skill ran.

Read [references/zh-TW.md](references/zh-TW.md) when Chinese guidance is needed.

## Gate and controls

Before testing gate eligibility, handle direct mode-inspection or mode-change commands under the shared policy. A user can switch `ask`, `auto`, or `off` in normal conversation. A command naming Context or Model changes only that child; an unqualified Adaptive Task Routing mode command changes both independent modes. Confirm the two effective values and scope concisely without emitting a routing note. If the same turn also requests substantial work, apply the new mode before routing that work. A mode change by itself does not authorize the work discussed earlier.

Route a substantial next phase only after its plan or preceding analysis is ready and before that phase begins. Substantial multi-step analysis, inspection, audits, scans, research, and planning qualify even when the user requested findings only and did not authorize implementation. Complete that authorized deliverable first; when its findings identify actionable changes, validation, or follow-on research, treat those actions as a concrete substantial next phase and append routing advice. A cross-file release-flow, cross-platform consistency, or test-gap scan is not a merely informational query. When the user already authorized execution, first prepare a concise actionable plan, using bounded non-mutating discovery when needed, then route before mutation or other substantial execution. Do not invent extra work after a complete answer with no concrete follow-on phase. Reuse a gate already completed for the same phase, context, catalog, preferences, and capabilities.

Read [shared policy](../../shared/runtime-routing-policy.md) and [defaults](../../shared/defaults.yaml), except when the generated Gemini dependency appendix is present: that appendix is the self-contained runtime contract and no external shared read or sibling activation is needed. Outside that generated package, resolve these paths from the directory containing this `SKILL.md`: the plugin root is three directories above this file, and shared resources are under `<plugin-root>/shared`, never `<plugin-root>/skills/shared`. Resolve the two user modes independently: `off`, `ask`, or `auto`. There is no third coordinator mode or separate fixed-routing policy. If both are `off`, skip routing, capability probing, and routing output. For one disabled router, skip its decision and continue with the other; the context stays current when context routing is off.

## Sequence

1. Establish the plan that the routing decision will govern. For a plan-only or analysis-only request, finish and present the requested findings and plan before the routing note. For an execution request, present a concise actionable plan first, but do not begin mutation or substantial execution.
2. For an enabled context gate, load and follow [task-context-router](../task-context-router/SKILL.md) as a coordinator-delegated call. The generated Gemini package appends a dependency appendix to this file because one Skill activation grants access only to that Skill directory; when that appendix is present, use its complete context-router instructions directly and do not attempt a second Skill activation. Otherwise read the packaged child file or use the host-provided Skill resource. Supply the fact that the full coordinator is active so the child's direct-selection guard does not dispatch back. It alone owns `CURRENT`, `HANDOFF`, `CLEAN`, and any handoff. A later model-only phase transition can reuse the resolved context.
3. Resolve the effective working context under that router's mode. In `ask`, continue immediately when the recommendation is `CURRENT`; when a change is recommended, pause and ask whether the user wants it. If declined, retain the current context and continue. If accepted, perform callable and verifiable operations; for `user_only` parts, provide the necessary handoff and let the user resume naturally in the destination without requiring a confirmation word. If a destination is awaiting confirmation, combine recommendations only when its model options are known; otherwise report model routing as deferred pending destination confirmation, with currently observable model/effort or `unknown`. Recheck in the destination before execution.
4. For an enabled model gate, load and follow [research-model-router](../research-model-router/SKILL.md) as a coordinator-delegated call, explicitly supplying the resolved effective context and completed or proposed plan so its direct-selection guard does not dispatch back. When the generated Gemini dependency appendix is present, use its complete model-router, host, registry, and shared-policy instructions directly. It alone owns model/effort judgment. Load the child even if the host did not independently select it. If neither packaged content, generated appendix, nor a host Skill resource is accessible, report that component as unavailable rather than inventing its result. Never synthesize a child result from the coordinator description or memory.
5. Present the requested findings and plan before one compact routing note. Read and follow [routing UX](../../shared/routing-ux.md), or its embedded Gemini copy. Start the note with a divider and plain `Adaptive Task Routing` heading, then lead with the combined action and reason. Keep one enabled conversation sentence visible before useful AI settings. Both routers still compute their own results; presentation does not select settings or hide unavailable components.
6. In `ask`, pause only before a proposed environment change or for a material blocker. Retain and nonblocking defer continue already authorized work without a routing confirmation. A plan-only request never authorizes implementing the plan. In `auto`, apply only justified, authorized, callable and verifiable changes; use the actual fallback and stop if a material blocker remains. Do not require a fixed confirmation word. Revalidate after manual changes. Context and Model modes remain independent; a retained model never overrides a pending context decision.

During an active coordinated run, child routers do not call this coordinator or each other. A child selected directly by the host may dispatch once to this coordinator under its direct-selection guard; the coordinator-delegated marker prevents recursion. Read only the children needed for the gate, and reuse an already loaded policy without repeating identical work. Keep gate state in the current session and persist only capability/catalog cache records allowed by the shared policy; do not create persistent activity logs.

Identify the effective host surface once and carry that evidence into both children. For OpenAI surfaces, never default to Codex App merely because the prompt does not name the interface. If host metadata or the user does not identify CLI versus App, the model router must use the bounded automatic surface check in the OpenAI host guide before choosing a probe scope.

When combining results, preserve task requirements, discovery limits, both task-based settings and the separate switch assessment in structured evidence. Never show those English enum tokens or source/probe diagnostics in ordinary output. Use `Task-fit setting` / `任務適配設定` for the internal `recommended_setting`; it is not an instruction to switch. Unknown current settings require provisional language, not a claim that the setup is suitable. Context-off and model-only do not imply an assessed conversation.

For Traditional Chinese the shared UX contract uses `### Adaptive Task Routing`, a localized action and reason, then a plain conversation sentence such as `對話：留在目前對話，不需開新對話。`. Localize every label and description to the user's language. Compact retains that line even when no model change is advised. Detailed adds minimum needed, task-fit setting and upgrade rationale without rerunning an unchanged gate. No third routing mode is introduced.

## Later phases and limitations

Pass the effective context decision and continuity rationale, including handoff/setup cost, to the model router. A declined handoff uses the current conversation; context-off passes current placement with no suitability claim. Route at task boundaries, not every prompt. Do not infer cache locality from the context enum. Preserve the model router's separate `switch_assessment` as well as both task-based settings: its `upgrade_value` is not the value of changing from the current setup. Express its practical consequence in the action-first reason; keep switch scoring internal under the UX contract. In `auto`, a router-initiated model/effort change additionally requires `decision: change`; retain or defer keeps the configuration while authorized work continues. User-selected settings take precedence. Reuse an unchanged gate; no persistent phase or activity log is needed.

Revisit model routing when implementation becomes validation, a pilot expands, mechanical processing becomes interpretation, or difficult evidence synthesis begins. Revisit context routing only when there is also a genuine context boundary. Do not repeat a routing note on every tool call or unchanged follow-up.

Resolve every host operation separately. A desktop/web/mobile App may allow automatic context creation while keeping current-model or effort changes user-only; a CLI can also expose mixed capabilities. Act only through available, authorized operations and verify the outcome. An interactive command intended for the user is not agent capability. Shared settings and injected reminders are instructions, not proof that the host invoked the Skill; the packaged host integrations make the reminder deterministic where those integrations are supported.

## Compact presentation invariants

Use the exact heading `### Adaptive Task Routing` without a subtitle. Verified model keep shows
only the observed current pair in compact; reserve task-fit alternatives for detailed output.
Context advice is one sentence stating whether to open a new conversation, not a window field.
For analysis/plan-only output, the complete findings and plan precede the divider; the routing
note is the final section. Action-first applies inside the note. Use the shared canonical action
line unchanged, with a separate short reason; any proposed implementation remains conditional.
An ordinary change asks only about the target setting. Provisional keep also covers uncertain
switch costs or benefits, not just an unknown current model. Follow the shared UX contract for
independent modes, pending context decisions and task authorization.

## Generated Gemini dependency appendix
This build-generated appendix is authoritative for this invocation. It keeps the complete compact Gemini routing contract inside the one directory authorized by activating this Skill. Follow it as the coordinator-delegated result of both child routers. Do not output until both enabled decisions are complete.

### Embedded dependency: `shared/gemini-coordinator-runtime.md`

# Gemini coordinator runtime projection

This projection and the shared routing UX contract are embedded in both the generated coordinator
Skill and extension startup context. Apply both directly; automatic routing does not depend on
`activate_skill`. Do not activate sibling Skills or request external shared files from this path.

## Sequence

Handle direct conversational mode commands before task classification. A named router changes only
that router; an unqualified mode command changes both. Both routers default to `ask`. Confirm scope
and values without running routing for a mode-only command. A mode change does not authorize work.
Use turn/conversation scope and persist defaults only through a host/user settings store.

1. Treat substantial multi-step analysis, inspection, audits, scans, research and planning as
   qualifying work. Complete and present the requested findings or plan before its next-phase note.
   For execution requests, present an actionable plan before mutation or substantial execution.
   When findings propose concrete changes, validation or follow-on research, route that next phase
   even if implementation was not requested. Do not invent extra work after a complete answer with
   no substantial next phase. Reuse an unchanged gate rather than routing every response.
   For analysis/plan-only output, finish the complete findings and plan before the divider;
   the routing note is the final section, never an introduction to “以下為改善計畫”.
   Action-first means the first line inside that note. Describe proposed implementation
   conditionally (“若後續進入實作”), without implying it is authorized.
2. Assess the conversation first when context routing is enabled. Stay when focused requirements
   or evidence remain useful. A fresh one-prompt session is focused; complexity alone does not
   justify a new conversation. Handoff preserves needed facts while dropping interfering history;
   clean starts avoid harmful task history. A handoff needs a concise summary, not a transcript.
   Context-off skips this decision and its visible conversation advice. Never infer a suitable context
   from its router being off. Defer unknown destination model choices until the destination is known.
3. If model routing is enabled, compute both task-based Gemini settings and the switch assessment
   below for the same concrete next phase. Use the effective context and its continuity rationale.
4. Follow the embedded shared UX contract: action first inside one routing note, enabled
   conversation advice, useful native AI settings and the practical next step. Minimum and
   upgrade value belong in detailed output; switch_value remains diagnostic. A provisional keep
   must not claim that an unreadable current model is known to be suitable.
5. In `ask`, pause before a proposed change or a material blocker, not every routing decision.
   Retain and nonblocking defer continue already authorized work without a routing confirmation.
   Only-plan requests end with their deliverable; do not implement the plan. In `auto`, apply any callable,
   authorized, justified and verifiable operation; otherwise report the actual fallback, continuing
   only when work is authorized and no quality or destination blocker remains. Both-off skips all routing.

## Gemini model decision

The bundled Gemini CLI fallback aliases are `Auto`, `Pro`, `Flash`, and `Flash-Lite`. Their actual
backend versions and account availability are runtime-dependent. Use only a live observed option or
one of these aliases. Never output Gemini 1.5. Use `Reasoning: model default`, localized as
`Reasoning：使用模型預設` in Traditional Chinese, unless the current session exposes an exact
configurable `thinkingBudget` or `thinkingLevel`. Never invent Codex-style low, medium, or high
Reasoning values for Gemini.

`Reasoning：使用模型預設` describes reasoning only; it is not a current model identity.
Do not render `目前 AI：使用模型預設`. Verified keep requires an observed current model,
its native reasoning configuration, and evidence of quality-floor adequacy and retention value.
An unresolved Auto backend, catalog availability or completed analysis cannot supply that evidence.
Without it, a retention result is provisional; preserve independent context changes and blockers.
Use the shared canonical action line verbatim, on its own line, then a short separate reason.

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

### Phase continuity and switching value

Route at task boundaries, not every prompt. Reuse the completed gate within an unchanged
phase; reassess after difficult work instead of automatically lowering settings. Optimize
total task cost and reliability over the remaining phase, including retries, rework,
latency, handoff/setup and user corrections; cheap-model turn share is not the objective.
Do not equate API prices with subscription usage or double-count cache processing costs.

Pass the effective context and continuity rationale into this decision. A declined
handoff uses the retained conversation; context-off or model-only uses current placement
without claiming a suitability assessment. Prefer model stickiness when the observed
pair meets the quality floor and continuity has value. A clear capability deficit or
failed validation outweighs cache preservation. A handoff or clean conversation permits
reassessment, but still has setup cost and does not require a different model.

Keep conversation continuity, prompt-cache reuse and switch capability separate. A cache
miss does not erase supplied history, and a retained conversation does not prove a hit.
Switching back may reuse a matching unexpired prefix. Provider, model, prefix, TTL,
tool/thinking compatibility and reasoning-only changes affect reuse under the host's
actual rules. Unknown cache evidence is not zero cost or certain cache loss. Use scoped
official rules or actual usage when available; do not start paid probes or warm caches.
Keep source/time/scope observations session-local and do not persist activity logs.

Keep both task-based settings in structured evidence even when retaining another suitable
configuration; the shared UX contract controls compact versus detailed visibility.
`upgrade_value` compares recommended versus minimum sufficient. Separately record
`switch_assessment` against the observed current pair for that same next phase, with
`switch_value: low | medium | high | unknown`, `decision: retain | change | defer`, and
a reason. The target is `recommended_setting`. Weigh capability/reliability and savings
over remaining work against switching cost and context disruption; use qualitative
judgment unless measured inputs support calculation. If gains do not meaningfully
exceed costs, retain. If the observed pair already matches, retain with low switch value.
Unknown current settings require unknown switch value and deferred automatic switching
while still giving both evidenced task settings. Unknown costs that could reverse the
decision require retention or deferral; a clear quality deficit may justify a change
despite unknown cache cost, with the tradeoff stated. A deferred switch assessment does
not make a completed task recommendation an unresolved destination gate.

### Select the action paragraph

For `retain` or `defer`, do not append `/model`, selectors or an invitation to apply the target.
Use the embedded UX contract to distinguish verified keep, provisional keep and a material blocker.
A nonblocking defer needs no routing confirmation. Unknown model metadata alone is not a blocker.
A proposed handoff still requires its independent context decision even when the model is retained.

Gemini CLI does not expose an agent-callable, verifiable current-model switch through this Skill.
Only for `decision: change` or an explicit user target, provide `/model` as the user control.
In `ask`, ask only whether to use the named target for an ordinary change, once. In `auto`, retain the actual setting and continue
only authorized work without material blockers. Never claim an application or a new conversation
unless that exact operation was performed and verified. Reasoning uses the model default unless
an exact native configurable thinking control is known. The shared UX examples are localized to
Traditional Chinese with `### Adaptive Task Routing` and one plain sentence explaining whether a new conversation is needed.

### Embedded dependency: `shared/routing-ux.md`

# Routing interaction and presentation

This is the shared UX contract for both routers and the coordinator. Apply it after their
independent assessments; it does not select models or authorize task execution. Gemini embeds
this contract beside its host-specific runtime projection. Other hosts read this file through
the shared policy. Keep `recommended_setting` as the compatible internal field name; label it
**Task-fit setting** / **任務適配設定** in the UI. This is an evidence-based task recommendation,
not a proven optimum or an instruction to switch now.

## Ask before a change, not after every decision

Resolve interaction separately from the model's `decision: retain | change | defer`:

| Situation | `ask` interaction | Next-phase execution |
| --- | --- | --- |
| Retain, no pending context change or material quality blocker | No routing confirmation | Continue already authorized work |
| Defer, uncertainty does not prevent responsible progress | Explain provisional retention; no routing confirmation | Continue already authorized work with appropriate validation |
| Defer, missing information materially blocks quality or the next action | Ask one concrete question explaining what the answer changes | Wait for the missing decision |
| Justified context/model/effort change | Ask before the proposed change unless already explicitly authorized | Wait for the change decision; after a manual change, revalidate |
| User only requested analysis or a plan | Deliver that work and the relevant advice | Do not start implementation, regardless of mode or switch decision |

Unknown current model metadata alone is not a quality blocker. Assess the next phase's concrete
needs, observed work quality and available validation. An unmet quality floor, unresolved
destination, or missing requirement that prevents responsible progress must not become silent
continuation. Do not ask “keep current?” after deciding to retain. A missing implementation
authorization means the requested deliverable is complete, not that routing needs confirmation.
Do not require a fixed confirmation word. An explicit user-selected setting or acceptance is
already authorization for that setting; do not ask for it again.

For an ordinary setting change, ask only whether to use the named target: “Use <model / effort>?”
or “要改用 <模型／推理設定> 嗎？” Do not also ask to revise the plan. Broader choices belong
only to a material blocker. After a decline, continue only if the current approach can meet the
quality requirement; promise extra validation only when a concrete feasible check addresses the
limitation. More checking is not a blanket remedy for an inadequate model.

`auto` still requires justified, authorized, callable and verifiable operations. A recommendation
is not an applied change. When an operation cannot be performed, explain the practical fallback;
continue in the effective context only if work is authorized and no material blocker remains.
Do not pretend a manual-only handoff occurred. If progress requires the destination, provide the
handoff and wait for the user to resume there. `off` skips that router entirely.

## Compose one action-first routing note

Present the user's requested findings or actionable plan first. Within the routing note, use:
Markdown divider → exact `### Adaptive Task Routing` heading → action → short reason
→ conversation advice → useful AI setting → necessary control or next step.
The heading has no localized subtitle. Localize the action and body instead.
For analysis/plan-only requests, the complete requested findings and plan precede the divider;
the routing note is the final section. Never place the plan after the note or end the note with
“以下為改善計畫”. Action-first applies inside the note, not to the whole response. For execution
requests, present the actionable plan, then the gate, then continue authorized execution only
when no decision remains. A routing-only request needs no invented findings or plan.
Keep the action on its own line, followed by a separate reason of one or two short sentences.
Localize labels using the canonical vocabulary below; keep raw enums and scores internal.

For plan-only work, acknowledge the authorized analysis/planning scope. If the gate evaluates
a proposed later implementation phase, say “若後續進入實作” / “If implementation is later
authorized”. Do not describe that phase as already authorized, or substitute an assessment of
completed analysis for the proposed next-phase assessment. Plan-only status alone is not
evidence that the current AI is suitable or that switching offers little benefit.

Select the action from the combined effective result, not just the model decision:

- **Keep current / ✓ 維持目前設定**: no proposed environment change; claim suitability only
  for enabled components with supporting evidence. Context-off cannot certify the conversation.
- **Keep provisionally / 暫時沿用設定**: uncertainty warrants retaining without certifying
  suitability. Explain the relevant uncertainty briefly, without dumping unreadable model fields.
- **New conversation with handoff / → 建議交接**: carry the objective, confirmed facts,
  constraints, decisions, relevant artifacts and next step. Omit failed hypotheses and secrets.
- **Start clean / ↻ 全新開始**: explain why prior task context would interfere; do not carry
  a task-history handoff. Supply only the new task's self-contained request when needed.
- **Change AI setting / ↑ 建議調整 AI 設定**: show the justified target and the next action. Show the
  current pair only when actually observed or supplied by the user.
- **Need your decision / ? 需要你的決定**: a material blocker remains; name it and ask the useful
  question. This is not a synonym for every deferred switch assessment.

### Verified retention evidence

When model routing is enabled, verified keep requires all three: a reliably observed current
model and native reasoning configuration, evidence that it meets the assessed phase's quality
floor, and evidence supporting retention over switching. Successful earlier analysis alone does
not establish suitability for a different next phase. A catalog, saved default or generic
“model default” label does not identify the running model. Native default reasoning is valid
when paired with a reliably observed model; it must never substitute for the model identity.
Keep evidence internal unless details are requested; merely printing a model name is not proof.

If a retention result lacks any of these prerequisites, use provisional keep and explain the
uncertainty without claiming “switching offers little benefit”. Omit unknown/default-only current
AI fields. A useful supported task-fit setting may still be shown. This rule does not replace
a pending context action, explicit user target or material blocker with provisional retention.
Model-off can give a verified context-only keep without probing or displaying any AI setting.

### Canonical action vocabulary

These are fixed action lines, not new routing states. Traditional Chinese uses exactly:

| Action | 繁體中文 |
| --- | --- |
| Verified keep | ✓ 維持目前設定 |
| Provisional keep | 暫時沿用設定 |
| Handoff | → 建議交接 |
| Clean | ↻ 全新開始 |
| Change | ↑ 建議調整 AI 設定 |
| Material blocker | ? 需要你的決定 |

Do not substitute synonyms such as “保留現況” or merge the action into its reason. All hosts
use the same action line for the same decision in the same language. In other languages use
the corresponding localized labels in the packaged examples; do not force Chinese on the user.

A handoff or clean start can also require a model change. Lead with the context action and
include the destination setting when known; otherwise say it will be assessed there. Combine
pending choices only when accurate for the same destination. Never hide an enabled context
result behind model retention. Compact includes one plain conversation sentence, not a separate
new-conversation field. Use “對話：留在目前對話，不需開新對話。” for staying;
“對話：建議開新對話並交接必要脈絡，待你確認。” for a proposed handoff; and
“對話：建議開啟全新對話，不帶入目前脈絡，待你確認。” for a proposed clean start.
Say the destination is awaiting a decision when unresolved. Translate these meanings for other
languages using conversation/session terms appropriate to the host, not desktop window controls.
Distinguish a proposal from a completed operation; “待你確認” applies only when confirmation is
pending, not after acceptance or a verified auto change. Omit the entire conversation assessment
for context-off or an explicit model-only request.

## Compact and detailed

`compact` is the default presentation; `detailed` is available on request. These are presentation
preferences, not extra routing modes. Apply turn/conversation scope like other preferences;
persist a default only through an available host/user settings store, never in the installed
package. Asking for details reuses the current gate and does not authorize work or repeat probing.

- Verified keep in compact shows only the observed current AI pair, never a task-fit alternative.
  Move task-fit settings and their comparison to detailed output. Keep the action, reason and
  enabled conversation sentence. Model-off never adds an AI setting, even for a context keep.
- Provisional keep can arise from unknown current settings or uncertain switching costs/benefits.
  It is not limited to missing model metadata. Show a supported task-fit pair when useful; a known
  current pair may also be shown without certifying suitability. If current settings are unknown,
  explain that briefly in prose instead of a `Current AI: Unknown` field. Do not promise to wait
  for metadata when other evidence could justify a later decision. An unavailable model component
  is reported, not silently omitted.
- Detailed adds the observed current pair when useful, **Minimum needed / 最低足夠設定**,
  **Task-fit setting / 任務適配設定**, upgrade value and concise comparison rationale. Both task
  settings are still computed and kept in structured evidence even when compact omits them.
- Keep `switch_value`, cache evidence, source/scope and other diagnostic fields internal unless
  diagnostics are explicitly requested. Detailed means more explanation, not hidden reasoning,
  a transcript, or a persistent developer activity log.

Examples below assume both routers are enabled. Replace sample values and reasons with evidence;
do not copy a positive suitability claim into an unknown-baseline result.

```text
---

### Adaptive Task Routing

✓ 維持目前設定

目前設定足以完成剩餘核對，切換帶來的改善有限。

對話：留在目前對話，不需開新對話。
目前 AI：<已觀察的模型與原生推理設定>。

不需操作，接著執行已授權的核對。
```

```text
---

### Adaptive Task Routing

暫時沿用設定

若後續授權執行檢查，切換效益仍需確認；本輪僅交付分析與計畫。

對話：留在目前對話，不需開新對話。
任務適配設定：<有依據的模型與原生推理設定>，不代表現在需要切換。

分析與計畫已交付；尚未開始實作。
```

The last sentence depends on authorization: continue authorized work immediately when no pending
decision remains; finish a plan-only deliverable without an artificial routing question. For
change or a material blocker, end with one concrete question or known manual next step instead.
