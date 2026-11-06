# 🎓 Copilot Lesson: Enterprise AI Settings in VS Code
**Date:** 2026-11-03 | **Day:** 34 of learning streak
**Source:** [code.visualstudio.com/docs/enterprise/manage-ai-settings](https://code.visualstudio.com/docs/enterprise/manage-ai-settings) | **Difficulty:** advanced

## 🎯 What You'll Learn

Today you'll learn how organizations govern AI features in VS Code through three distinct management layers, and how precedence rules decide which control wins when they overlap. You'll also see how admins set managed model defaults, dynamic OTel headers, plugin restrictions, and temporary local-harness routing controls.

## 📖 The Concept

VS Code enterprise AI governance is split across three solutions, each controlling a different layer. **Copilot enterprise-managed settings** are Copilot guardrails delivered by GitHub (server-managed, MDM-managed, or file-based) that apply across supported clients like VS Code and Copilot CLI. **VS Code device policies** are editor-specific controls deployed through OS-level tools like Group Policy, Intune, or macOS configuration profiles, applied to the managed device. **GitHub organization/enterprise settings** are account-level, service-side controls configured on GitHub itself — Copilot access, model availability, content exclusions, and org-provided custom instructions or custom agents. They follow the signed-in account, not the device.

The critical skill is knowing which layer to use for which control, and how they stack. When the same managed setting arrives from multiple sources, precedence is: MDM-managed, then server-managed, then file-based, then user settings. For permission keys like `permissions.deny`, `permissions.ask`, and `permissions.allow`, restrictions from multiple sources *combine* in the most restrictive direction rather than one source simply winning. When an enterprise-managed setting and a VS Code device policy configure the *same* control, the enterprise-managed setting wins outright (no merging); if they configure different controls, both apply. The docs strongly recommend configuring any overlapping control in only one system, and `Developer: Policy Diagnostics` lets you inspect the effective value and its source.

On the model side, admins can set a consistent starting model via the top-level `model` managed setting (or `fusionTier` with values `standard`, `high`, `max`, or `unmanaged` for Copilot harness sessions) without locking developers out of picking another model per task. A temporary control, `vscode.temporaryDefaultToLocalHarness`, can suppress automatic routing to the Copilot harness — including routing triggered by managed `sandbox.enabled` or `permissions` values — without changing explicit or remembered harness choices; the docs warn that local sessions don't enforce runtime-only sandbox and permission rules, so this has security implications.

For observability, VS Code 1.143 added two managed telemetry capabilities: `telemetry.filePath` writes Copilot Agent Host telemetry to a JSONL file with no collector and no personal OTel opt-in, and `telemetry.headersProvider` generates and refreshes dynamic HTTP headers for OTLP export (routing, metadata, authentication). The highest-precedence delivery source that supplies telemetry owns the entire block — omitted fields don't inherit from lower-precedence sources. On the device-policy side, notable controls include `ChatApprovedAccountOrganizations` (fail-closed gate requiring an approved org before AI features activate), `ChatDefaultModel`, `ChatToolsAutoApprove` (disabling global auto-approval so saved approvals aren't applied), network filtering via `ChatAgentNetworkFilter` plus allowed/denied domain lists, and plugin governance via `ChatEnabledPlugins` and `ChatStrictMarketplaces`.

## 🔑 Key Takeaways

- Three governance layers exist — Copilot enterprise-managed settings (cross-client guardrails), VS Code device policies (editor-specific, per-device), and GitHub org/enterprise settings (account-level, service-side) — and each control should live in only one system.
- Precedence for managed settings is MDM → server → file → user; permission keys (`deny`/`ask`/`allow`) combine most-restrictively, and enterprise-managed settings beat overlapping device policies without merging.
- Admins can set a default model or fusion tier, restrict plugins and marketplaces, gate AI features to approved orgs (fail-closed), filter agent network access by domain, and control tool auto-approval — all without blocking developers from per-task choices where intended.
- `telemetry.filePath` and `telemetry.headersProvider` (1.143+) enable managed OTel export, file-based JSONL logs, and dynamic collector headers with no developer opt-in; the highest-precedence telemetry source owns the whole block.
- `Developer: Policy Diagnostics` is the verification tool: it shows the effective value of any policy and which source supplied it — always check it after rollout.

## 💡 Practical Example

An org wants new agent sessions to start in Assisted permissions but never allow "Allow all". That's a Copilot enterprise-managed setting (not a device policy), delivered via server or MDM:

```json
{
  "permissions": {
    "disableBypassPermissionsMode": "disable",
    "disableAssistedPermissionsMode": false,
    "defaultMode": "assisted"
  }
}
```

After deployment, in VS Code the permission picker shows restricted levels visibly but disabled, with a "Disabled by policy" label and briefcase icon. To verify it actually took effect — especially if team overrides via `copilot/team-mappings.json` could interfere — run `Developer: Policy Diagnostics` and confirm the source and value. A developer who still sees "Allow all" can be traced by checking which delivery source is winning per the precedence order, or whether a lower-precedence source is being shadowed.

## 🔗 Related Concepts

- [Enterprise-managed settings reference](https://docs.github.com/en/copilot/reference/enterprise-administrators/enterprise-managed-settings)
- [VS Code enterprise policy reference](https://code.visualstudio.com/docs/enterprise/policies)
- [Agent sandboxing](https://code.visualstudio.com/docs/agents/run/agent-sandboxing)
- [Tool approval and permission levels](https://code.visualstudio.com/docs/agents/run/approvals)

## 🤔 Quick Check

Your security team deployed `ChatToolsAutoApprove: false` as a VS Code device policy to stop agents from auto-running terminal commands. A week later, a developer's Copilot harness session runs a shell command without prompting, and the team is confused why their policy didn't catch it. Based on what you learned today, where would you look first, and what does that tell you about the limits of that particular policy?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** You'd start with `Developer: Policy Diagnostics` to inspect the effective approval settings, then check the Copilot enterprise-managed permission settings rather than the device policy. The docs state that `ChatToolsAutoApprove` only controls the *global* auto-approval toggle — with it set to false, tool confirmations offer only one-time approval and previously saved approvals aren't applied. But in Copilot harness sessions, a command can still be approved through session-level permission levels like Assisted permissions or saved session approvals, and tool-eligibility controls belong to managed settings like `ChatToolsEligibleForAutoApproval` and `permissions.disableBypassPermissionsMode`. The lesson is that no single policy is a blanket terminal block; you need to verify which approval path fired in the harness your developers actually use.

</details>

---
*Lesson 34 of Phase 3 (Advanced Workflows) | Streak: 34 days*
