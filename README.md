# Sovereign Development Architectures

**Breaking free from $200/month AI coding subscriptions with local, open-source alternatives.**

The gap between proprietary AI coding agents (Claude Code, Cursor) and open-source alternatives has shrunk from insurmountable to manageable. This repo documents a complete sovereign AI development stack that achieves **72% SWE-bench** performance at **$0/month** ongoing cost.

---

## The Stack

| Component | Choice | Why |
|-----------|--------|-----|
| **LLM** | Qwen3.5-27B | 72.4% SWE-bench, fits 24GB VRAM |
| **IDE Agent** | Roo Code | Best VS Code integration, diff-based editing |
| **Terminal Agent** | Aider | Git-native, automatic commits |
| **Inference** | Ollama | Simple local serving, one-command setup |
| **Fallback** | OpenRouter | DeepSeek V3.2 at $0.27/M tokens for hard tasks |

---

## Documentation

### [SOVEREIGN-AI-DEV-MANIFESTO.md](SOVEREIGN-AI-DEV-MANIFESTO.md)
The complete guide in three parts:
- **Part I: Why We Build** — The extraction economy, platform feudalism, and why the third path matters
- **Part II: What We're Building Against** — Benchmarks, hardware requirements, TCO analysis vs Claude Code
- **Part III: How to Build It** — Tool comparisons, model rankings, setup guides

### [AI-DEV-STACK-GUIDE.md](AI-DEV-STACK-GUIDE.md)
Quick reference for daily use:
- Available models and commands
- Workflow decision tree
- Troubleshooting checklist

### [ubuntu_ai_dev_stack_setup.md](ubuntu_ai_dev_stack_setup.md)
Original step-by-step setup documentation for Ubuntu + NVIDIA GPU environments.

### [ollama-override.conf](ollama-override.conf)
Systemd service configuration for optimal Ollama performance:
```ini
OLLAMA_NUM_PARALLEL=1
OLLAMA_MAX_LOADED_MODELS=1
OLLAMA_FLASH_ATTENTION=1
OLLAMA_GPU_OVERHEAD=512
OLLAMA_KEEP_ALIVE=60m
```

---

## Quick Start

```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Pull the primary model
ollama pull qwen3.5:27b

# Install Roo Code in VS Code
code --install-extension RooVeterinaryInc.roo-cline

# Configure: Settings → Provider: Ollama → Model: qwen3.5:27b
```

For terminal workflows:
```bash
pipx install aider-chat
aider --model ollama_chat/qwen3.5:27b
```

---

## Hardware Requirements

| Tier | GPU | Models | Context |
|------|-----|--------|---------|
| Budget | RTX 3060 12GB | Qwen3.5-35B-A3B (MoE) | 32K |
| Recommended | RTX 4090 24GB | Qwen3.5-27B, Qwen2.5-Coder-32B | 64K |
| Enthusiast | RTX 5090 32GB | All models at Q8 precision | 256K |

---

## The Economics

| Approach | Monthly Cost | Break-even vs Claude Max |
|----------|--------------|--------------------------|
| Claude Code Max 20x | $200 | — |
| Local (RTX 4090) | $0 | ~3 months |
| Hybrid (local + API fallback) | $10-30 | Immediate |

A $500 GPU pays for itself in under 3 months versus a $200/month subscription.

---

## License

Documentation is provided as-is. Referenced tools have their own licenses:
- Roo Code, Cline, Aider: Apache 2.0
- Qwen models: Apache 2.0
- Ollama: MIT
