<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Segoe+UI&weight=800&size=30&duration=2600&pause=1000&color=EDEBDC&center=true&vCenter=true&width=760&lines=KORCORE+Studio+%E2%80%94+Native+Local+AI+From+Scratch;NO+llama.cpp+%E2%9C%97+%7C+korcore.cpp+%E2%9C%93;26B+%26+120B+Models+%E2%80%A2+One-Click+Setup+%E2%80%A2+100%25+Offline" alt="KORCORE Studio" />

<p>
  <img src="https://img.shields.io/badge/Platform-Windows%20x64%20(AVX2)-1b222a?style=for-the-badge&logo=windows&logoColor=edebdc" alt="Platform"/>
  <img src="https://img.shields.io/badge/Engine-korcore.cpp-16a34a?style=for-the-badge&logo=cplusplus&logoColor=white" alt="Engine"/>
  <img src="https://img.shields.io/badge/API-OpenAI%20%2B%20Ollama-2c3640?style=for-the-badge" alt="API"/>
  <img src="https://img.shields.io/badge/Privacy-100%25%20Offline-c99177?style=for-the-badge" alt="Offline"/>
  <a href="https://www.virustotal.com/gui/file/7e2dd3ae3a8a6d8fbd71e6710587575bc05d5b7f717d0cfc1ebdaa0e95426882?nocache=1"><img src="https://img.shields.io/badge/VirusTotal-Report-16a34a?style=for-the-badge&logo=virustotal&logoColor=white" alt="VirusTotal"/></a>
  <a href="https://korkuttech.com"><img src="https://img.shields.io/badge/Korkut%20Tech-korkuttech.com-1b222a?style=for-the-badge" alt="Korkut Tech"/></a>
</p>

**Was all the money spent on massive GPUs and cloud subscriptions really necessary, or was proper programming the only missing piece?**

