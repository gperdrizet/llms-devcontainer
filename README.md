# LLM application development environment

[![Sync release](https://github.com/gperdrizet/llms-devcontainer/actions/workflows/sync.yml/badge.svg)](https://github.com/gperdrizet/llms-devcontainer/actions/workflows/sync.yml)
[![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.11.0-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![CUDA](https://img.shields.io/badge/CUDA-12.8-76B900?logo=nvidia&logoColor=white)](https://developer.nvidia.com/cuda-toolkit)
[![Transformers](https://img.shields.io/badge/🤗_Transformers-latest-FFD21E)](https://huggingface.co/docs/transformers)
[![LangChain](https://img.shields.io/badge/LangChain-latest-1C3C3C?logo=langchain&logoColor=white)](https://www.langchain.com/)
[![Docker Pulls llms-nvidia](https://img.shields.io/docker/pulls/gperdrizet/llms-nvidia?label=llms-nvidia&logo=docker)](https://hub.docker.com/r/gperdrizet/llms-nvidia)
[![Docker Pulls llms-cpu](https://img.shields.io/docker/pulls/gperdrizet/llms-cpu?label=llms-cpu&logo=docker)](https://hub.docker.com/r/gperdrizet/llms-cpu)
[![Docker Pulls llms-mac](https://img.shields.io/docker/pulls/gperdrizet/llms-mac?label=llms-mac&logo=docker)](https://hub.docker.com/r/gperdrizet/llms-mac)

A ready-to-use LLM application development environment for VS Code. Includes **LangChain**, **LlamaIndex**, **Hugging Face Transformers**, and **smolagents**. Available in three configurations: NVIDIA GPU, CPU-only, and Mac (Apple Silicon).

## Requirements

**All users**
- [Docker Desktop](https://docs.docker.com/desktop/) (Windows / Mac) or [Docker Engine](https://docs.docker.com/engine/install/) (Linux)
- [VS Code](https://code.visualstudio.com/) with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

**NVIDIA GPU users** (also required)
- NVIDIA driver ≥570 ([download](https://www.nvidia.com/Download/index.aspx))
- [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) *(Linux only - not needed on Windows)*

> **Mac users:** GPU acceleration (Metal/MPS) does not pass through to Docker containers. The Mac configuration uses native ARM64 CPU, no extra setup needed beyond Docker Desktop.

## Quick start

1. **Fork** this repository (click **Fork** at the top of this page)

2. **Clone** your fork:
   ```bash
   git clone https://github.com/<your-username>/llms-devcontainer.git
   ```

3. **Open the folder in VS Code**, then open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`) and run **Dev Containers: Open Folder in Container**

   > VS Code will ask which configuration to use, pick the one that matches your machine (see table below).

4. **Verify** your setup by running `python src/environment_test.py`

## Which configuration should I use?

| If you have... | Choose this |
|----------------|-------------|
| NVIDIA GPU (GTX 10xx / RTX / Quadro / Tesla) | **LLM NVIDIA** |
| Windows or Linux machine, no NVIDIA GPU | **LLM CPU** |
| Apple Silicon Mac (M1 / M2 / M3 / M4) | **LLM Mac** |

Not sure if your GPU is compatible? Check: [NVIDIA CUDA GPUs](https://developer.nvidia.com/cuda-gpus) (need compute capability ≥6.0).

## Using as a template for new projects

Fork this repo once, then use it as a GitHub template to spin up new projects instantly.

### One-time setup

1. Go to your fork on GitHub
2. Click **Settings** → scroll to **Template repository** → enable it

### Creating a new project

1. Go to your fork and click **Use this template** → **Create a new repository**
2. Name your new repo and click **Create repository**
3. Clone it and start working:
   ```bash
   git clone https://github.com/<your-username>/my-new-project.git
   ```

4. **Clean it up** - remove anything that doesn't belong to your project:
   - Update `README.md` to describe your project
   - Delete unused devcontainer configs (e.g. if you only use CPU, remove `nvidia/` and `mac/`)
   - Replace `src/environment_test.py` with your own source files
   - Clear `logs/`, `models/`, and `data/`
   ```bash
   git add -A && git commit -m "Initial project setup" && git push
   ```

## Adding Python packages

### Temporary (lost on container rebuild)

```bash
pip install <package-name>
```

### Permanent (recommended)

1. Create a `requirements.txt` in the repository root:
   ```
   openai
   langchain-openai
   ```

2. Add a `postCreateCommand` to the relevant `.devcontainer/*/devcontainer.json`:
   ```json
   "postCreateCommand": "pip install -r requirements.txt"
   ```

3. Rebuild the container (`Ctrl+Shift+P` → **Dev Containers: Rebuild Container**)

## Running Gradio apps

Gradio is included for building interactive demos. To run a Gradio app:

```python
import gradio as gr

def greet(name):
    return f"Hello, {name}!"

demo = gr.Interface(fn=greet, inputs="text", outputs="text")
demo.launch(server_name="0.0.0.0")
```

Port 7860 is published by the container, so the app is accessible at `http://localhost:7860` on your host machine.

## Keeping your fork updated

```bash
# Add upstream once
git remote add upstream https://github.com/gperdrizet/llms-devcontainer.git

# Pull in updates
git fetch upstream && git merge upstream/main
```

## What's included

### NVIDIA environment

| Category | Details |
|----------|---------|
| **Base image** | `nvidia/cuda:12.8.1-cudnn-runtime-ubuntu24.04` |
| **GPU** | CUDA 12.8, PyTorch 2.11.0 (custom wheel, Pascal-Blackwell) |
| **Python** | 3.12 |
| **LLM frameworks** | LangChain, LlamaIndex, Transformers, smolagents |
| **API clients** | OpenAI, Anthropic, Ollama |
| **Vector store** | ChromaDB, sentence-transformers |
| **Tools** | Gradio, accelerate, datasets, tiktoken |

### CPU environment

| Category | Details |
|----------|---------|
| **Base image** | `python:3.12-slim` |
| **Python** | 3.12, PyTorch (CPU) |
| **LLM frameworks** | LangChain, LlamaIndex, Transformers, smolagents |
| **API clients** | OpenAI, Anthropic, Ollama |
| **Vector store** | ChromaDB, sentence-transformers |
| **Tools** | Gradio, accelerate, datasets, tiktoken |

### Mac environment

| Category | Details |
|----------|---------|
| **Base image** | `python:3.12-slim` (linux/arm64) |
| **Python** | 3.12, PyTorch (ARM64, from PyPI) |
| **LLM frameworks** | LangChain, LlamaIndex, Transformers, smolagents |
| **API clients** | OpenAI, Anthropic, Ollama (ARM64 binary) |
| **Vector store** | ChromaDB, sentence-transformers |
| **Tools** | Gradio, accelerate, datasets, tiktoken |

## GPU compatibility (NVIDIA)

Requires compute capability ≥6.0 (Pascal / GTX 10xx or newer):

| Architecture | Example GPUs | Compute Capability |
|--------------|--------------|-------------------|
| Pascal | GTX 1050-1080, Tesla P100 | 6.0-6.1 |
| Volta | Tesla V100, Titan V | 7.0 |
| Turing | RTX 2060-2080, GTX 1660 | 7.5 |
| Ampere | RTX 3060-3090, A100 | 8.0-8.6 |
| Ada Lovelace | RTX 4060-4090 | 8.9 |
| Hopper | H100, H200 | 9.0 |
| Blackwell | RTX 5070-5090, B100, B200 | 10.0 |

## Project structure

```
llms-devcontainer/
├── .devcontainer/
│   ├── nvidia/
│   │   └── devcontainer.json   # NVIDIA GPU configuration
│   ├── cpu/
│   │   └── devcontainer.json   # CPU configuration
│   └── mac/
│       └── devcontainer.json   # Mac (ARM64) configuration
├── data/                       # Store datasets here
├── logs/                       # Training/experiment logs
├── models/                     # Saved model files
├── src/
│   └── environment_test.py     # Verify your setup
├── .gitignore
├── LICENSE
└── README.md
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Docker won't start | Enable virtualization in BIOS / enable WSL2 on Windows |
| Permission denied (Linux) | Add your user to the docker group, then log out and back in |
| GPU not detected | Update NVIDIA drivers (≥570); Linux: install [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) |
| Container build fails | Check your internet connection |
| Module not found | Add the package to `requirements.txt` and rebuild the container |

