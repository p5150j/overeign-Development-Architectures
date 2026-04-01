# Sovereign Development Architectures

## Purpose
Documentation repository for building a zero-cost AI coding stack using local LLMs. Replaces $200/month Claude Code/Cursor subscriptions with open-source alternatives.

## Stack
- **Inference**: Ollama (local LLM server)
- **Primary Model**: Qwen3.5-27B (72.4% SWE-bench)
- **Vision Model**: Qwen2.5VL-7B (local image analysis)
- **IDE Agent**: Roo Code (VS Code extension)
- **Terminal Agent**: Aider (git-native pair programming)
- **Cloud Fallback**: OpenRouter + DeepSeek V3.2

## Hardware Target
- RTX 4090 24GB VRAM (primary)
- RTX 3060 12GB (budget tier)
- Ubuntu 22.04/24.04 LTS

## Repository Structure
```
/
├── README.md                      # Overview and quick start
├── SOVEREIGN-AI-DEV-MANIFESTO.md  # Philosophy + detailed analysis
├── AI-DEV-STACK-GUIDE.md          # Daily use reference guide
├── ubuntu_ai_dev_stack_setup.md   # Step-by-step installation
├── aider.conf.yml.example         # Aider config template
├── mcp_settings.json.example      # MCP server config template
└── ollama-override.conf           # Systemd service config
```

## Conventions
- All documentation in Markdown
- Code examples use bash fenced blocks
- Config examples use appropriate language tags (yaml, json, ini)
- Tables for comparison data
- No emojis unless explicitly requested

## Current State
- Stack fully documented and tested
- Vision model integration complete
- MCP/Playwright integration working
- All docs committed to GitHub
