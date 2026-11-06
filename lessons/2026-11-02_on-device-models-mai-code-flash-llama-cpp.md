# 🎓 Copilot Lesson: On-Device Models (MAI Code 1.1 Flash Locally with llama.cpp)
**Date:** 2026-11-02 | **Day:** 33 of learning streak
**Source:** https://code.visualstudio.com/docs/agent-customization/language-models | **Difficulty:** intermediate

## 🎯 What You'll Learn

VS Code can now run a Copilot-provided coding model — currently MAI Code 1.1 Flash — directly on your own hardware, served by llama.cpp, inside Copilot harness sessions. You get a local model with zero credit consumption, without having to hunt down, download, and serve a model yourself. The trade-offs are heavy: strict GPU requirements, a big download, and some security caveats worth knowing before you let it run with auto-approved actions.

## 📖 The Concept

On-device models let you run a model that Copilot itself provides and manages. VS Code uses [llama.cpp](https://github.com/ggml-org/llama.cpp) to serve the model on your device; the model is downloaded from the Azure ML registry, stored in `~/.copilot/azureml-models`, and shared with other Copilot apps such as Copilot CLI. You can use the local model alongside cloud models: auto-model selection can route suitable requests to the on-device model and falls back to a cloud model (before any output) if the local one can't respond.

It's worth separating this from the BYOK local models from last week's lesson. BYOK models (e.g., Ollama) are providers *you* configure, and they work without a GitHub sign-in. On-device models are provided and managed by Copilot and *require* GitHub sign-in — they bring that capability to Copilot in VS Code rather than to your own key.

The requirements are the real gatekeeper. You need: GitHub sign-in with Copilot access (for Business/Enterprise, the admin must enable Editor Preview Features in the Copilot policy settings), a local Copilot harness session in the Chat view or Agents window, an NVIDIA GPU with 96 GB or more of GPU memory (unified memory that the GPU shares with the system counts toward the requirement), about 55 GB of free disk space (the MAI Code 1.1 Flash download is 52.8 GB), and a llama.cpp build of b11658 or later — VS Code does not install llama.cpp for you.

Using it is straightforward once set up: open the model picker in a Copilot harness session, pick the **On This Device** tab, and download the model (progress shows above the chat input, with a Pause option). Loading takes a few minutes. When selected, the picker and the footer of every response show the model name followed by **On-device**, and responses don't use AI credits — the footer shows 0 credits. The model's details page lets you tune Effort Level (Low/Medium/High) and Context size (sized to your GPU memory), and only one on-device model is loaded at a time. One caution the docs flag explicitly: local models might not use the same safety filters as Copilot's cloud models and may be more susceptible to malicious instructions — especially when actions are automatically approved.

## 🔑 Key Takeaways

- On-device models are Copilot-provided and -managed (unlike BYOK/Ollama): they require GitHub sign-in and run only in local Copilot harness sessions, served by a llama.cpp build you install (b11658+).
- Hardware bar is high: NVIDIA GPU with 96 GB+ VRAM (unified memory counts), ~55 GB free disk, and for Business/Enterprise an admin opt-in to Editor Preview Features.
- On-device responses cost 0 AI credits; models live in `~/.copilot/azureml-models` and are shared with Copilot CLI and other Copilot apps.
- Auto-model selection can route to the local model with cloud fallback (enable "Use in Auto"); a model too slow for your device shows "Too slow for Auto" but can still be selected directly.
- Local models may lack cloud-side safety filters — be especially careful with auto-approved actions.

## 💡 Practical Example

Setting up MAI Code 1.1 Flash on a qualifying machine:

1. Install a llama.cpp build (b11658 or later) from the [releases page](https://github.com/ggml-org/llama.cpp/releases).
2. Configure the model:

```jsonc
// settings.json
{
  // machine-scoped: absolute path to the llama-server executable
  "chat.copilot.localModel.llamaServerPath": "/usr/local/bin/llama-server",
  // starts the local Copilot runtime (restarts after active responses finish)
  "chat.copilot.localModel.enabled": true,
  // optional: let Auto-model selection route requests to this model
  "chat.copilot.localModel.useInAuto": true
}
```

3. In a Copilot harness session (Chat view or Agents window), open the model picker → **On This Device** tab → select **MAI Code 1.1 Flash** → **Download** (~52.8 GB). Keep using Copilot while it downloads; Pause is available.
4. After the download, VS Code verifies and loads the model (a few minutes), then offers **Switch to MAI Code 1.1 Flash**. Responses now show "MAI Code 1.1 Flash On-device" in the footer with 0 credits.
5. If setup fails, the tab shows a status (e.g., "Needs llama.cpp", "GPU memory busy", "Not enough disk space") and the notice above the chat input offers actions like Get llama.cpp, Open Settings, or Retry. Management is also available via Command Palette: `Chat: Start/Stop/Update On-Device Model`, `Chat: Remove On-Device Model...`, etc.

## 🔗 Related Concepts

- [Bring Your Own Language Model Key (BYOK)](https://code.visualstudio.com/docs/agent-customization/language-models#_bring-your-own-language-model-key)
- [Auto model selection](https://code.visualstudio.com/docs/agents/concepts/language-models#_auto-model-selection)
- [Agent harnesses](https://code.visualstudio.com/docs/agents/run/agent-harnesses)
- [Approvals for agent actions](https://code.visualstudio.com/docs/agents/run/approvals)
- [AI credits and model costs](https://code.visualstudio.com/docs/agents/concepts/language-models#_ai-credits-and-model-costs)

## 🤔 Quick Check

Your team has two developer workstations: one with an Apple Silicon GPU and 80 GB of unified memory, and one with an NVIDIA GPU and 96 GB of VRAM. Both are signed in to GitHub under Copilot Business, and the organization administrator has just enabled Editor Preview Features in the Copilot policy settings. A teammate asks which machine can run MAI Code 1.1 Flash on-device and what the setup would involve. Answer from the docs: which machine qualifies, why the other does not, and what you would install, configure, and download on the one that does.

<details>
<summary>Click to reveal the answer</summary>

**Answer:** The NVIDIA machine qualifies; the Apple Silicon machine does not. The docs specify an NVIDIA GPU with 96 GB or more of GPU memory — unified memory counts toward that number, but the hardware requirement is NVIDIA, so an 80 GB Apple Silicon GPU fails both counts. On the NVIDIA machine: install a llama.cpp build (b11658 or later — VS Code won't install it), point the machine-scoped `chat.copilot.localModel.llamaServerPath` setting at the llama-server executable, enable `chat.copilot.localModel.enabled`, then in a local Copilot harness session open the model picker's On This Device tab and download MAI Code 1.1 Flash (~55 GB needed on disk). The admin's Editor Preview Features opt-in is what makes the model visible to your Copilot Business account at all, and once the model is loaded, responses run at 0 AI credits.

</details>

---
*Lesson 33 of Phase 4 (Deep Customization) | Streak: 33 days*
