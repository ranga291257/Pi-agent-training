# Pi Agent on Windows 11 + Local Ollama (Quick Start)

This guide is intentionally minimal: install Git for Windows, Node.js, Ollama, then connect Pi Agent to a local model.

## 1) Prerequisites

- Windows 11
- Administrator access to install apps

## 2) Install Git for Windows (bash requirement)

1. Download and install **Git for Windows**: [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Keep defaults unless your company policy requires changes.
3. Open a new PowerShell window and verify:

```powershell
git --version
```

## 3) Install Node.js 20+ (includes npm)

1. Download the **LTS** installer from [https://nodejs.org/](https://nodejs.org/)
2. Run the installer (keep defaults).
3. Open a new PowerShell window and verify:

```powershell
node --version
npm --version
```

## 4) Install Ollama on Windows

1. Download and install from [https://ollama.com/download/windows](https://ollama.com/download/windows)
2. Start Ollama once so the local service is running.
3. Verify:

```powershell
ollama --version
curl.exe -s http://localhost:11434/api/tags | ConvertFrom-Json | ConvertTo-Json -Depth 20
```

## 5) Install Pi Agent

Open **PowerShell**:

```powershell
npm install -g @mariozechner/pi-coding-agent
```

Verify:

```powershell
pi --version
```

If `pi` is not found, close/reopen PowerShell and run again.

If Ollama model list is empty, pull one model first:

```powershell
ollama pull llama3.2:latest
```

## 6) Create Pi models config

Create folder if needed:

```powershell
New-Item -ItemType Directory -Force "$HOME\.pi\agent" | Out-Null
```

Create file: `%USERPROFILE%\.pi\agent\models.json`

```json
{
  "providers": {
    "ollama": {
      "baseUrl": "http://localhost:11434/v1",
      "api": "openai-completions",
      "apiKey": "ollama",
      "compat": {
        "supportsDeveloperRole": false,
        "supportsReasoningEffort": false
      },
      "models": [
        { "id": "llama3.2:latest", "name": "Llama 3.2 (Local Ollama)" }
      ]
    }
  },
  "defaultProvider": "ollama",
  "roles": {
    "planner": "ollama/llama3.2:latest",
    "worker": "ollama/llama3.2:latest",
    "reviewer": "ollama/llama3.2:latest"
  }
}
```

## 7) Start Pi and select model

```powershell
pi
```

Inside Pi:

- Type `/model`
- Select `Llama 3.2 (Local Ollama)`

## 8) Quick test

Ask Pi a simple prompt, for example:

- "Summarize the current folder structure."

If it responds, setup is complete.

## Troubleshooting (short)

- **`pi` not recognized**: reopen terminal, check `npm -g` bin is in PATH.
- **No models in `/model`**: confirm model ID in `models.json` matches `curl .../api/tags` output exactly.
- **Connection error**: verify Ollama app/service is running and `http://localhost:11434/api/tags` works.
