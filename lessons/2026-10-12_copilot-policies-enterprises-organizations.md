# 🎓 Copilot Lesson: GitHub Copilot Policies for Enterprises and Organizations

**Date:** 2026-10-12 | **Day:** 18 of learning streak
**Source:** https://docs.github.com/en/copilot/concepts/enterprise/policies | **Difficulty:** advanced

## 🎯 What You'll Learn

Copilot is not just a toggle you flip per user — enterprises and organizations steer it with AI policies that decide which features, agents, and models exist at all, and where. Today you'll learn how policy precedence works when a developer holds licenses from multiple organizations or even multiple enterprises, and how to keep "policy drift" from quietly changing what your team can use.

## 📖 The Concept

Policies live in the enterprise's **AI controls** tab (or an organization's settings) and gate everything from Copilot Chat on github.com to the Copilot app and Copilot CLI. Each policy can be explicitly enabled, explicitly disabled, or left on "let organizations decide," which hands the decision down to the orgs inside the enterprise. The notable exception is the Copilot cloud agent, where the enterprise picks exactly which organizations get access instead of using the org-decides option. Users licensed directly by the enterprise rather than through an org are covered by a separate "Policies for enterprise-assigned users" setting.

Most policies apply only to people on your Copilot plan, and a user is governed by the enterprise or organization that hands them their Copilot Business or Enterprise license. That license is what matters when conflicts arise: if one user holds licenses from several organizations in the *same* enterprise and those orgs disagree, the **least restrictive** policy usually wins. When licenses come from *different* enterprises, the rule flips — the **most restrictive** policy across the enterprises almost always applies. A personal plan is cancelled the moment someone joins a Business or Enterprise plan, so personal settings can't conflict with org policies.

Two "default availability" policies control what unconfigured generally-available features and models do out of the box: enabled by default, or disabled until an admin opts in. Not every policy applies to every surface, so one user can be fully allowed in VS Code while blocked in the CLI — the Copilot app and the CLI are governed by separate, independent client policies you can switch in opposite directions. The docs also name the failure mode: **policy drift**, where too many people with edit access let settings wander over time. The prescribed countermeasures are reviewing who holds the "Manage enterprise AI controls" permission (or the granular org equivalents) and watching policy changes in the audit log.

## 🔑 Key Takeaways

- Enterprise-level policies cascade; "let organizations decide" pushes the decision down, except for the cloud agent, where the enterprise names the orgs explicitly.
- Conflicting licenses resolve least-restrictive within one enterprise, but most-restrictive across different enterprises.
- The Copilot app and CLI have independent client policies, so you can allow one and block the other.
- Regular permission reviews plus audit-log monitoring are the documented defense against policy drift.

## 💡 Practical Example

Your enterprise wants everyone to keep using Copilot in IDEs but nobody to run the CLI yet. As enterprise owner, open **Enterprise settings → AI controls** and disable the Copilot CLI policy at the enterprise level, while leaving the IDE policies on "let organizations decide." Because a disabled enterprise policy applies to every surface that authenticates for it, individual orgs can't flip it back on. A week later a contractor joins a second org and complains their CLI behaves differently than a teammate's — with the CLI disabled enterprise-wide, the cross-org question never comes up, because the enterprise decision wins. And when an org owner later wonders who changed a model allow-list, the audit log shows exactly which account made the change and when.

## 🔗 Related Concepts

- [Feature availability when GitHub Copilot policies conflict](https://docs.github.com/en/copilot/reference/enterprise-administrators/policy-conflicts)
- [Supported surfaces for GitHub Copilot policies](https://docs.github.com/en/copilot/reference/supported-surfaces-for-policies)
- [Default availability of Copilot features and models](https://docs.github.com/en/copilot/concepts/enterprise/default-availability)

## 🤔 Quick Check

Your enterprise contains two organizations: one has the Copilot CLI policy enabled, the other disabled. A developer holds a license from both and reports that the CLI is still not available. Based on the conflict rule for multiple licenses inside a single enterprise, what should be happening to this developer, and where would you look first if the expected access isn't there?

<details>
<summary>Click to reveal the answer</summary>

**Answer:** The least-restrictive rule for licenses inside one enterprise means the developer's access should be determined by the org that has the CLI enabled — so the CLI should be working for them.

If it isn't, the block is coming from outside that rule: a higher-level enterprise policy that disables the CLI, the CLI's own client policy on the surface in question, or a license from a *different* enterprise where the most-restrictive policy wins.

</details>

---
*Lesson 18 of Phase 3 (Advanced Workflows) | Streak: 18 days*
