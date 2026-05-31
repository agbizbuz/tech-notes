Local Agentic Coding Setup: MacBook Pro M 5 Pro (64 GB RAM)

This configuration optimizes a **32 B parameter model** for autonomous coding tasks in **Python** and **Rust**, utilizing **Ollama** for the backend, **Aider/OpenCode** for the interface, and **Vibe** for sandboxing.

---

**1. Core Infrastructure**

Install the base requirements for Apple Silicon.

- **Model Backend:** `brew install ollama`
- **Coding Agent:** `pip install aider-chat`
- **Execution Sandbox:**
    
    Bash
    
    ```
    curl -fsSL https://github.com -o vibe.zip
    unzip vibe.zip && mv vibe /usr/local/bin/
    ```
    
    Verwende Code mit Vorsicht.
    

---

**2. Custom Model Definitions (Modelfiles)**

Create these files to bake specialist prompts and a **32 k context window** into your local LLM.

**Python Specialist (`PythonModelfile`)**

Dockerfile

```
FROM qwen2.5-coder:32b
PARAMETER temperature 0.2
PARAMETER num_ctx 32768
SYSTEM """
You are an expert Python 3.12+ engineer. 
- Typing: Use strict type hints for all signatures.
- Standards: Follow PEP 8 and ruff linting expectations.
- Modernity: Prefer pathlib and asyncio for I/O tasks.
- Agentic Behavior: Describe plan in 3 bullet points before writing code.
"""
```

Verwende Code mit Vorsicht.

**Rust Specialist (`RustModelfile`)**

Dockerfile

```
FROM qwen2.5-coder:32b
PARAMETER temperature 0.1
PARAMETER num_ctx 32768
SYSTEM """
You are a senior Rust systems engineer. 
- Safety: Avoid 'unsafe' blocks; justify if used.
- Idiomatic Code: Use clippy-compliant patterns and '?' operator.
- Efficiency: Prioritize stack allocation and 'Cow' for strings.
- Crate Management: Check Cargo.toml before adding new dependencies.
"""
```

Verwende Code mit Vorsicht.

**Build Commands:**

Bash

```
ollama create qwen-python -f PythonModelfile
ollama create qwen-rust -f RustModelfile
```

Verwende Code mit Vorsicht.

---

**3. Workflow Execution**

**Scenario A: Pair Programming (Aider)**

Best for direct file editing within a Git repository.

Bash

```
# Python
aider --model ollama_chat/qwen-python

# Rust
aider --model ollama_chat/qwen-rust
```

Verwende Code mit Vorsicht.

**Scenario B: Autonomous Agents (OpenCode + Vibe)**

Best for tasks requiring shell execution (tests, builds, installs) in a secure sandbox.

Bash

```
# Enter the sandbox
vibe
# Run the agent (connected to local Ollama endpoint)
opencode 
```

Verwende Code mit Vorsicht.

---

**Hardware Optimization (M 5 Pro 64 GB)**

- **Memory Allocation:** The 32 B model (Q 4 quantization) uses ~20 GB. The remaining ~44 GB supports the 32 k context window and system overhead.
- **Quantization:** Use `Q4_K_M` for the best balance of logic and speed.

To make these environments instant to access, add these aliases to your shell configuration file (likely `~/.zshrc` on a Mac). These commands streamline the launch process by automatically starting the backend and entering the correct agent mode.

## 1. Update your Shell Profile

Open your configuration in a text editor:

```bash
nano ~/.zshrc
```

## 2. Add the Aliases

Paste the following block at the bottom of the file:

```bash
# --- Local LLM Agent Aliases ---

# Launch Python Agent (Aider)
alias pyagent='aider --model ollama_chat/qwen-python'

# Launch Rust Agent (Aider)
alias rsagent='aider --model ollama_chat/qwen-rust'

# Launch Secure Sandbox (Vibe + OpenCode)
alias safeagent='vibe --cmd "opencode"'

# Quick check on Ollama resources/models
alias llm-status='ollama ps && top -l 1 | grep "PhysMem"'
```

## 3. Apply Changes

Reload your terminal profile to activate the commands:

```bash
source ~/.zshrc
```

## Usage Guide

- `pyagent`: Use this when you are in a Python project folder. It will index your files and start the specialized coding session.
- `rsagent`: Use this for Rust workspaces to ensure memory-safe code generation.
- `safeagent`: Use this when you want an agent to run terminal commands or install dependencies autonomously without risking your host macOS system.
- `llm-status`: A quick utility to see if your 64 GB RAM is being throttled or to check which model is currently loaded in memory.