[Download](https://drive.google.com/drive/folders/1_K3rMYqzaHTXIPoesEmhpOyCb7gB0Qx_?usp=sharing) · [Installation](#-installation) · [Supported Models](#-supported-models) · [Benchmarks](#-measured-speed--stress-tests) · [API Endpoints](#-local-api-endpoints-12700111434) · [korkuttech.com](https://korkuttech.com)

</div>

---

## Why KORCORE Studio?

This is not a UI wrapper built on top of an existing engine (`llama.cpp`, `Ollama`, etc.). Powered by an independent native **C++17 + CUDA** engine (`korcore.cpp`) written from scratch:

- **Zero External Dependencies:** No Python, MinGW, or CUDA Toolkit required; runs as a single EXE / single installer.
- **Zero Cloud & Full Privacy:** All chats, code, and model files are processed strictly on your device (`127.0.0.1:11434`).
- **In-App Model Downloader:** Clicking an uninstalled model in the list downloads it directly inside the app with `.part` resume support and a live progress bar.
- **3 Interfaces in One Package:** Native Desktop Studio + Browser Web Panel (`/chat`) + **VS Code Agent Extension**.

---

## Supported Models

| Model | File | Size | In-App Download |
| :--- | :--- | :--- | :--- |
| **Gemma-4 26B MoE (Q8)** | `gemma-4-26B-A4B-it-UD-Q8_K_XL.gguf` | `25.7 GB` | One-click from list |
| **GPT-OSS 120B (F16)** | `gpt-oss-120b-F16.gguf` | `60.9 GB` | One-click from list |

---

## Measured Speed & Stress Tests

> Note: In regular everyday usage, speeds reach **15 – 20 tok/s**. The table below specifically reflects constrained low-resource stress tests — with higher-end hardware, you get even higher performance; our job isn't hardware, it's building software :D

| Scenario / Hardware | Model | Normally (Standard Tools) | KORCORE Studio (`korcore.cpp`) |
| :--- | :--- | :--- | :--- |
| **Standard Laptop** *(RTX 3050 4 GB VRAM + 16 GB RAM)* | **Gemma-4 26B (Q8)** | ~1 – 3 tok/s or insufficient VRAM / OOM | **`10.4 tok/s`** · Cache hit: **`99.5%`** |
| **Strict Constrained Stress Test** *(4 GB RAM + 2 GB VRAM limit)* | **Gemma-4 26B (Q8)** | Won't run / OOM | **`7.42 tok/s`** · RAM: `3858 MB` · VRAM: `1907 MB` |
| **Standard Laptop** *(After warm cache)* | **GPT-OSS 120B (F16)** | Won't open on standard laptop / OOM | **`1.9 – 5.0 tok/s`** *(60.9 GB F16 local inference)* |
| **Strict Constrained Stress Test** *(4 GB RAM + 2 GB VRAM limit)* | **GPT-OSS 120B (F16)** | Won't run / OOM | **`1.59 tok/s`** · RAM: `3828 MB` · VRAM: `1913 MB` |

> Note: On a cold first start, speeds may start lower while the cache warms up; after 1–2 messages, warm caching kicks in.

---

## System Architecture

```text
+---------------------------+     +------------------------------+     +---------------------------------+     +---------------------------+
|     1. INTERFACES         |     |       2. LOCAL API           |     |    3. KORCORE.CPP               |     |     4. HARDWARE           |
+---------------------------+     +------------------------------+     +---------------------------------+     +---------------------------+
|  Desktop Studio (Win32)   | --> |                              |     |  Smart Memory Manager           | <-> |  NVIDIA GPU (CUDA)        |
|  Web Panel (/chat)        | --> |  http://127.0.0.1:11434      | --> |              |                  |     |  System RAM               |
|  VS Code Agent Extension  | --> |  OpenAI + Ollama Compatible  |     |              v                  | <-> |  NVMe SSD (.gguf & Cache) |
|                           |     |  (100% On-Device / Private)  |     |  Custom C++17 / CUDA Engine     |     |  CPU Fallback (AVX2)      |
+---------------------------+     +------------------------------+     +---------------------------------+     +---------------------------+
```

---

## Local API Endpoints (`127.0.0.1:11434`)

| Endpoint | Method | Description |
| :--- | :--- | :--- |
| `/v1/chat/completions` | `POST` | OpenAI-compatible SSE streaming + native Tool Calling |
| `/v1/models` | `GET` | Currently loaded model list |
| `/v1/status` | `GET` | Live speed (`tok/s`), RAM/VRAM usage, and hardware status |
| `/v1/stop` | `POST` | Immediately abort ongoing generation |
| `/api/chat` · `/api/tags` · `/api/show` | `POST`/`GET` | Ollama-compatible client support |
| `/chat` | `GET` | Built-in multi-chat Web Panel |

---

## Key Features

- **Desktop & Web Panel (`/chat`):** Multi-chat history, per-chat custom system prompts, live `tok/s` & `Hit %` telemetry, one-click `Copy Code`, smart scroll-up lock, and toggleable `Thinking` process.
- **VS Code Integration:** Automatically installs the VS Code extension during setup; supports Agent mode file creation, editing, and unlimited-duration local code generation.
- **Settings & Auto-Start:** Performance profiles, RAM/VRAM/Thread controls, **Run on Windows Startup**, and **Auto-start selected model on launch**.

---

## Installation

Download the package that suits you from the [Google Drive Download Folder](https://drive.google.com/drive/folders/1_K3rMYqzaHTXIPoesEmhpOyCb7gB0Qx_?usp=sharing):

| Package | Description |
| :--- | :--- |
| **`KORCORE_Studio_Setup.exe`** | Setup wizard with Desktop/Start Menu shortcuts, Windows Startup option, and automatic VS Code extension installation. Upgrading over an existing install **preserves** `models\`, chats, and settings. *(Silent mode: `/S`)* |
| **`KORCORE_Portable.zip`** | Portable zero-install package; extract to a folder and run `KORCORE_Studio.exe`. |

### First Run
1. Open `KORCORE Studio`.
2. In the **Local Models** list on the left, click your preferred model (`gemma-4` or `gpt-oss-120b`) to download it directly in-app (or place your `.gguf` file in `models\`).
3. Click **`Start Engine`** (`Motoru Baslat`) and start chatting — or connect via **`Web UI`** (`Web Arayuz`) in your browser or **VS Code**.

> **Requirements:** Windows 10/11 (64-bit), AVX2-capable CPU (2013+ Intel / AMD Ryzen). Uses CUDA automatically if an NVIDIA GPU is present; otherwise runs in CPU mode.  
> **Windows SmartScreen:** If Windows shows a warning on first launch, click *"More info → Run anyway"*.

---

## License

Copyright (c) 2026 [dertos.org](https://dertos.org) · [korkuttech.com](https://korkuttech.com). All rights reserved.

<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:1b222a,100:c99177&height=90&section=footer" width="100%" alt="footer"/>
</div>
