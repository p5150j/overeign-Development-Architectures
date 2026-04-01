# Technical Context

## Models Available Locally

| Model | VRAM | Use Case |
|-------|------|----------|
| qwen3.5:27b | ~20GB | Primary coding, highest quality |
| qwen3.5:35b-a3b | ~8GB | MoE efficiency, fast inference |
| qwen2.5-coder:32b | ~20GB | Pure code generation |
| qwen2.5vl:7b | ~8GB | Vision/image analysis |

## Key Integrations

### Ollama
- Runs as systemd service
- API at http://localhost:11434
- Environment config in `/etc/systemd/system/ollama.service.d/override.conf`

### Roo Code
- VS Code extension: RooVeterinaryInc.roo-cline
- MCP config: `~/.config/Code/User/globalStorage/rooveterinaryinc.roo-cline/settings/mcp_settings.json`
- Supports Memory Bank via `.roo/memory-bank/` directory

### Aider
- Config at `~/.aider.conf.yml`
- Uses `ollama_chat/` prefix for models
- Auto-commits every change

### MCP Playwright
- Enables reading live web documentation
- Runs headless Chrome locally
- Tools: browser_navigate, browser_snapshot, browser_screenshot

## Cloud Fallback
- Provider: OpenRouter
- Model: deepseek/deepseek-chat-v3-5
- Cost: ~$0.27/M tokens
- API key in ~/.bashrc as OPENROUTER_API_KEY