Architect Mode is a powerful two-step workflow in Aider that separates planning from execution. For your M 5 Pro setup, this prevents the model from "getting ahead of itself" and results in higher-quality code edits. [1, 2, 3, 4, 5]

## 1. How it Works

In standard "Code" mode, the LLM tries to explain the solution and write the code at the same time. In Architect Mode: [6]

1. The Architect Phase: The LLM proposes a high-level solution and design pattern.
2. The Human Check: You review the plan. If it's wrong, you correct it before a single line is written.
3. The Editor Phase: Once you say "go ahead," the LLM (or a second model) translates that plan into specific file edits. [1, 4, 6, 7, 8, 9]

## 2. Launching Architect Mode

You can launch directly into this mode using the `--architect` flag. [10, 11]

- Python: `aider --model ollama_chat/qwen-python --architect`
- Rust: `aider --model ollama_chat/qwen-rust --architect` [12, 13, 14]

Tip: If you are already in a session, switch modes instantly by typing `/chat-mode architect`. [12, 15]

## 3. Recommended Workflow

Since you have 64 GB of RAM, you can actually use two different models for this—a "smart" architect and a "fast" editor—though using your 32 B model for both is excellent for local-only work. [1, 14]

1. Describe the Task: `architect> Refactor the user authentication to use JWT instead of sessions.`
2. Review the Plan: The model will list the files it needs to change and the logic it will use.
3. Refine or Approve:
    
    - _To refine: _ "Actually, use the `PyJWT` library specifically."
    - _To approve: _ Just type `ok` or `go ahead`.
    
4. Auto-Commit: Aider will apply the changes and, by default, create a git commit once the "Editor" phase finishes. [4, 8, 15, 16, 17]

## 4. Updated Alias for Architect Mode

Add this to your `~/.zshrc` to make this your default way of working:

```bash
# Launch the Python Agent specifically in Architect Mode
alias pyplan='aider --model ollama_chat/qwen-python --architect'
```

Pro-Tip: If you find the local 32 B model struggles to follow the strict "Editor" format, you can use a "Weak Model" (like a 7 B parameter model) as the editor by adding `--editor-model ollama_chat/qwen2.5-coder:7b` to your launch command. [14]

Using a secondary, smaller Editor model allows your "Architect" (the 32 B model) to handle high-level logic while a faster model (like an 8 B model) handles the repetitive task of writing code back into files. On your M 5 Pro, this speeds up the final "writing" phase significantly since smaller models have much higher tokens-per-second. [1, 2, 3, 4]

## 1. Pick a "Fast" Editor Model

For the editor role, Qwen 2.5-Coder-7 B or Llama-3-8 B are ideal because they are highly efficient at following formatting instructions without the overhead of a 32 B parameter count.

```bash
# Pull a fast 7B model to act as your Editor
ollama pull qwen2.5-coder:7b
```

## 2. Launch with Dual-Model Configuration

You can specify a "Weak" or "Editor" model using the `--editor-model` flag. This tells Aider to use your smart model for planning and the smaller model for implementation. [5, 6, 7]

Python Example:

```bash
aider --model ollama_chat/qwen-python --architect --editor-model ollama_chat/qwen2.5-coder:7b
```

Rust Example:

```bash
aider --model ollama_chat/qwen-rust --architect --editor-model ollama_chat/qwen2.5-coder:7b
```

## 3. Optimized Dual-Model Aliases

Update your `~/.zshrc` with these "High-Speed" aliases to make this workflow effortless.

```bash
# High-speed Architect + Editor workflow
alias pydash='aider --model ollama_chat/qwen-python --architect --editor-model ollama_chat/qwen2.5-coder:7b'
alias rsdash='aider --model ollama_chat/qwen-rust --architect --editor-model ollama_chat/qwen2.5-coder:7b'
```

## Why this works best on 64 GB RAM:

- Parallel Loading: Your Mac will keep both models (32 B + 7 B) in unified memory simultaneously (~20 GB + ~5 GB = 25 GB total).
- Instant Switching: There is zero "swap" time between planning and writing phases because both fit comfortably in your 64 GB pool.
- Prompt Caching: Aider will cache the project context in the 32 B Architect, while the 7 B Editor only processes the specific diffs, saving massive amounts of compute time. [8, 9]

Pro-Tip: If the 7 B model makes mistakes in the final code blocks, try increasing its temperature slightly or revert to using the 32 B model for both—your M 5 Pro is fast enough to handle both roles if precision is more important than raw speed.

