# 🌱 Model Garden

An experimental setup combining LiteLLM and Open WebUI to create a unified AI gateway for testing multiple LLM providers through a single interface.

## Overview

This is a personal experiment that integrates two existing open-source projects:
- **[LiteLLM](https://github.com/BerriAI/litellm)** – Unified API gateway for 100+ LLM providers
- **[Open WebUI](https://github.com/open-webui/open-webui)** – Modern chat interface

The goal is to explore how different AI models perform through a single, consistent interface.

## What This Does

- Routes all LLM requests through LiteLLM's OpenAI-compatible proxy
- Provides a ChatGPT-style interface via Open WebUI
- Allows quick switching between providers (OpenAI, Gemini, Claude, Groq, local models)
- Enables direct comparison of model responses

## Architecture

```
┌─────────────────┐
│   Open WebUI    │  ← Your chat interface
└────────┬────────┘
         │ (talks OpenAI format)
┌────────┴────────┐
│  LiteLLM Proxy  │  ← Translates to any provider
└────┬───┬───┬────┘
     │   │   │
  OpenAI │ Ollama
      Gemini (local)
```

## Setup

### Prerequisites

- Docker and Docker Compose installed
- API keys for providers you want to test

### Steps

1. **Install LiteLLM globally or via Docker:**
```bash
pip install litellm
# or use their Docker image
```

2. **Configure your models** in `proxy_server_config.yaml`:
```yaml
model_list:
  - model_name: gpt-4o-mini
    litellm_params:
      model: openai/gpt-4o-mini
      api_key: os.environ/OPENAI_API_KEY

  - model_name: gemini-flash
    litellm_params:
      model: google/gemini-1.5-flash
      api_key: os.environ/GOOGLE_API_KEY
```

3. **Start LiteLLM:**
```bash
litellm --config proxy_server_config.yaml --port 4000
```

4. **Run Open WebUI** (pointing to your LiteLLM proxy):
```bash
docker run -d -p 3000:8080 \
  -e OPENAI_API_BASE_URL=http://localhost:4000/v1 \
  -e OPENAI_API_KEY=sk-anything \
  ghcr.io/open-webui/open-webui:main
```

5. **Access the UI** at http://localhost:3000

## Usage

1. Open http://localhost:3000 in your browser
2. Sign up/login to Open WebUI
3. Select any model from your config
4. Start chatting and comparing responses

## Why This Setup?

- **Experimentation** – Test multiple models without changing code
- **Cost comparison** – See which models give best value
- **Performance testing** – Compare response quality and speed
- **Learning** – Understand how different models handle the same prompts

## Notes

- This is a **personal experiment**, not production-ready
- Uses official LiteLLM and Open WebUI projects
- No custom code, just configuration and integration
- Modify configs to add/remove models as needed

## Credits

- [LiteLLM](https://github.com/BerriAI/litellm) by BerriAI
- [Open WebUI](https://github.com/open-webui/open-webui) by the Open WebUI team

