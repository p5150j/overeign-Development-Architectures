# AI Development Stack Quick Reference

## Installed Components

| Component | Version | Status |
|-----------|---------|--------|
| Ollama | 0.19.0 | Running |
| NVIDIA Driver | 535 / CUDA 12.2 | Working |
| RTX 4090 | 24GB VRAM | Detected |
| VS Code | 1.113.0 | Installed |
| Roo Code | 3.51.1 | Installed |
| Aider | 0.86.2 | Installed |

## Available Models

| Model | Size | Use Case |
|-------|------|----------|
| qwen3.5:27b | 17GB | Primary - General coding & chat |
| qwen3.5:35b-a3b | 23GB | MoE efficiency model |
| qwen2.5-coder:32b | 19GB | Code-focused tasks |
| deepseek-r1:32b | 19GB | Reasoning tasks |

## Daily Workflow Decision Tree

```
Task Type?
    |
    +-- Quick code edit/fix --> Roo Code (VS Code)
    |
    +-- Multi-file refactor --> Aider (terminal)
    |
    +-- Complex reasoning --> deepseek-r1:32b
    |
    +-- General coding --> qwen3.5:27b (default)
    |
    +-- Need cloud fallback --> OpenRouter (DeepSeek V3.2)
```

## Quick Reference Commands

### Ollama
```bash
# List models
ollama list

# Run a model interactively
ollama run qwen3.5:27b

# Run specific model
ollama run qwen2.5-coder:32b

# Check service status
systemctl status ollama

# Restart service
sudo systemctl restart ollama

# View logs
journalctl -u ollama -f
```

### Aider
```bash
# Start aider in current directory (uses ~/.aider.conf.yml)
aider

# Start with specific model
aider --model ollama_chat/qwen2.5-coder:32b

# Start with specific files
aider file1.py file2.py

# Use cloud fallback (DeepSeek via OpenRouter)
aider --model openrouter/deepseek/deepseek-chat-v3-0324
```

### GPU Monitoring
```bash
# Quick GPU status
nvidia-smi

# Continuous monitoring
watch -n 1 nvidia-smi

# Check VRAM usage
nvidia-smi --query-gpu=memory.used,memory.total --format=csv
```

## Roo Code Configuration

Open VS Code settings (Ctrl+,) and configure:
- API Provider: Ollama
- Base URL: http://localhost:11434
- Model: qwen3.5:27b
- Context Window: 32768
- Max Output Tokens: 8192

For cloud fallback, add a second profile:
- API Provider: OpenRouter
- API Key: (your OpenRouter key)
- Model: deepseek/deepseek-chat-v3-0324

## Troubleshooting Checklist

### Model won't load
1. Check VRAM: `nvidia-smi` (need ~20GB free for 27b models)
2. Unload other models: Restart ollama service
3. Check service: `systemctl status ollama`

### Slow inference
1. Verify GPU is being used: `nvidia-smi` during inference
2. Check OLLAMA_FLASH_ATTENTION is enabled
3. Reduce context window if needed

### Aider connection issues
1. Verify Ollama is running: `curl http://localhost:11434/api/tags`
2. Check config: `cat ~/.aider.conf.yml`
3. Test model directly: `ollama run qwen3.5:27b "test"`

### Out of VRAM
1. Only one model loads at a time (OLLAMA_MAX_LOADED_MODELS=1)
2. Restart ollama to clear VRAM
3. Use smaller model (qwen3.5:35b-a3b uses MoE, more efficient)

## Configuration Files

| File | Purpose |
|------|---------|
| /etc/systemd/system/ollama.service.d/override.conf | Ollama performance settings |
| ~/.aider.conf.yml | Aider configuration |
| ~/.bashrc | OpenRouter API key |

## Environment Variables (set in ~/.bashrc)

```bash
OPENROUTER_API_KEY="sk-or-v1-..."  # Cloud fallback
```

Reload after changes: `source ~/.bashrc`