Monitoring memory on your MacBook Pro M 5 Pro is essential for ensuring your 32 B Architect and 7 B Editor models coexist efficiently within your 64 GB unified memory pool.

## 1. Real-Time Terminal Dashboards

The best way to see how much RAM the LLMs are "wiring" (dedicating to the GPU) is through specialized Apple Silicon monitoring tools.

- ASITOP: An `nvtop` -inspired terminal UI specifically for Apple Silicon. It shows CPU/GPU utilization, power consumption, and total memory usage in a sleek, color-coded dashboard.
    
    - Install: `brew install asitop`
    - Run: `sudo asitop` (requires root to read power metrics).
    
- Macmon: A lightweight alternative that provides similar GPU and memory metrics but often runs without requiring `sudo`.
    
    - Install: `brew install macmon`
    - Run: `macmon` [1, 2, 3, 4, 5, 6]
    

## 2. Ollama-Specific Monitoring

To see exactly how much memory each individual model is consuming while active:

- Command: `ollama ps`.
- What to look for: This command outputs a table showing the size of each loaded model and the percentage of it that is currently in the GPU. With 64 GB, your "Processor" column should ideally show 100% GPU for both models. [7, 8, 9]

## 3. Optimizing for 64 GB RAM

By default, macOS limits the amount of unified memory available to the GPU to roughly 60–70% of total RAM. For a 64 GB machine, this is around 44 GB. [10]

If you find that your 32 B model is "offloading" layers to the CPU (which slows it down significantly), you can manually increase the GPU allocation limit:

- Command: `sudo sysctl iogpu.wired_limit_mb=57344` (sets the limit to 56 GB).
- Verify: Run `sysctl iogpu.wired_limit_mb` to confirm the change. [8, 11, 12]

## Real-Time Memory Health Check

|Tool|Best For|Requirement|
|---|---|---|
|Ollama ps|Checking specific model memory footprints|None|
|ASITOP|Visualizing GPU/RAM/Power load simultaneously| `sudo` |
|Activity Monitor|Viewing "Memory Pressure" (Green/Yellow/Red)|GUI|

To set up a split-terminal view automatically, we will use tmux (Terminal Multiplexer). This allows you to launch your monitoring dashboard in one pane and your coding agent in the other with a single command.

## 1. Install tmux

If you don't have it yet:

```bash
brew install tmux
```

## 2. Create the "Agent Dashboard" Alias

Add this function to your `~/.zshrc`. This script creates a new tmux session, splits the screen vertically, starts `asitop` on the left, and your `pyagent` on the right.

```bash
# Function to launch agent and monitor in a split view
agent-dash() {
    # Start a new tmux session named 'agent' and run asitop in the first pane
    # (Note: asitop usually needs sudo, so it will prompt for your password)
    tmux new-session -d -s agent 'sudo asitop'
    
    # Split the window vertically
    tmux split-window -h -p 65
    
    # Send the agent command to the new right pane (65% width)
    tmux send-keys 'pyagent' C-m
    
    # Attach to the session
    tmux attach-session -t agent
}

# Add a simple alias for the function
alias dashboard='agent-dash'
```

## 3. Apply and Launch

1. Reload your config: `source ~/.zshrc`
2. Launch the dashboard: `dashboard`

## What you will see on your M 5 Pro:

- Left Pane (35%): `asitop` showing your M 5 GPU clusters lighting up and your Memory usage climbing to ~25 GB as your 32 B + 7 B models load.
- Right Pane (65%): Your `aider` interface ready for Architect mode.

## Quick Tips for tmux:

- Exit: Type `exit` in both panes or press `Ctrl+b` then `d` to "detach" and keep the models running in the background.
- Switch Panes: Press `Ctrl+b` then an Arrow Key.
- Resize: Press `Ctrl+b` then hold Option + Arrow Key.

Pro-Tip: Since `asitop` requires `sudo`, you can configure your Mac to let you run it without a password by editing your sudoers file, but most users find it safer to just type the password once upon launch.

Would you like to see how to auto-load your specific Python/Rust project context into these aliases so they open exactly where you left off?
  

