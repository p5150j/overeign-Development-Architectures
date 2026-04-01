# Ubuntu RTX 4090 — Zero-Cost AI Coding Stack Setup Guide
**Stack: Ollama + Qwen3.5-27B + Roo Code + Aider + Hybrid API Fallback**

---

## Table of Contents
1. [Prerequisites & System Check](#1-prerequisites--system-check)
2. [NVIDIA Drivers & CUDA](#2-nvidia-drivers--cuda)
3. [Ollama Installation & Configuration](#3-ollama-installation--configuration)
4. [Pulling the Right Models](#4-pulling-the-right-models)
5. [VS Code + Roo Code Setup](#5-vs-code--roo-code-setup)
6. [Aider Setup (Terminal / Git Workflow)](#6-aider-setup-terminal--git-workflow)
7. [Reading Live Documentation (MCP + Playwright)](#7-reading-live-documentation-mcp--playwright)
8. [Vision & Multimodal (Analyzing Images Locally)](#8-vision--multimodal-analyzing-images-locally)
9. [Hybrid API Fallback (DeepSeek V3.2)](#9-hybrid-api-fallback-deepseek-v32)
10. [Performance Tuning](#10-performance-tuning)
11. [Daily Workflow Guide](#11-daily-workflow-guide)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. Prerequisites & System Check

Before touching anything else, verify your environment is in the right shape.

### Ubuntu Version
```bash
lsb_release -a
```
This guide is written for **Ubuntu 22.04 LTS or 24.04 LTS**. Both work. If you're on 20.04, upgrade before proceeding — Ollama and several dependencies have dropping support for it.

### Check Your GPU is Recognized
```bash
lspci | grep -i nvidia
```
You should see your RTX 4090. If nothing shows, the card may not be seated properly or there's a PCIe issue.

### Check Current Driver Status
```bash
nvidia-smi
```
If this returns a result with your GPU listed and CUDA version shown, you already have working drivers. Note the CUDA version shown in the top-right of the output — you'll need it later. If the command is not found, proceed to Section 2.

### System Package Update (Always Do This First)
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl wget git build-essential
```

---

## 2. NVIDIA Drivers & CUDA

### Check for Existing Drivers
```bash
dpkg -l | grep nvidia
```

### Install the Right Driver (If Missing or Outdated)
The RTX 4090 requires driver **≥ 525**. For production use, install the latest recommended stable driver:

```bash
# Add the NVIDIA PPA for latest drivers
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update

# See what's available and recommended
ubuntu-drivers devices

# Install recommended automatically
sudo ubuntu-drivers autoinstall

# OR install a specific version explicitly (530+ recommended for 4090)
sudo apt install -y nvidia-driver-550
```

Reboot after driver install:
```bash
sudo reboot
```

### Verify After Reboot
```bash
nvidia-smi
```
Expected output will show:
- Driver Version: 550.x or higher
- CUDA Version: 12.x
- GPU Name: NVIDIA GeForce RTX 4090
- Memory-Usage: ~0MiB / 24564MiB (clean state)

### CUDA Toolkit (Required for Some Tools)
Ollama bundles its own CUDA libraries, so you don't strictly need the full CUDA toolkit. However, Aider and some Python tooling benefits from it:

```bash
# Install CUDA toolkit (matches driver version)
sudo apt install -y nvidia-cuda-toolkit

# Verify
nvcc --version
```

---

## 3. Ollama Installation & Configuration

Ollama is the runtime that serves models locally via an OpenAI-compatible API endpoint. Everything else (Roo Code, Aider, Continue.dev) connects to it.

### Install Ollama
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

This installs Ollama as a systemd service and puts the `ollama` binary in `/usr/local/bin`.

### Verify Installation
```bash
ollama --version
ollama list  # Should be empty at first
```

### Configure Ollama as a System Service

By default, Ollama only listens on localhost (127.0.0.1:11434). This is fine for a single-machine setup. To confirm it's running:

```bash
systemctl status ollama
```

If it's not running, start it:
```bash
sudo systemctl start ollama
sudo systemctl enable ollama  # Auto-start on boot
```

### Configure Ollama Environment Variables

Create or edit the Ollama service override to set important environment variables:

```bash
sudo systemctl edit ollama
```

This opens an editor. Add the following between the comment lines:

```ini
[Service]
Environment="OLLAMA_NUM_PARALLEL=1"
Environment="OLLAMA_MAX_LOADED_MODELS=1"
Environment="OLLAMA_FLASH_ATTENTION=1"
Environment="OLLAMA_GPU_OVERHEAD=512"
```

**What these do:**
- `OLLAMA_NUM_PARALLEL=1` — Only one request at a time. You're sole user; parallel wastes VRAM.
- `OLLAMA_MAX_LOADED_MODELS=1` — Keep only one model hot in VRAM at a time. Critical on 24GB.
- `OLLAMA_FLASH_ATTENTION=1` — Enables Flash Attention 2. Significantly faster inference and lower VRAM usage for long contexts.
- `OLLAMA_GPU_OVERHEAD=512` — Reserves 512MB for the OS/driver overhead, preventing OOM crashes.

Reload and restart after editing:
```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

### Test the API Endpoint
```bash
curl http://localhost:11434/api/tags
```
Should return a JSON response (empty models list initially).

---

## 4. Pulling the Right Models

Your 24GB VRAM supports two primary models. Pull both — they serve different purposes.

### Primary Model: Qwen3.5-27B
**Best quality. Best all-around agentic coding. Use this 90% of the time.**

```bash
ollama pull qwen3.5:27b
```

- Size on disk: ~16–18GB (Q4_K_M quantization, which Ollama uses by default)
- VRAM usage: ~16–20GB loaded
- SWE-bench Verified: 72.4%
- This download is large (~16GB). Let it complete fully.

### Battle-Tested Fallback: Qwen2.5-Coder-32B
**Most validated by the Aider community. Most tested codebase comprehension. Use for large repo work.**

```bash
ollama pull qwen2.5-coder:32b
```

- Size on disk: ~20GB (Q4_K_M)
- VRAM usage: ~20–22GB loaded
- Note: This is a tight fit on 24GB. It will work but leaves less headroom. Ensure no other GPU processes are running when using it.

### Efficiency Option: Qwen3.5-35B-A3B (MoE)
**Fastest inference. Only 8GB VRAM active. Good for quick tasks and autocomplete.**

```bash
ollama pull qwen3.5:35b-a3b
```

- VRAM usage: ~8GB (only 3B parameters active per token despite 35B total)
- SWE-bench Verified: 69.2%
- Useful when you want speed over maximum quality

### Verify All Models Are Pulled
```bash
ollama list
```

### Test a Model is Working
```bash
ollama run qwen3.5:27b "Write a Python function that flattens a nested list"
```
Type `/bye` to exit the interactive session.

### Managing VRAM — Key Rule
Only one model loads at a time (due to our config). Ollama automatically unloads a model when you switch to another. If you need to manually evict a model from VRAM:

```bash
# Keep a model loaded (pings it to prevent unload)
ollama run qwen3.5:27b ""

# Force unload everything
sudo systemctl restart ollama
```

---

## 5. VS Code + Roo Code Setup

Roo Code is the VS Code extension that turns Ollama into an agentic coding assistant with Custom Modes, diff-based editing, Memory Bank, and browser automation.

### Install VS Code (If Not Already Installed)
```bash
# Download and install the .deb package
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null
sudo apt update
sudo apt install -y code
```

### Install Roo Code Extension
In VS Code, open the Extensions panel (`Ctrl+Shift+X`) and search for **Roo Code**. Install it. Alternatively from terminal:

```bash
code --install-extension RooVeterinaryInc.roo-cline
```

### Configure Roo Code to Use Ollama

1. Open VS Code
2. Click the Roo Code icon in the Activity Bar (left sidebar)
3. Click the gear icon (Settings) in the Roo Code panel
4. Set the following:

| Setting | Value |
|---------|-------|
| API Provider | Ollama |
| Base URL | `http://localhost:11434` |
| Model | `qwen3.5:27b` |
| Context Window | `32768` (start here; increase to 65536 if tasks need it) |
| Max Output Tokens | `8192` |

5. Click **Save**

### Enable Diff-Based Editing (Critical for Token Efficiency)
In Roo Code settings, ensure **Diff Editing Mode** is enabled. This instructs the model to output only changed sections rather than rewriting entire files — essential for token efficiency with local models.

### Set Up the Memory Bank
Roo Code's Memory Bank gives the model persistent context across sessions (project structure, conventions, architecture decisions).

1. In your project root, create a `.roo` directory:
```bash
mkdir -p /your/project/.roo/memory-bank
```

2. Create a `projectBrief.md` file in that directory describing your project:
```bash
cat > /your/project/.roo/memory-bank/projectBrief.md << 'EOF'
# Project: [Your Project Name]

## Stack
[e.g., Python FastAPI backend, React frontend, PostgreSQL]

## Architecture
[Brief description of your project structure]

## Key Conventions
[Naming conventions, code style preferences, etc.]

## Current Focus
[What you're actively working on]
EOF
```

3. Optionally add more context files:

```bash
# Technical context (APIs, integrations, dependencies)
touch /your/project/.roo/memory-bank/techContext.md

# Code conventions (style guide, patterns)
touch /your/project/.roo/memory-bank/conventions.md
```

Roo Code automatically reads all `.md` files in the memory-bank directory at session start.

**Memory Bank tips:**
- Update `currentFocus` section as you switch tasks
- Keep files concise — more context = more tokens per request
- Add `.roo/` to `.gitignore` if it contains sensitive project details
- Memory Bank persists even after VS Code restarts

### Custom Modes — What They Are and How to Use Them
Roo Code ships with these built-in modes (switch with the mode selector at the top of the panel):

| Mode | Use For |
|------|---------|
| **Code** | Writing and editing code (default) |
| **Architect** | System design, planning, reviewing structure |
| **Debug** | Diagnosing and fixing bugs |
| **Orchestrator** | Breaking large tasks into subtasks |
| **Ask** | Questions and explanations without code changes |

**Workflow tip:** Start complex features in **Architect** mode to plan, then switch to **Code** mode to implement. Use **Debug** when something breaks.

### Roo Code Usage Tips
- Use `@` to reference specific files: `@src/api/routes.py fix the authentication logic`
- Use `/` commands for quick actions
- Keep tasks focused — one logical change per prompt works better than "refactor everything"
- If a response gets cut off, type "continue" — Ollama has output token limits

---

## 6. Aider Setup (Terminal / Git Workflow)

Aider is the gold standard for git-native coding. Every AI edit is a clean git commit. Use it for:
- Anything touching multiple files where you want clean commit history
- Code reviews and refactors where audit trail matters
- Working in large codebases where repo-map context is important

### Install Aider
```bash
# Using pip (Python 3.10+ required)
pip install aider-chat --break-system-packages

# Verify
aider --version
```

If pip isn't available or you want isolation:
```bash
# Using pipx for isolated install (recommended)
sudo apt install -y pipx
pipx install aider-chat
pipx ensurepath
source ~/.bashrc
```

### Configure Aider with Ollama

Aider connects to Ollama via its OpenAI-compatible API.

```bash
# Primary setup — use qwen3.5:27b
aider --model ollama/qwen3.5:27b

# Alternative — battle-tested setup
aider --model ollama/qwen2.5-coder:32b
```

### Create a Persistent Config File

Instead of typing flags every time, create an `.aider.conf.yml` in your home directory:

```bash
cat > ~/.aider.conf.yml << 'EOF'
# Default model
model: ollama/qwen3.5:27b

# Architecture: use same model for editing and weak tasks
editor-model: ollama/qwen3.5:27b
weak-model: ollama/qwen3.5:27b

# Git settings
auto-commits: true
commit-prompt: "feat: {summary}"

# Context settings
map-tokens: 2048
max-chat-history-tokens: 16384

# Output settings
dark-mode: true
stream: true
EOF
```

### Project-Level Config

In any project root, create `.aider.conf.yml` to override defaults for that project:

```bash
cat > /your/project/.aider.conf.yml << 'EOF'
# Project-specific model choice
model: ollama/qwen2.5-coder:32b  # Use battle-tested model for this large codebase

# Tell aider about your test command
test-cmd: pytest tests/

# Auto-lint with your linter
lint-cmd: ruff check --fix
EOF
```

### Basic Aider Usage

```bash
# Navigate to your project
cd /your/project

# Start aider (reads .aider.conf.yml automatically)
aider

# Or specify files to focus on
aider src/api/auth.py src/models/user.py

# Run in watch mode (monitors for AI comments in code)
aider --watch-files
```

### Key Aider Commands (Inside the Session)
| Command | Action |
|---------|--------|
| `/add src/file.py` | Add a file to context |
| `/drop src/file.py` | Remove a file from context |
| `/ls` | List files in context |
| `/diff` | Show last change as a diff |
| `/undo` | Undo last commit |
| `/run pytest tests/` | Run a command and share output with model |
| `/ask` | Ask a question without making changes |
| `/exit` | Exit |

### The Repo Map — Aider's Secret Weapon
Aider automatically builds a tree-sitter-based map of your entire repository and feeds relevant structural context to the model. This means it understands cross-file dependencies without you explicitly adding every file. For large codebases, this is a significant advantage over Roo Code's default behavior.

To see the repo map:
```bash
aider --show-repo-map
```

---

## 7. Reading Live Documentation (MCP + Playwright)

When you need Roo Code to read external documentation, API references, or any web page, use the Playwright MCP server. This runs a headless browser locally — no API costs, handles JavaScript-heavy sites.

### Install Playwright MCP Server

```bash
# Install the MCP server
npm install -g @playwright/mcp@latest

# Install browser (requires sudo for system deps)
sudo npx playwright install chrome
```

### Configure Roo Code to Use Playwright

The MCP settings file is located at:
```
~/.config/Code/User/globalStorage/rooveterinaryinc.roo-cline/settings/mcp_settings.json
```

Add the Playwright server:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "/home/YOUR_USER/.nvm/versions/node/v22.22.0/bin/npx",
      "args": ["@playwright/mcp"],
      "disabled": false,
      "env": {
        "PATH": "/home/YOUR_USER/.nvm/versions/node/v22.22.0/bin:/usr/local/bin:/usr/bin:/bin"
      }
    }
  }
}
```

Replace `YOUR_USER` with your actual username. If not using nvm, find your npx path with `which npx`.

### Reload VS Code

After configuring, reload VS Code:
- `Ctrl+Shift+P` → "Developer: Reload Window"

### Using Live Docs in Roo Code

Once configured, just paste a URL and ask about it:

```
tell me about this page https://firebase.google.com/docs

read https://react.dev/reference/react/useState and explain the API
```

Roo Code will:
1. Use the `browser_navigate` tool to open the page
2. Use `browser_snapshot` to capture the rendered content
3. Return the page content for analysis

### Available Playwright Tools

| Tool | Purpose |
|------|---------|
| `browser_navigate` | Go to a URL |
| `browser_snapshot` | Capture page content (accessibility tree) |
| `browser_screenshot` | Take visual screenshot |
| `browser_click` | Click elements |
| `browser_type` | Type into inputs |

### Tips for Reading Docs

- **Single pages work best** — don't ask it to crawl entire doc sites
- **Be specific** — "read the useState section" vs "read all of React docs"
- **JS-heavy sites work** — Playwright renders JavaScript before capturing
- **Local only** — no API costs, runs entirely on your machine

---

## 8. Vision & Multimodal (Analyzing Images Locally)

Need to analyze screenshots, mockups, design files, or any image? Use a vision-capable LLM locally — no cloud API needed.

### Install the Vision Model

```bash
ollama pull qwen2.5vl:7b
```

- Size on disk: ~5GB
- VRAM usage: ~8GB loaded
- Capabilities: Image understanding, OCR, UI analysis, design critique

### Test the Model

```bash
# Analyze a screenshot
ollama run qwen2.5vl:7b "Describe this image" /path/to/screenshot.png

# Or use a URL
ollama run qwen2.5vl:7b "What colors are used in this design?" https://example.com/mockup.png
```

### Using Vision in Roo Code

1. Switch to the vision model:
   - Open Roo Code settings (gear icon)
   - Change Model to `qwen2.5vl:7b`

2. Add images to context:
   - Drag and drop image files into the chat
   - Or use the attachment button

3. Ask about the image:
   ```
   What's the color palette in this design?
   Describe the layout and UI components
   Read the text visible in this screenshot
   Compare this mockup to the current implementation
   ```

### Using Vision in Aider

Aider doesn't natively support image input, but you can:

1. Use `ollama run` directly for image analysis:
   ```bash
   ollama run qwen2.5vl:7b "Analyze this UI screenshot and describe the layout" ./screenshot.png
   ```

2. Copy the analysis into your Aider session as context

### Vision Model Capabilities

| Task | Quality |
|------|---------|
| UI/Layout description | Excellent |
| Color identification | Excellent |
| Text/OCR reading | Good |
| Design critique | Good |
| Code from screenshot | Moderate (use for reference only) |

### When to Use Vision

- **Design implementation** — Analyze mockups before coding
- **Bug reports** — Understand screenshots of UI issues
- **UI audits** — Check accessibility, contrast, layout
- **Documentation** — Extract info from diagrams or charts
- **Competitive analysis** — Study other app interfaces

### VRAM Considerations

The vision model (8GB) can coexist with the MoE model (qwen3.5:35b-a3b at 8GB active), but not with the 27B or 32B models. When switching:

```bash
# Restart Ollama to clear VRAM before switching to vision
sudo systemctl restart ollama
ollama run qwen2.5vl:7b
```

Or let Ollama auto-switch (slower, but it handles unloading):
```bash
ollama run qwen2.5vl:7b "Analyze this" ./image.png
```

---

## 9. Hybrid API Fallback (DeepSeek V3.2)

For the hard 20% of tasks — complex multi-file architectural refactors, security-sensitive logic, very long context sessions — use a cheap cloud API instead of burning time on local inference. DeepSeek V3.2 at $0.27/million input tokens is the consensus choice.

### Set Up OpenRouter (Easiest Access to DeepSeek)
1. Create a free account at https://openrouter.ai
2. Generate an API key
3. Add credit ($10 lasts weeks of occasional hard tasks)

### Configure Roo Code for Hybrid Use

In Roo Code settings, you can switch providers per session. For heavy tasks:

1. Change API Provider to **OpenRouter**
2. Enter your OpenRouter API key
3. Set Model to `deepseek/deepseek-chat-v3-5` (DeepSeek V3.2)
4. Run the hard task
5. Switch back to Ollama for regular work

**Alternatively**, configure a second Roo Code profile:
- Profile A: Ollama / qwen3.5:27b (default, $0)
- Profile B: OpenRouter / deepseek-v3 (fallback, ~$0.01–0.05 per session)

### Configure Aider for Hybrid Use

For Aider, switch the model on the command line:

```bash
# Set API key
export OPENROUTER_API_KEY=your_key_here

# Use DeepSeek via OpenRouter for hard tasks
aider --model openrouter/deepseek/deepseek-chat-v3-5 src/complex_module.py

# Back to local for everything else
aider  # Uses your .aider.conf.yml default (Ollama)
```

### When to Switch to the Cloud Fallback

Switch when your task involves:
- Refactoring across **10+ files** simultaneously
- Security-critical code (auth, encryption, input validation)
- Context that exceeds ~80K tokens (the model starts degrading on local)
- You've gotten 2+ wrong answers from the local model on the same problem

---

## 10. Performance Tuning

### Verify GPU Is Being Used (Not CPU)

After starting a model, watch GPU utilization:
```bash
watch -n 1 nvidia-smi
```
You should see GPU utilization spike to 80–100% and VRAM usage jump to ~16–20GB when the model is running inference. If VRAM usage stays at 0 and CPU usage spikes instead, Ollama is falling back to CPU — check driver setup.

### Optimal Quantization Format

Ollama's default pulls use Q4_K_M quantization. This is the right choice for the 4090 — best quality-to-VRAM tradeoff. Do not manually pull Q2 or Q3 quantized versions to save VRAM; the quality degradation on coding tasks is significant.

If you want higher quality and your task fits:
```bash
# Pull Q5_K_M for slightly better quality (needs ~22GB for 27B)
ollama pull qwen3.5:27b-q5_K_M
```

### Context Window Settings

The 4090 with 24GB can sustain these context windows without quality loss:

| Model | Safe Context Window | Max Before Degradation |
|-------|-------------------|----------------------|
| Qwen3.5-27B (Q4_K_M) | 32K tokens | 64K tokens |
| Qwen2.5-Coder-32B (Q4_K_M) | 16K tokens | 32K tokens |
| Qwen3.5-35B-A3B | 32K tokens | 64K tokens |

In Roo Code and Aider, start with 32K and reduce if you hit OOM errors.

### Ollama Model Timeout Settings

By default Ollama unloads a model after 5 minutes of inactivity, requiring a cold-load next request. To keep it hot during a work session:

```bash
# Keep model loaded indefinitely (unload manually or on restart)
OLLAMA_KEEP_ALIVE=-1 ollama run qwen3.5:27b ""
```

Or set permanently in the service environment (add to your systemctl edit):
```ini
Environment="OLLAMA_KEEP_ALIVE=60m"
```

### Monitoring Tokens Per Second

When running inference, Ollama logs token speed. Baseline expectations on RTX 4090:

| Model | Expected Tokens/Second |
|-------|----------------------|
| Qwen3.5-27B (Q4_K_M) | 25–40 t/s |
| Qwen2.5-Coder-32B (Q4_K_M) | 15–25 t/s |
| Qwen3.5-35B-A3B (MoE Q4) | 35–55 t/s |

If you're seeing significantly lower numbers (under 10 t/s), check:
```bash
# Is anything else using the GPU?
nvidia-smi

# Is Flash Attention enabled?
sudo systemctl cat ollama | grep FLASH
```

---

## 11. Daily Workflow Guide

### The Decision Tree — Which Tool For What

```
Is this a quick question, code explanation, or single-function task?
  → Roo Code (Code mode) with qwen3.5:27b

Is this a multi-file refactor or new feature spanning several files?
  → Aider with qwen3.5:27b or qwen2.5-coder:32b

Is this system design, architecture planning, or reviewing structure?
  → Roo Code (Architect mode)

Is this debugging a specific error?
  → Roo Code (Debug mode)

Is this a complex refactor across 10+ files, or is the local model giving bad answers?
  → Switch to DeepSeek V3.2 via OpenRouter

Is this just fast autocomplete while typing?
  → Consider adding Continue.dev alongside Roo Code
    (ollama pull qwen3.5:35b-a3b for autocomplete, faster/cheaper)
```

### Starting a Work Session

```bash
# 1. Check Ollama is running
systemctl status ollama

# 2. Verify GPU is clear
nvidia-smi

# 3. Confirm model is available
ollama list

# 4. Open VS Code in your project
code /your/project

# 5. Roo Code panel will auto-connect to Ollama
```

### Token Budget Awareness

Local models don't cost money but they do cost time. Keep prompts efficient:
- Add only the files you're actively changing with `/add`
- Use `/drop` to remove files no longer needed
- Start a new session for unrelated tasks (don't let context bloat)
- In Roo Code, the token counter in the panel header shows current session usage

### Git Hygiene with Aider

Aider auto-commits every change. After a session:
```bash
# Review what aider committed
git log --oneline -10

# If you want to squash aider commits into one logical commit
git rebase -i HEAD~5  # Squash last 5 commits
```

---

## 12. Troubleshooting

### Ollama Not Using GPU
```bash
# Check Ollama logs for GPU detection
journalctl -u ollama -n 50

# Should see lines like:
# "detected GPU: NVIDIA GeForce RTX 4090"
# "VRAM available: 24564 MB"

# If it says "no GPU found":
sudo systemctl restart ollama
# Then check nvidia-smi is working
```

### Out of Memory (OOM) Errors
```bash
# Check what's currently in VRAM
nvidia-smi

# Kill any zombie Ollama processes
pkill -f ollama
sudo systemctl restart ollama

# If running qwen2.5-coder:32b, close all other GPU processes first
# It needs nearly all 24GB
```

### Model Loads But Gives Poor Responses
This usually means the context window is too large and quality is degrading. In Roo Code or Aider:
- Reduce context window from 64K to 32K
- Start a fresh session (clear accumulated context)
- Use `/drop` to remove large files not relevant to current task

### Roo Code Can't Connect to Ollama
```bash
# Verify Ollama API is accessible
curl http://localhost:11434/api/tags

# Check firewall isn't blocking localhost (unlikely but possible)
sudo ufw status

# Confirm Ollama is listening
ss -tlnp | grep 11434
```

### Aider Says "Model Not Found"
```bash
# Verify the model name exactly matches what Ollama has
ollama list

# Use the exact name shown in ollama list
aider --model ollama/qwen3.5:27b  # must match exactly

# If model name has a tag, include it
aider --model ollama/qwen3.5:27b-q4_K_M
```

### Slow First Response (Cold Start)
The first request after loading a model loads weights from disk into VRAM. On an NVMe SSD this takes 5–15 seconds. On SATA SSD, 20–40 seconds. This is normal. Subsequent requests in the same session are fast. Keep `OLLAMA_KEEP_ALIVE=60m` to avoid repeated cold starts.

### VS Code Extension Not Responding
```bash
# Restart Ollama service
sudo systemctl restart ollama

# In VS Code: Ctrl+Shift+P → "Roo Code: Reset Connection"
# Or: Close and reopen VS Code
```

---

## Quick Reference Card

| Task | Command |
|------|---------|
| Start Ollama | `sudo systemctl start ollama` |
| Check GPU status | `nvidia-smi` |
| List models | `ollama list` |
| Pull primary model | `ollama pull qwen3.5:27b` |
| Pull coder model | `ollama pull qwen2.5-coder:32b` |
| Run Aider (local) | `aider --model ollama/qwen3.5:27b` |
| Run Aider (cloud fallback) | `aider --model openrouter/deepseek/deepseek-chat-v3-5` |
| Watch GPU during inference | `watch -n 1 nvidia-smi` |
| Check Ollama logs | `journalctl -u ollama -n 50` |
| Restart Ollama | `sudo systemctl restart ollama` |
| Unload models from VRAM | `sudo systemctl restart ollama` |

---

*Stack versions this guide targets: Ollama 0.6+, Roo Code 3.x, Aider 0.77+, Ubuntu 22.04/24.04*
*Primary model: qwen3.5:27b — fallback: qwen2.5-coder:32b — cloud: deepseek-v3 via OpenRouter*
