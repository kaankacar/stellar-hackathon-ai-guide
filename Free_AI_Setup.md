# Stellar Development Foundation Hackathon: AI Tools Guide
## Using Open-Source AI Models for Coding

**Prepared for: SDF Hackathon, Mexico**
**Date: March 2026**

---

## Table of Contents

1. [Introduction](#introduction)
2. [Top Open-Source Coding Models on Hugging Face](#top-models)
3. [How to Use a Local Model with Claude Code](#claude-code-guide)
4. [Running Your Model on a VPS](#vps-guide)
5. [Free AI Coding Alternatives](#free-alternatives)
6. [About Stella: Stellar's AI Assistant](#stella)

---

## 1. Introduction

As a hacker at this event, you have several options when it comes to AI-powered coding assistance:

- **Paid plans**: If you already have a subscription to Claude, ChatGPT, GitHub Copilot, or another AI service, you can continue using those normally.
- **Open-source local models**: You can download a free, open-source AI model and run it on your own computer, with no subscription required and no data leaving your machine.
- **Cloud VPS**: If your laptop doesn't have enough power to run a large model, you can rent a GPU server for a few dollars and run the model there.
- **Free tiers**: Tools like Cursor offer free plans with access to capable AI models.

This guide focuses on helping you get set up with open-source models from Hugging Face (the world's largest platform for sharing and downloading AI models) so you can code with AI entirely for free.

---

## 2. Top Open-Source Coding Models on Hugging Face

Hugging Face (https://huggingface.co) is the central hub for open-source AI models. Below are the best coding-focused models available, ranked by overall capability and ease of local deployment.

---

### Model 1: Qwen2.5-Coder (by Alibaba / Qwen Team)

**What it is:**
Qwen2.5-Coder is a family of open-source code-specialized large language models developed by Alibaba's Qwen team. It is trained on 5.5 trillion tokens of code-related data, covering over 92 programming languages. It excels at code generation, completion, debugging, and explanation.

**What it does:**
- Generates complete functions and classes from natural language descriptions
- Debugs and fixes broken code
- Explains existing code in plain English
- Supports Solidity, Rust, Python, JavaScript, TypeScript, Go, and many more languages relevant to blockchain development

**Available sizes:**
| Model | Best For | Notes |
|-------|----------|-------|
| Qwen2.5-Coder-1.5B | Very low-end hardware | Lightweight, fast |
| Qwen2.5-Coder-7B | Most laptops with a decent GPU | Best balance |
| Qwen2.5-Coder-32B | High-end machines or VPS | Near state-of-the-art |

**System Requirements:**

| Model Size | Minimum VRAM (GPU) | Recommended RAM (CPU-only) |
|---|---|---|
| 1.5B | 4 GB VRAM | 8 GB RAM |
| 7B | 8-10 GB VRAM | 16 GB RAM |
| 32B | 24 GB VRAM | 64 GB RAM |

> Tip: Quantized (compressed) versions reduce memory needs by 50-75% with minimal quality loss. Always look for GGUF or AWQ versions if your hardware is limited.

**Hugging Face page:** https://huggingface.co/Qwen/Qwen2.5-Coder-32B-Instruct

**How to install (using Ollama - recommended method):**

```bash
# Step 1: Install Ollama (see Section 3 for full Ollama setup)
# Step 2: Pull the model
ollama pull qwen2.5-coder:7b

# Or for the larger version:
ollama pull qwen2.5-coder:32b

# Step 3: Run it interactively
ollama run qwen2.5-coder:7b
```

---

### Model 2: DeepSeek-Coder-V2 (by DeepSeek AI)

**What it is:**
DeepSeek-Coder-V2 is a powerful open-source coding model from the Chinese AI lab DeepSeek. It is a Mixture-of-Experts (MoE) model with 236 billion total parameters (but only 21 billion active at a time), achieving performance comparable to GPT-4o on coding benchmarks while remaining fully open and free.

**What it does:**
- Extremely strong at competitive programming problems
- Handles multi-file code understanding
- Supports 338 programming languages
- Especially strong at math and algorithm problems

**Available sizes:**
| Model | Notes |
|-------|-------|
| DeepSeek-Coder-V2-Lite (16B active) | Suitable for local machines |
| DeepSeek-Coder-V2 (21B active / 236B total) | Requires a VPS or multi-GPU server |

**System Requirements:**

| Model | Minimum VRAM | Notes |
|-------|-------------|-------|
| DeepSeek-Coder-V2-Lite | 16-24 GB VRAM | RTX 3090 or RTX 4090 |
| DeepSeek-Coder-V2 | 80+ GB VRAM | Requires A100/H100 or multi-GPU |

**Hugging Face page:** https://huggingface.co/deepseek-ai/DeepSeek-Coder-V2-Instruct

**How to install:**

```bash
# Install via Ollama
ollama pull deepseek-coder-v2

# Run it
ollama run deepseek-coder-v2
```

---

### Model 3: Codestral (by Mistral AI)

**What it is:**
Codestral is Mistral AI's dedicated code model. It has 22 billion parameters and was trained specifically for code generation and completion. It supports a very long context window (32,000 tokens), which means it can understand and work with large codebases at once.

**What it does:**
- Autocompletes code mid-function (fill-in-the-middle capability)
- Generates code in 80+ programming languages
- Works well as an inline coding assistant (similar to GitHub Copilot)
- Strong performance on Solidity and smart contract code

**System Requirements:**

| Configuration | Requirement |
|---|---|
| Minimum VRAM (full precision) | 44 GB VRAM |
| Recommended VRAM (quantized) | 16-24 GB VRAM |
| CPU-only (quantized) | 32-64 GB RAM |

**Hugging Face page:** https://huggingface.co/mistralai/Codestral-22B-v0.1

**How to install:**

```bash
# Install via Ollama
ollama pull codestral

# Run it
ollama run codestral
```

> Note: Codestral has a non-commercial license for local use. Check Mistral's license terms for commercial hackathon projects.

---

### Model 4: StarCoder2 (by BigCode / Hugging Face)

**What it is:**
StarCoder2 is a family of open-source code models created by the BigCode project, a collaboration between Hugging Face and ServiceNow. It was trained transparently on permissively licensed code from GitHub, making it one of the most ethically sourced coding models available. It supports 600+ programming languages.

**What it does:**
- Code generation and completion
- Code explanation and documentation
- Supports an exceptionally wide range of programming languages
- Well-suited for open-source and academic projects

**Available sizes:**
| Model | Notes |
|-------|-------|
| StarCoder2-3B | Runs on almost any modern laptop |
| StarCoder2-7B | Best mid-range option |
| StarCoder2-15B | Strongest in the family |

**System Requirements:**

| Model | Minimum VRAM | CPU-only RAM |
|-------|-------------|--------------|
| 3B | 6 GB VRAM | 8 GB RAM |
| 7B | 14 GB VRAM | 16 GB RAM |
| 15B | 30 GB VRAM | 32 GB RAM |

**Hugging Face page:** https://huggingface.co/bigcode/starcoder2-15b

**How to install:**

```bash
# Install via Ollama
ollama pull starcoder2:7b

# Run it
ollama run starcoder2:7b
```

---

### Model 5: Llama 3.1 / 3.3 (by Meta)

**What it is:**
Meta's Llama 3 family are general-purpose open-source models that perform very well at coding tasks despite not being exclusively code-focused. Llama 3.1 (405B) and Llama 3.3 (70B) are among the strongest open-source models available for any task, including code.

**What it does:**
- Strong general reasoning that translates to better code quality
- Excellent at explaining complex concepts and architectural decisions
- Great for planning your project structure, writing tests, and debugging
- Recommended for use with Claude Code due to best tool-calling compatibility

**Available sizes:**
| Model | Notes |
|-------|-------|
| Llama 3.2 3B | Very fast, runs on any laptop |
| Llama 3.1 8B | Great for everyday coding tasks |
| Llama 3.3 70B | Near-GPT-4 quality, needs good hardware |

**System Requirements:**

| Model | Minimum VRAM | CPU-only RAM |
|-------|-------------|--------------|
| 3B | 4 GB VRAM | 8 GB RAM |
| 8B | 10 GB VRAM | 16 GB RAM |
| 70B | 48 GB VRAM | 128 GB RAM |

**Hugging Face page:** https://huggingface.co/meta-llama/Llama-3.3-70B-Instruct

**How to install:**

```bash
# Install via Ollama
ollama pull llama3.1:8b

# Or the larger version
ollama pull llama3.3:70b

# Run it
ollama run llama3.1:8b
```

---

### Model 6: Phi-4 (by Microsoft)

**What it is:**
Microsoft's Phi-4 is a small but surprisingly capable model with only 14 billion parameters. It punches well above its weight class for coding and reasoning tasks, making it ideal for hackers with limited hardware.

**What it does:**
- Strong mathematical and logical reasoning
- Good code generation for Python, JavaScript, and TypeScript
- Very fast inference speeds due to small size
- Fits easily on most gaming laptops with a GPU

**System Requirements:**

| Configuration | Requirement |
|---|---|
| Minimum VRAM | 8 GB VRAM |
| Recommended VRAM | 12-16 GB VRAM |
| CPU-only RAM | 16 GB RAM |

**Hugging Face page:** https://huggingface.co/microsoft/phi-4

**How to install:**

```bash
# Install via Ollama
ollama pull phi4

# Run it
ollama run phi4
```

---

### Quick Comparison Summary

| Model | Size | Best For | Min. VRAM |
|-------|------|----------|-----------|
| Qwen2.5-Coder-7B | 7B | Best all-rounder for coding | 8 GB |
| DeepSeek-Coder-V2-Lite | 16B active | Competitive coding, algorithms | 16 GB |
| Codestral | 22B | Inline completion, autocomplete | 16 GB (quantized) |
| StarCoder2-7B | 7B | Open-source friendly, broad language support | 14 GB |
| Llama 3.1 8B | 8B | General reasoning + coding, Claude Code | 10 GB |
| Phi-4 | 14B | Low-end hardware, fast responses | 8 GB |

---

## 3. How to Use a Local Model with Claude Code

Claude Code is the AI coding assistant you can run in your terminal. By default it uses Anthropic's Claude models in the cloud, but it can be pointed to a locally running open-source model instead, meaning you can use Claude Code completely free with no API credits required.

This works through **Ollama**, a tool that runs AI models locally and exposes them through an API that Claude Code can talk to.

---

### Step 1: Install Ollama

Ollama is available for macOS, Linux, and Windows.

**macOS:**
```bash
brew install ollama
```

Or download the installer from: https://ollama.com/download

**Linux:**
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

**Windows:**
Download the installer from: https://ollama.com/download/windows

---

### Step 2: Start the Ollama Server

```bash
ollama serve
```

This starts Ollama in the background, listening on port 11434. Keep this terminal window open, or run it as a background service.

---

### Step 3: Pull Your Chosen Model

Download the model you want to use. For most hackers, we recommend starting with Qwen2.5-Coder or Llama 3.1:

```bash
# Recommended for coding (requires ~8 GB VRAM or 16 GB RAM)
ollama pull qwen2.5-coder:7b

# Recommended for Claude Code compatibility (best tool-calling)
ollama pull llama3.1:8b

# Lightweight option for low-end hardware
ollama pull phi4
```

You can check what models you have downloaded:
```bash
ollama list
```

---

### Step 4: Install Claude Code

If you haven't already installed Claude Code:

```bash
npm install -g @anthropic-ai/claude-code
```

---

### Step 5: Connect Claude Code to Your Local Model

Set the following environment variables to tell Claude Code to use your local Ollama model instead of Anthropic's servers:

**macOS / Linux (Terminal):**
```bash
export ANTHROPIC_BASE_URL="http://localhost:11434"
export ANTHROPIC_AUTH_TOKEN="ollama"
export ANTHROPIC_API_KEY=""
```

**Windows (Command Prompt):**
```cmd
set ANTHROPIC_BASE_URL=http://localhost:11434
set ANTHROPIC_AUTH_TOKEN=ollama
set ANTHROPIC_API_KEY=
```

**Windows (PowerShell):**
```powershell
$env:ANTHROPIC_BASE_URL = "http://localhost:11434"
$env:ANTHROPIC_AUTH_TOKEN = "ollama"
$env:ANTHROPIC_API_KEY = ""
```

---

### Step 6: Launch Claude Code with Your Local Model

```bash
# Launch Claude Code pointing to your local Ollama model
claude --model qwen2.5-coder:7b

# Or with Llama:
claude --model llama3.1:8b
```

You can also use the quick launch command that Ollama provides:
```bash
ollama launch claude
```

---

### Step 7: Increase Context Window (Important!)

Claude Code works best with a large context window. Ollama defaults may be too small. Set it to at least 64,000 tokens:

```bash
# Set context window when running a model
OLLAMA_NUM_CTX=64000 ollama run qwen2.5-coder:7b
```

Or add to your Ollama model configuration permanently:
```bash
# Create a custom Modelfile
cat > Modelfile << 'EOF'
FROM qwen2.5-coder:7b
PARAMETER num_ctx 65536
EOF

ollama create my-coder -f Modelfile
claude --model my-coder
```

---

### Step 8: Make It Permanent (Optional)

To avoid setting environment variables every session, add them to your shell profile:

**macOS / Linux** - add to `~/.zshrc` or `~/.bashrc`:
```bash
export ANTHROPIC_BASE_URL="http://localhost:11434"
export ANTHROPIC_AUTH_TOKEN="ollama"
export ANTHROPIC_API_KEY=""
```

Then reload:
```bash
source ~/.zshrc
```

---

### Tips for Best Results with Local Models

- **Smaller models are faster but less accurate.** Start with a 7B model and upgrade if you need better results.
- **Quantized models (Q4, Q8)** use much less memory with only a small drop in quality. Ollama downloads quantized versions automatically.
- **If tool calls fail**, try switching to `llama3.1:8b`, which has the best tool-calling compatibility with Claude Code.
- **Close other heavy applications** while running local models to free up RAM and VRAM.

---

## 4. Running Your Model on a VPS

If your laptop doesn't have enough GPU power to run a capable model locally, you can rent a cloud server (VPS) with a GPU for just a few dollars and run the model there. You then connect to it from your laptop as if it were running locally.

---

### Recommended VPS Providers for LLMs

| Provider | Starting Price | Best For | URL |
|----------|---------------|----------|-----|
| RunPod | ~$0.20/hr (RTX 4090) | Best value for hackers | https://runpod.io |
| Vast.ai | ~$0.15/hr | Cheapest option, peer marketplace | https://vast.ai |
| Lambda Labs | ~$1.10/hr (A100) | Reliable, easy to use | https://lambdalabs.com |
| Paperspace | ~$0.45/hr (A4000) | Good free tier available | https://paperspace.com |
| Hostinger VPS | From $5/mo (CPU) | Budget CPU-only option | https://hostinger.com |
| Contabo VPS | From $5/mo (CPU) | Very affordable CPU servers | https://contabo.com |

> For a hackathon, **RunPod** or **Vast.ai** are recommended because you only pay for the time you use (hourly billing).

---

### Step-by-Step: Setting Up a RunPod GPU Server

**Step 1: Create an account**

Go to https://runpod.io and sign up. Add a payment method (credit card or crypto accepted). You only need ~$10-20 for a full hackathon weekend.

**Step 2: Create a new Pod**

1. Click "Pods" in the left sidebar
2. Click "+ Deploy"
3. Select a GPU:
   - **RTX 4090 (24 GB VRAM)**: best value, runs 7B-32B models
   - **RTX 3090 (24 GB VRAM)**: slightly cheaper
   - **A100 (80 GB VRAM)**: for 70B models, more expensive
4. For the template/container, select **"RunPod PyTorch"** or search for "Ollama"
5. Set disk storage to at least **50 GB** (models are large files)
6. Click "Deploy"

**Step 3: Connect to your server**

Once the pod is running, click "Connect" and choose "SSH Terminal". You'll get a command like:

```bash
ssh root@<your-pod-ip> -p <port> -i ~/.ssh/id_rsa
```

Run this in your local terminal.

**Step 4: Install Ollama on the server**

```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama serve &
```

**Step 5: Download your model**

```bash
ollama pull qwen2.5-coder:7b
# or
ollama pull llama3.1:8b
```

**Step 6: Expose Ollama to your local machine**

On your VPS, Ollama runs on port 11434. Forward this port to your local machine using SSH tunneling:

```bash
# Run this on your LOCAL machine
ssh -L 11434:localhost:11434 root@<your-pod-ip> -p <port>
```

Keep this terminal open. Now your local Claude Code will see the VPS model as if it were running on your laptop.

**Step 7: Connect Claude Code (same as local setup)**

```bash
export ANTHROPIC_BASE_URL="http://localhost:11434"
export ANTHROPIC_AUTH_TOKEN="ollama"
export ANTHROPIC_API_KEY=""
claude --model qwen2.5-coder:7b
```

**Step 8: Stop your pod when done**

To avoid charges, go to the RunPod dashboard and stop or terminate your pod when you're not using it.

---

### Step-by-Step: Setting Up on Vast.ai (Budget Option)

**Step 1:** Sign up at https://vast.ai and add credit ($5-10 is enough for a weekend)

**Step 2:** Go to "Search" and filter by:
- GPU: RTX 3090 or RTX 4090
- VRAM: at least 16 GB
- Sort by: Price (cheapest first)

**Step 3:** Click "Rent" on your chosen machine and select the **"PyTorch"** image

**Step 4:** Once running, open the SSH connection and follow the same Ollama installation steps as above (Steps 4-7 from RunPod guide)

---

### Which GPU to Choose?

| GPU | VRAM | Models it can run | Approx. Cost |
|-----|------|-------------------|-------------|
| RTX 3060 / 4060 | 12 GB | Up to 7B (quantized) | ~$0.10-0.20/hr |
| RTX 3090 / 4090 | 24 GB | Up to 32B (quantized) | ~$0.20-0.50/hr |
| A100 40 GB | 40 GB | Up to 70B (quantized) | ~$1.00-1.50/hr |
| A100 80 GB | 80 GB | 70B+ full precision | ~$1.50-2.50/hr |
| H100 | 80 GB | Largest models | ~$2.50-4.00/hr |

---

## 5. Free AI Coding Alternatives

If you don't want to set up a local model or a VPS, here are free (or freemium) AI coding tools you can use directly:

---

### Cursor (https://cursor.com)

Cursor is a code editor (based on VS Code) with built-in AI coding assistance.

**Free plan includes:**
- 2,000 code completions per month
- Limited access to GPT-4o and Claude Sonnet
- Unlimited access to cursor-small (a fast, lightweight model)

**How to get started:**
1. Download Cursor from https://cursor.com
2. Install it like any application
3. Open your project folder
4. Use `Ctrl+K` (or `Cmd+K` on Mac) to ask AI to write or edit code
5. Use `Ctrl+L` to open the AI chat sidebar

---

### GitHub Copilot Free Tier (https://github.com/features/copilot)

GitHub Copilot now has a free tier:
- 2,000 code completions per month
- 50 chat messages per month
- Powered by Claude Sonnet and GPT-4o

**How to get started:**
1. Go to GitHub Settings > Copilot
2. Enable the free plan
3. Install the Copilot extension in VS Code
4. Start coding; suggestions appear automatically

---

### Google AI Studio (https://aistudio.google.com)

- Free access to Gemini 2.0 Flash (very capable model)
- Large context window (1 million tokens)
- Great for pasting large amounts of code and getting explanations
- No subscription required, just a Google account

---

### OpenRouter (https://openrouter.ai)

OpenRouter provides access to many AI models through a single API. Several models are available for free or at very low cost. This can also be used as a backend for Claude Code:

```bash
export ANTHROPIC_BASE_URL="https://openrouter.ai/api/v1"
export ANTHROPIC_API_KEY="your-openrouter-key"
```

---

## 6. About Stella: Stellar's AI Assistant

**What is Stella?**

Stella is the official AI assistant developed by the Stellar Development Foundation (SDF) and available on the Stellar developer documentation website at https://developers.stellar.org.

Stella is specifically designed to help developers who are building on the Stellar blockchain network. It is currently in beta and was created to make Stellar development faster and more accessible, especially for developers who are new to the ecosystem.

**What can Stella do?**

- Answer questions about the Stellar network, protocols, and SDKs
- Help you understand Stellar Consensus Protocol (SCP)
- Explain Soroban smart contracts (Stellar's smart contract platform built on Rust and WebAssembly)
- Guide you through the Stellar documentation
- Answer questions about Stellar assets, accounts, transactions, and fees
- Help debug Stellar-specific code and API calls

**Where to find Stella:**

- Look for the yellow chat icon at the bottom-right corner of https://developers.stellar.org
- Join the **#stella-help** channel in the Stellar Developer Discord server

**What Stella knows:**

Stella draws from a curated knowledge base that includes:
- Stellar's official documentation and developer guides
- Stack Overflow answers related to Stellar
- The SCF (Stellar Community Fund) Handbook
- YouTube tutorials and educational content
- GitHub repositories from the Stellar ecosystem

**How to use Stella at the Hackathon:**

During the hackathon, use Stella when you have Stellar-specific questions. It's your first stop for:
- "How do I create a Soroban smart contract?"
- "What is the difference between Stellar assets and Soroban tokens?"
- "How do I set up a testnet account?"
- "How do I interact with the Stellar network using JavaScript?"

For general programming questions, use one of the open-source models described in this guide or a free tool like Cursor or Google AI Studio.

---

## Summary: Your AI Toolkit for the Hackathon

| Situation | Recommended Tool |
|-----------|-----------------|
| Stellar-specific questions | Stella (developers.stellar.org) |
| Good laptop with 8+ GB VRAM | Qwen2.5-Coder-7B via Ollama + Claude Code |
| Low-end laptop (no GPU) | Phi-4 via Ollama (CPU mode) or Cursor free tier |
| Want the best possible local model | Rent a RunPod GPU and run Qwen2.5-Coder-32B |
| Just want something that works now | Cursor free tier or Google AI Studio |
| Using Claude Code for free | Llama 3.1 8B via Ollama (best tool-call compatibility) |

---

*Document prepared by the Stellar Development Foundation for hackathon participants.*
*Open-source models are free to download and use. Always check individual model licenses for commercial use restrictions.*