[1] [https://medium.com](https://medium.com/@MetricFire/monitor-macos-apple-silicon-gpu-with-macmon-hosted-graphite-metricfire-b9f39fadb2e6)

[2] [https://whatsuphome.fi](https://whatsuphome.fi/blog/part-111-some-handy-command-line-tools-plus-mac-gpu-monitoring#:~:text=asitop:%20Monitor%20Apple%20Silicon%20Mac%20CPU/GPU/ANE.%20Speaking,Neural%20Engine%29%20usage%20in%20a%20fancy%20way.)

[3] [https://github.com](https://github.com/tlkh/asitop)

[4] [https://www.reddit.com](https://www.reddit.com/r/macapps/comments/1dr13tr/monitor_current_apple_silicon_performance_in/)

[5] [https://tlkh.github.io](https://tlkh.github.io/asitop/)

[6] [https://github.com](https://github.com/vladkens/macmon)

[7] [https://docs.ollama.com](https://docs.ollama.com/faq)

[8] [https://stencel.io](https://stencel.io/)

[9] [https://medium.com](https://medium.com/@rosgluk/ollama-cli-cheatsheet-ls-serve-run-ps-commands-2026-update-ad4765daf43f)

[10] [https://www.reddit.com](https://www.reddit.com/r/LocalLLaMA/comments/186phti/m1m2m3_increase_vram_allocation_with_sudo_sysctl/)

[11] [https://stencel.io](https://stencel.io/posts/apple-silicon-limitations-with-usage-on-local-llm%20.html)

[12] [https://medium.com](https://medium.com/@se.mehmet.baykar/increase-vram-on-apple-silicon-for-local-llms-1b35c453b165)

  

[1] [https://aider.chat](https://aider.chat/2024/09/26/architect.html)

[2] [https://jasongoecke.substack.com](https://jasongoecke.substack.com/p/aiders-new-architecteditor-feature)

[3] [https://arxiv.org](https://arxiv.org/html/2603.05344v1)

[4] [https://medium.com](https://medium.com/@technobios/getting-started-running-ai-models-locally-with-ollama-41c2411c0833#:~:text=Start%20with%20smaller%20models%20%28like%20starting%20with,Document%20your%20prompts%20%28like%20documenting%20SQL%20queries%29)

[5] [https://aider.chat](https://aider.chat/docs/usage/modes.html)

[6] [https://aider.chat](https://aider.chat/docs/config/options.html)

[7] [https://codenotary.com](https://codenotary.com/blog/step-by-step-guide-refactoring-a-large-rust-codebase-with-aiderdev-and-custom-llms)

[8] [https://www.reddit.com](https://www.reddit.com/r/macbookpro/comments/1s47lmi/best_local_llm_recommendations_for_coding_stats/#:~:text=Device:%20MacBook%20Pro%20M5%20Max.%20RAM:%2064GB,Specs:%2018%2Dcore%20CPU%2C%2040%2Dcore%20GPU%2C%202TB%20Storage.)

[9] [https://dev.to](https://dev.to/bspann/running-llms-locally-on-macos-the-complete-2026-comparison-48fc)

  

[1] [https://aider.chat](https://aider.chat/2024/09/26/architect.html)

[2] [https://www.youtube.com](https://www.youtube.com/watch?v=tElgVPUargw&t=1092)

[3] [https://github.com](https://github.com/Aider-AI/aider/issues/3634)

[4] [https://www.blott.com](https://www.blott.com/blog/post/aider-review-a-developers-month-with-this-terminal-based-code-assistant)

[5] [https://codenotary.com](https://codenotary.com/blog/step-by-step-guide-refactoring-a-large-rust-codebase-with-aiderdev-and-custom-llms)

[6] [https://aider.chat](https://aider.chat/2024/09/26/architect.html)

[7] [https://www.reddit.com](https://www.reddit.com/r/ChatGPTCoding/comments/1fshzxl/aiders_architecteditor_approach_sets_new_sota_for/)

[8] [https://aider.chat](https://aider.chat/docs/usage/modes.html)

[9] [https://github.com](https://github.com/Aider-AI/aider/issues/3543)

[10] [https://aider.chat](https://aider.chat/docs/usage/modes.html)

[11] [https://aider.chat](https://aider.chat/docs/usage/modes.html)

[12] [https://aider.chat](https://aider.chat/docs/usage/modes.html)

[13] [https://aider.chat](https://aider.chat/docs/llms/ollama.html#:~:text=%23%20Pull%20the%20model%20ollama%20pull%20%23,your%20codebase%20cd%20/to/your/project%20aider%20%2D%2Dmodel%20ollama_chat/)

[14] [https://github.com](https://github.com/Aider-AI/aider/issues/2258)

[15] [https://aider.chat](https://aider.chat/docs/usage/commands.html)

[16] [https://www.goodvibecode.com](https://www.goodvibecode.com/tools/aider)

[17] [https://www.youtube.com](https://www.youtube.com/watch?v=UeY1Y4oEyAQ)