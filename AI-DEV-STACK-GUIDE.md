# AI Development Stack User Guide

A complete guide to running your sovereign AI coding stack with Ollama, Roo Code, and Aider.

---

## Table of Contents

1. [System Overview](#system-overview)
2. [Model Selection Guide](#model-selection-guide)
3. [Roo Code (VS Code)](#roo-code-vs-code)
4. [Aider (Terminal)](#aider-terminal)
5. [Ollama Management](#ollama-management)
6. [Workflow Patterns](#workflow-patterns)
7. [Reading Live Documentation (MCP)](#reading-live-documentation-mcp)
8. [Cloud Fallback](#cloud-fallback)
9. [Performance Tuning](#performance-tuning)
10. [Troubleshooting](#troubleshooting)
11. [Configuration Reference](#configuration-reference)

---

## System Overview

### Installed Components

| Component | Version | Purpose |
|-----------|---------|---------|
| Ollama | 0.19.0 | Local LLM inference server |
| Roo Code | 3.51.1 | VS Code agentic coding extension |
| Aider | 0.86.2 | Terminal-based pair programming |
| NVIDIA Driver | 535 / CUDA 12.2 | GPU acceleration |

### Hardware

| Resource | Spec | Notes |
|----------|------|-------|
| GPU | RTX 4090 24GB | Runs all models at Q4 precision |
| VRAM Available | ~23GB | After system overhead |

### Installed Models

| Model | Size | VRAM Usage | Best For |
|-------|------|------------|----------|
| `qwen3.5:27b` | 17GB | ~20GB | Primary coding - highest quality |
| `qwen3.5:35b-a3b` | 23GB | ~8GB active | MoE efficiency - fast inference |
| `qwen2.5-coder:32b` | 19GB | ~20GB | Pure code generation |
| `deepseek-r1:32b` | 19GB | ~20GB | Complex reasoning & debugging |

---

## Model Selection Guide

### Decision Tree

```
What are you doing?
│
├─► Writing new code from scratch
│   └─► qwen3.5:27b (best overall quality)
│
├─► Quick edits / small fixes
│   └─► qwen3.5:35b-a3b (faster, lower VRAM)
│
├─► Pure code completion / generation
│   └─► qwen2.5-coder:32b (code specialist)
│
├─► Debugging complex logic
│   └─► deepseek-r1:32b (reasoning focused)
│
├─► Multi-file refactoring
│   └─► qwen3.5:27b + Aider (git safety)
│
└─► Task exceeds local capability
    └─► OpenRouter fallback (DeepSeek V3.2)
```

### When to Use Each Model

**qwen3.5:27b** (Default)
- General-purpose coding tasks
- Writing functions, classes, modules
- Code review and suggestions
- Documentation generation
- 72.4% SWE-bench - closest to Claude quality

**qwen3.5:35b-a3b** (Speed)
- Quick iterations
- Simple edits and fixes
- When you need faster responses
- Running on lower VRAM systems
- Only activates 3B parameters per token

**qwen2.5-coder:32b** (Code Specialist)
- Pure code generation
- Algorithm implementation
- Code translation between languages
- Most battle-tested with Aider

**deepseek-r1:32b** (Reasoning)
- Complex debugging sessions
- Architecture decisions
- Understanding legacy code
- When you need "thinking" through problems

---

## Roo Code (VS Code)

### Opening Roo Code

- Click the Roo icon in the sidebar, or
- `Ctrl+Shift+P` → "Roo Code: Open Panel"

### Configuration

Settings are accessed via the gear icon in the Roo Code panel:

```
API Provider: Ollama
Base URL: http://localhost:11434
Model ID: qwen3.5:27b
```

Optional settings:
```
Context Window: 32768
Max Output Tokens: 8192
Enable Diff Editing: Yes (saves tokens)
```

### Effective Prompting

**Be specific about scope:**
```
# Good
"Add input validation to the handleSubmit function in src/forms/contact.ts"

# Vague
"Add validation"
```

**Reference files explicitly:**
```
"Looking at README.md, update the installation section to include the new Docker option"
```

**Use modes effectively:**
- **Code Mode**: Direct implementation
- **Architect Mode**: Planning and design
- **Debug Mode**: Finding and fixing issues

### Roo Code Shortcuts

| Action | Method |
|--------|--------|
| Add file to context | Drag file into chat or @filename |
| Add folder | @foldername |
| Reference terminal | @terminal |
| Clear context | Click "Clear" or start new chat |

### Memory Bank

Roo Code can persist context across sessions:
1. Open settings → Memory Bank
2. Save key project context
3. Auto-loads on next session

---

## Aider (Terminal)

### Basic Usage

```bash
# Start in current directory
cd ~/your-project
aider

# Start with specific files
aider src/main.py src/utils.py

# Read-only file (context but no edits)
aider --read docs/api.md src/main.py
```

### In-Chat Commands

| Command | Action |
|---------|--------|
| `/add <file>` | Add file to chat |
| `/drop <file>` | Remove file from chat |
| `/ls` | List files in chat |
| `/diff` | Show pending changes |
| `/undo` | Undo last change |
| `/clear` | Clear chat history |
| `/help` | Show all commands |
| `/quit` | Exit aider |

### Switching Models Mid-Session

```bash
# Start with default (qwen3.5:27b)
aider

# In chat, switch to reasoning model
/model ollama_chat/deepseek-r1:32b

# Switch back
/model ollama_chat/qwen3.5:27b
```

### Git Integration

Aider auto-commits every change. Control this with:

```bash
# Disable auto-commits
aider --no-auto-commits

# In chat, manual commit
/commit

# See git log
/git log --oneline -5
```

### Effective Aider Workflows

**Multi-file refactoring:**
```bash
# Add all relevant files first
aider src/models/*.py src/api/*.py

# Then describe the refactor
"Rename the User class to Account across all files, updating all references"
```

**Test-driven development:**
```bash
aider tests/test_auth.py src/auth.py

"Write a failing test for password reset, then implement the feature to pass it"
```

**Code review:**
```bash
aider --read src/new_feature.py

"Review this code for security issues and performance problems"
```

---

## Ollama Management

### Essential Commands

```bash
# Service management
systemctl status ollama          # Check status
sudo systemctl restart ollama    # Restart (clears VRAM)
sudo systemctl stop ollama       # Stop service
journalctl -u ollama -f          # Follow logs

# Model management
ollama list                      # List installed models
ollama pull <model>              # Download model
ollama rm <model>                # Delete model
ollama show <model>              # Model details

# Direct interaction
ollama run qwen3.5:27b           # Interactive chat
ollama run qwen3.5:27b "prompt"  # One-shot query
```

### API Endpoints

```bash
# Health check
curl http://localhost:11434/

# List models
curl http://localhost:11434/api/tags

# Generate (streaming)
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3.5:27b",
  "prompt": "Hello"
}'
```

### Managing VRAM

Only one model fits in VRAM at a time with these settings. To switch models:

```bash
# Option 1: Just use the new model (auto-unloads old)
ollama run qwen2.5-coder:32b

# Option 2: Restart service (immediate VRAM clear)
sudo systemctl restart ollama
```

Check current VRAM usage:
```bash
nvidia-smi --query-gpu=memory.used,memory.total,memory.free --format=csv
```

---

## Workflow Patterns

### Pattern 1: Quick Fix (Roo Code)

1. Open file in VS Code
2. Open Roo Code panel
3. Describe the fix: "Fix the null pointer exception on line 42"
4. Review diff → Accept

**Best for:** Single-file fixes, small features, quick iterations

### Pattern 2: Feature Development (Roo Code + Terminal)

1. Start in Roo Code Architect mode
2. Describe the feature, get a plan
3. Switch to Code mode, implement step by step
4. Use terminal for tests: `npm test`
5. Iterate until tests pass

**Best for:** New features, structured development

### Pattern 3: Multi-File Refactor (Aider)

```bash
# Add all affected files
aider src/**/*.py

# Describe the refactor
"Migrate from requests to httpx across all files, updating async patterns"

# Aider edits all files, commits each change
# Review with /diff, undo with /undo if needed
```

**Best for:** Codebase-wide changes, renames, migrations

### Pattern 4: Debug Session (Aider + deepseek-r1)

```bash
aider --model ollama_chat/deepseek-r1:32b src/broken.py

"This function returns wrong results for edge cases.
Here's a failing test case: [paste test]
Think through the logic step by step and find the bug."
```

**Best for:** Complex bugs, logic errors, understanding unfamiliar code

### Pattern 5: Hybrid (Local + Cloud)

When local models struggle:

```bash
# Try local first
aider src/complex_feature.py

# If quality is insufficient, switch to cloud
aider --model openrouter/deepseek/deepseek-chat-v3-0324 src/complex_feature.py
```

**Best for:** Complex architectural work, security-critical code

---

## Reading Live Documentation (MCP)

Roo Code can read external documentation, API references, and web pages using the Playwright MCP server. This runs a headless browser locally — no API costs, handles JavaScript-heavy sites.

### How It Works

The Playwright MCP server is configured in:
```
~/.config/Code/User/globalStorage/rooveterinaryinc.roo-cline/settings/mcp_settings.json
```

When you paste a URL, Roo Code uses these tools:

| Tool | Purpose |
|------|---------|
| `browser_navigate` | Open the URL |
| `browser_snapshot` | Capture page content (accessibility tree) |
| `browser_screenshot` | Take visual screenshot if needed |
| `browser_click` | Interact with page elements |

### Usage Examples

Just paste a URL and ask:

```
tell me about this page https://firebase.google.com/docs

read https://react.dev/reference/react/useState and explain the API

summarize the authentication section at https://docs.github.com/en/authentication
```

Roo Code will:
1. Navigate to the page
2. Wait for JavaScript to render
3. Capture the content
4. Analyze and respond

### Tips for Reading Docs

- **Single pages work best** — don't ask it to crawl entire sites
- **Be specific** — "read the useState section" vs "read all of React"
- **JS-heavy sites work** — Playwright renders JavaScript before capturing
- **Approve tool use** — click "Run" when Roo asks to use browser tools

### Troubleshooting MCP

If Playwright isn't working:

```bash
# Verify Chrome is installed for Playwright
npx playwright install chrome

# Check MCP config
cat ~/.config/Code/User/globalStorage/rooveterinaryinc.roo-cline/settings/mcp_settings.json

# Reload VS Code
# Ctrl+Shift+P → "Developer: Reload Window"
```

---

## Cloud Fallback

### When to Use Cloud

- Complex multi-file changes that local models fumble
- Security-sensitive code requiring higher accuracy
- When you need 80%+ SWE-bench quality
- Long context windows (>64K tokens)

### OpenRouter Setup

Your API key is in `~/.bashrc`:
```bash
export OPENROUTER_API_KEY="sk-or-v1-..."
```

Reload: `source ~/.bashrc`

### Using Cloud with Aider

```bash
# DeepSeek V3.2 - Best value ($0.27/M tokens)
aider --model openrouter/deepseek/deepseek-chat-v3-0324

# Claude (when you need the best)
aider --model openrouter/anthropic/claude-sonnet-4
```

### Using Cloud with Roo Code

1. Open Roo Code settings (gear icon)
2. Add new profile: "Cloud Fallback"
3. Configure:
   - API Provider: OpenRouter
   - API Key: (paste your key)
   - Model: `deepseek/deepseek-chat-v3-0324`
4. Switch profiles via dropdown when needed

### Cost Monitoring

Track spending at: https://openrouter.ai/account

Typical usage:
- Light day: $0.50-2
- Heavy day: $5-10
- Still 90%+ cheaper than Claude Max

---

## Performance Tuning

### Ollama Environment Variables

Current settings in `/etc/systemd/system/ollama.service.d/override.conf`:

```ini
OLLAMA_NUM_PARALLEL=1        # One request at a time
OLLAMA_MAX_LOADED_MODELS=1   # One model in VRAM
OLLAMA_FLASH_ATTENTION=1     # Faster attention computation
OLLAMA_GPU_OVERHEAD=512      # Reserve 512MB for system
OLLAMA_KEEP_ALIVE=60m        # Keep model loaded 60 min
```

### Adjusting for Different Workloads

**Maximum quality (current setup):**
```ini
OLLAMA_NUM_PARALLEL=1
OLLAMA_MAX_LOADED_MODELS=1
```

**Faster model switching:**
```ini
OLLAMA_KEEP_ALIVE=5m    # Unload faster when idle
```

**If you add more VRAM (multi-GPU):**
```ini
OLLAMA_NUM_PARALLEL=2
OLLAMA_MAX_LOADED_MODELS=2
```

Apply changes:
```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

### Context Window Optimization

| Model | Max Context | Recommended | Notes |
|-------|-------------|-------------|-------|
| qwen3.5:27b | 128K | 32K-64K | Quality degrades past 64K |
| qwen3.5:35b-a3b | 128K | 32K | MoE works best with shorter context |
| qwen2.5-coder:32b | 128K | 32K | Sweet spot for code |

Set in Roo Code settings or aider:
```bash
aider --map-tokens 4096  # Increase repo map size
```

---

## Troubleshooting

### Model Won't Load

```bash
# Check VRAM
nvidia-smi

# If VRAM full, restart Ollama
sudo systemctl restart ollama

# Check Ollama logs
journalctl -u ollama -n 50

# Verify model exists
ollama list
```

### Slow Inference

```bash
# Verify GPU is being used (should show process)
nvidia-smi

# Check flash attention is enabled
cat /etc/systemd/system/ollama.service.d/override.conf | grep FLASH

# Test raw speed
time ollama run qwen3.5:27b "Say hello" --verbose
```

Expected: 20-40 tokens/second on RTX 4090

### Aider Can't Connect

```bash
# Test Ollama API
curl http://localhost:11434/api/tags

# Check aider config
cat ~/.aider.conf.yml

# Run with verbose output
aider --verbose
```

### Roo Code Not Responding

1. Check Ollama is running: `systemctl status ollama`
2. Check model is loaded: `nvidia-smi` (should show ~20GB used)
3. Restart VS Code
4. Check Roo Code output panel for errors

### Out of Memory Errors

```bash
# Check what's using VRAM
nvidia-smi

# Kill other GPU processes if needed
# Then restart Ollama
sudo systemctl restart ollama

# Use smaller model
ollama run qwen3.5:35b-a3b  # Only 8GB active
```

### Model Gives Poor Results

1. Try a different model for the task (see Model Selection Guide)
2. Reduce context - too much context degrades quality
3. Be more specific in prompts
4. For complex tasks, use cloud fallback

---

## Configuration Reference

### File Locations

| File | Purpose |
|------|---------|
| `/etc/systemd/system/ollama.service.d/override.conf` | Ollama service settings |
| `~/.aider.conf.yml` | Aider defaults |
| `~/.bashrc` | Environment variables (API keys) |
| `~/.config/Code/User/settings.json` | VS Code settings |

### Aider Configuration (~/.aider.conf.yml)

```yaml
model: ollama_chat/qwen3.5:27b
weak-model: ollama_chat/qwen3.5:35b-a3b
editor-model: ollama_chat/qwen2.5-coder:32b
edit-format: diff
auto-commits: true
pretty: true
stream: true
map-tokens: 2048
map-refresh: auto
dark-mode: true
```

### Environment Variables

```bash
# In ~/.bashrc
export OPENROUTER_API_KEY="sk-or-v1-..."  # Cloud fallback
export OLLAMA_HOST="http://localhost:11434"  # Optional
```

### Useful Aliases

Add to `~/.bashrc`:

```bash
# Quick model switching
alias aid='aider'
alias aid-reason='aider --model ollama_chat/deepseek-r1:32b'
alias aid-code='aider --model ollama_chat/qwen2.5-coder:32b'
alias aid-cloud='aider --model openrouter/deepseek/deepseek-chat-v3-0324'

# Ollama shortcuts
alias oll='ollama list'
alias ollr='sudo systemctl restart ollama'
alias olls='systemctl status ollama'

# GPU monitoring
alias gpu='nvidia-smi'
alias gpuw='watch -n 1 nvidia-smi'
```

Reload: `source ~/.bashrc`

---

## Quick Reference Card

### Daily Commands

```bash
# Start coding session
cd ~/project && aider           # Terminal
code ~/project                  # VS Code + Roo Code

# Check system
ollama list                     # Models
nvidia-smi                      # GPU
systemctl status ollama         # Service

# Fix issues
sudo systemctl restart ollama   # Clear VRAM / restart
source ~/.bashrc                # Reload env vars
```

### Model Quick Switch

| Need | Model | Command |
|------|-------|---------|
| Quality | qwen3.5:27b | (default) |
| Speed | qwen3.5:35b-a3b | `/model ollama_chat/qwen3.5:35b-a3b` |
| Code | qwen2.5-coder:32b | `/model ollama_chat/qwen2.5-coder:32b` |
| Reasoning | deepseek-r1:32b | `/model ollama_chat/deepseek-r1:32b` |
| Cloud | DeepSeek V3.2 | `--model openrouter/deepseek/deepseek-chat-v3-0324` |
