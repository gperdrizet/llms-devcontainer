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

## What's included

### GPU environment

| Category | Details |
|----------|---------|
| **Base Image** | `nvidia/cuda:12.8.1-cudnn-runtime-ubuntu24.04` |
| **GPU** | CUDA 12.8, PyTorch 2.11.0 (custom wheel, Pascal-Blackwell) |
| **Python** | 3.12 |
| **LLM Frameworks** | LangChain, LlamaIndex, Transformers, smolagents |
| **API Clients** | OpenAI, Anthropic, Ollama |
| **Vector Store** | ChromaDB, sentence-transformers |
| **Tools** | Gradio, accelerate, datasets, tiktoken |

### CPU environment

| Category | Details |
|----------|----------|
| **Base Image** | `python:3.12-slim` |
| **Python** | 3.12, PyTorch (CPU) |
| **LLM Frameworks** | LangChain, Transformers, smolagents |
| **API Clients** | OpenAI, Anthropic, Ollama |
| **Vector Store** | ChromaDB, sentence-transformers |
| **Tools** | Gradio, accelerate, datasets, tiktoken |

### Mac environment

| Category | Details |
|----------|----------|
| **Base Image** | `python:3.12-slim` (linux/arm64) |
| **Python** | 3.12, PyTorch (ARM64, from PyPI) |
| **LLM Frameworks** | LangChain, Transformers, smolagents |
| **API Clients** | OpenAI, Anthropic, Ollama (ARM64 binary) |
| **Vector Store** | ChromaDB, sentence-transformers |
| **Tools** | Gradio, accelerate, datasets, tiktoken |


## Project structure

```
llms-devcontainer/
├── .devcontainer/
│   ├── nvidia/
│   │   └── devcontainer.json   # NVIDIA GPU dev container configuration
│   ├── cpu/
│   │   └── devcontainer.json   # CPU dev container configuration
│   └── mac/
│       └── devcontainer.json   # Mac (ARM64) dev container configuration
├── data/                       # Store datasets here
├── logs/                       # Training/experiment logs
├── models/                     # Saved model files
├── src/
│   └── environment_test.py     # Verify your setup
├── .gitignore
├── LICENSE
└── README.md
```

## Requirements

### GPU environment
- **NVIDIA GPU** (Pascal or newer) with driver ≥570
- **Docker** with GPU support ([Windows](https://docs.docker.com/desktop/setup/install/windows-install) | [Linux](https://docs.docker.com/desktop/setup/install/linux))
- **VS Code** with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

> **Linux users:** Also install the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

### CPU environment
- **Docker** ([Windows](https://docs.docker.com/desktop/setup/install/windows-install) | [Linux](https://docs.docker.com/desktop/setup/install/linux))
- **VS Code** with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### Mac environment
- **Docker Desktop for Mac** (Apple Silicon): [install guide](https://docs.docker.com/desktop/setup/install/mac-install/)
- **VS Code** with the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

> **Note:** GPU acceleration is not available inside Docker containers on Apple Silicon. Metal/MPS is a macOS-only framework with no Docker passthrough. The Mac configuration provides native ARM64 CPU performance.

### GPU compatibility

The GPU environment requires an NVIDIA GPU with **compute capability 6.0+** (Pascal architecture or newer):

| Architecture | Example GPUs | Compute Capability |
|--------------|--------------|-------------------|
| Pascal | GTX 1050–1080, Tesla P100 | 6.0–6.1 |
| Volta | Tesla V100, Titan V | 7.0 |
| Turing | RTX 2060–2080, GTX 1660 | 7.5 |
| Ampere | RTX 3060–3090, A100 | 8.0–8.6 |
| Ada Lovelace | RTX 4060–4090 | 8.9 |
| Hopper | H100, H200 | 9.0 |
| Blackwell | RTX 5070–5090, B100, B200 | 10.0 |

Check your GPU's compute capability: [NVIDIA CUDA GPUs](https://developer.nvidia.com/cuda-gpus)

## Quick start

1. **Fork** this repository (click "Fork" button above)

2. **Clone** your fork:
   ```bash
   git clone https://github.com/<your-username>/llms-devcontainer.git
   ```

3. **Open VS Code**

4. **Open Folder in Container** from the VS Code command palette (Ctrl+Shift+P), start typing `Open Folder in`...
   - Select **LLM NVIDIA** if your machine has a compatible NVIDIA GPU, **LLM Mac** if you're on Apple Silicon, or **LLM CPU** otherwise.

5. **Verify** by running `python src/environment_test.py`

## Using as a template for new projects

You can use your fork as a template to quickly create new LLM application projects:

### One-time setup: Make your fork a template

1. Go to your fork on GitHub
2. Click **Settings** → scroll to **Template repository**
3. Check the box to enable it

### Creating a new project from your template

1. Go to your fork on GitHub
2. Click the green **Use this template** button → **Create a new repository**
3. Enter your new repository name and settings
4. Click **Create repository**
5. **Clone** your new repository:
   ```bash
   git clone https://github.com/<your-username>/my-new-project.git
   ```

Now you have a fresh LLM application project with the dev container configuration ready to go!

## Adding Python packages

### Using pip directly

Install packages in the container terminal:

```bash
pip install <package-name>
```

> **Note:** Packages installed this way will be lost when the container is rebuilt.

### Using requirements.txt (recommended)

For persistent packages that survive container rebuilds:

1. **Create** a `requirements.txt` file in the repository root:
   ```
   langchain-community
   faiss-cpu
   ```

2. **Update** the appropriate `.devcontainer/*/devcontainer.json` to install packages on container creation by adding a `postCreateCommand`:
   ```json
   "postCreateCommand": "pip install -r requirements.txt"
   ```

3. **Rebuild** the container (`F1` → "Dev Containers: Rebuild Container")

Now your packages will be automatically installed whenever the container is created.

## Gradio web UI

Both environments include Gradio for building interactive demos. To run a Gradio app:

```python
import gradio as gr

def greet(name):
    return f"Hello, {name}!"

demo = gr.Interface(fn=greet, inputs="text", outputs="text")
demo.launch(server_name="0.0.0.0")
```

The default Gradio port (7860) is accessible from your host machine.

## Keeping your fork updated

```bash
# Add upstream (once)
git remote add upstream https://github.com/gperdrizet/llms-devcontainer.git

# Sync
git fetch upstream
git merge upstream/main
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Docker won't start | Enable virtualization in BIOS |
| Permission denied (Linux) | Add user to docker group, then log out/in |
| GPU not detected | Update NVIDIA drivers (≥570), install NVIDIA Container Toolkit |
| Container build fails | Check internet connection |
| Module not found | Rebuild container after adding to requirements.txt |

