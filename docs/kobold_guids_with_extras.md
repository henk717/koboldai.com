Great. I’ll create a fully organized, comprehensive instruction manual for using KoboldCpp. This will be based on the wiki you shared, and I’ll also supplement any missing or unclear parts with additional research.

I’ll structure the manual to be logically organized and beginner-friendly, covering installation, configuration, usage, performance optimization, UI modes, integrations, API usage, and advanced settings.
I’ll let you know when it’s ready for your review.

# KoboldCpp Comprehensive User Manual

## 1. Overview of KoboldCpp

KoboldCpp is an easy-to-use local AI text generation software for running large language models (LLMs) in the GGML/GGUF format ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20KoboldCpp%3F)). It is inspired by the original KoboldAI project and provides a single self-contained application that bundles a web-based UI, a text-generation backend (built on the llama.cpp library), and multiple integrated features. KoboldCpp allows you to load **LLM models** (such as LLaMA, LLaMA-2, Mistral, Pygmalion, etc.) in a lightweight format and interact with them through a convenient web interface or API ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20KoboldCpp%3F)). In addition to text generation, KoboldCpp extends functionality with advanced features like: 

- A **web UI (Kobold Lite)** that supports persistent stories, editing tools, memory, world info, character profiles, scenario presets, and multiple **interaction modes** (Story, Chat, Instruct, Adventure) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20KoboldCpp%3F)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,crafted%20Scenario)).
- Built-in **KoboldAI API** compatibility (so other applications can use KoboldCpp as if it were a KoboldAI server), as well as **OpenAI**-compatible and **Ollama**-compatible API endpoints for broad integration support ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Is%20there%20an%20OpenAI%20API%3F)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Is%20there%20an%20Ollama%20API%3F)).
- Support for **Stable Diffusion image generation** (via *stable-diffusion.cpp* integration) and **speech processing** (speech-to-text via Whisper, text-to-speech via Oobabooga’s OuteTTS) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20KoboldCpp%3F)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20Whisper%3F)).
- Full **offline operation** for privacy – all processing is local by default (no internet required) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=KoboldCpp%20is%20capable%20of%20running,or%20review%20it%20on%20github)) – and **open-source** code (AGPLv3) available for verification ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=displayed%20in%20the%20terminal%20console%2C,or%20review%20it%20on%20github)).
- **Cross-platform support**: KoboldCpp runs on Windows, Linux, macOS (including Apple Silicon), Android (via Termux) and even under WSL, with both CPU-only and GPU-accelerated execution options.

In summary, KoboldCpp provides an all-in-one solution to run LLMs on your own hardware with an intuitive interface and rich feature set, combining the strengths of llama.cpp’s efficient inference with KoboldAI’s user-friendly storytelling/chat UI ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20KoboldCpp%3F)). The sections below detail system requirements, model formats, installation steps, configuration, usage, and advanced features to help you get the most out of KoboldCpp.

## 2. System Requirements

**Operating System:** KoboldCpp is cross-platform and runs on Windows (10 or later recommended), Linux (64-bit), macOS (both Intel and Apple Silicon), and Android (via a Linux environment app). It can also be compiled and run in a Windows Subsystem for Linux (WSL) environment ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,guide%20below.%20%2A%20WSL)). Windows 7 is *not* recommended; it lacks modern CPU support required for performance, though a fallback mode can allow it to run slowly ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Koboldcpp%20is%20not%20working%20on,windows%207)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Windows%207%20is%20not%20a,your%20OS%20to%20Windows%2010)).

**CPU:** A 64-bit CPU is required. For optimal performance, a processor with AVX2 support is recommended, as KoboldCpp uses AVX2 instructions for fast math operations. On older CPUs without AVX2, KoboldCpp offers a “No AVX2” fallback mode (`--noavx2` flag or a special launcher preset) that uses only AVX or basic instructions ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20older%20devices%20do%20not,all%20CPU%20intrinsics%20and%20GPU)). There’s also a `--failsafe` mode which disables all CPU SIMD optimizations as a last resort ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=mode,CPU%20intrinsics%20and%20GPU%20usage)). Keep in mind that using these fallback modes will be significantly slower ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=resort%2C%20you%20can%20try%20enabling,CPU%20intrinsics%20and%20GPU%20usage)). In general, more CPU cores help, since you can configure the number of threads for generation (usually set threads ≈ number of physical cores) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=How%20to%20choose%20how%20many,blasthreads)).

**Memory (RAM):** Memory requirements depend on the size of the model and quantization level (see section on *Model Formats and Quantization*). As a rule of thumb, for a standard 2048-token context and 4-bit quantized model (Q4_0), you will need approximately ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=The%20amount%20of%20RAM%20required,context%20with%20a%20Q4_0%20quantization)):

- **4 GB RAM** for a 3 billion parameter model ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=2048%20context%20with%20a%20Q4_0,quantization)).
- **8 GB RAM** for a 7B model ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=2048%20context%20with%20a%20Q4_0,quantization)).
- **16 GB RAM** for a 13B model ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,needs%20at%20least%2064GB%20RAM)).
- **32 GB RAM** for a 30B model ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,needs%20at%20least%2064GB%20RAM)).
- **64 GB RAM** for a 65B model ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,needs%20at%20least%2064GB%20RAM)).

Using higher precision models (e.g. 8-bit or 16-bit) or larger context windows (4096, 8192 tokens, etc.) will increase RAM needs, whereas using smaller quantization (e.g. 3-bit) reduces RAM at the cost of some model quality ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=The%20amount%20of%20RAM%20required,context%20with%20a%20Q4_0%20quantization)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Offloading%20layers%20to%20the%20GPU,section%20on%20GPU%20layer%20offloading)). **Disk Space** required depends on model file size; quantized models range from a few gigabytes (for 7B) up to tens of GB (for 65B in higher precisions). Ensure you have enough storage for at least one model file and any additional components (like image models or audio models if you use those features).

**GPU (Optional):** KoboldCpp can run purely on CPU, but if you have a discrete GPU or modern iGPU, it can leverage it for acceleration. NVIDIA GPUs (with CUDA 11+ support) and many AMD or Intel GPUs (with OpenCL or Vulkan support) are supported for **inference acceleration**. However, a GPU is *not* strictly required – even without one, KoboldCpp will use CPU threads for all computations. If you plan to use GPU offloading, see the *Performance Tuning* section for driver/library requirements (e.g. NVIDIA CUDA Toolkit for Windows or appropriate libraries on Linux for OpenCL/Vulkan) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,GPU%20acceleration%20with)). For Apple Silicon Macs, the built-in GPU can be utilized via Metal support ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=with%20%60,decent%20speeds%20by%20selecting%20the)). 

**Additional Requirements:** If using the **Stable Diffusion** image generation feature, you will need a compatible Stable Diffusion model file (which can be a `.safetensors` for SD) and sufficient VRAM/RAM for it (generating images may require several GB of VRAM or will fall back to CPU if VRAM is insufficient) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20generate%20images%20with,KoboldCpp)). For **Whisper** speech-to-text or **TTS** audio generation, you will need to download the respective model files (Whisper GGML for transcription, OuteTTS GGUF for TTS along with a WavTokenizer) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Whisper%20is%20a%20speech,with%20the%20KoboldCpp%20transcription%20endpoint)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20OuteTTS%20Text%20To,Speech)). We will discuss obtaining these in later sections. Finally, a modern web browser (Chrome, Firefox, Safari, etc.) is needed to use the KoboldCpp Web UI; the UI is served locally at a configurable port.

## 3. Model Formats and Obtaining Models

**Supported Model Formats:** KoboldCpp supports *GGUF* models natively, which is the latest generation of the GGML family of quantized LLM formats ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20models%20does%20KoboldCpp%20support%3F,What%20architectures%20are%20supported)). GGUF (.gguf) is a container format for LLMs that includes model weights and metadata; it is now the recommended format and is actively updated. KoboldCpp also retains backward compatibility with older GGML `.bin` model files (legacy GGMLv1-v3, GPTQ-for-llama conversions, etc.), so you can still load older models if needed ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20models%20does%20KoboldCpp%20support%3F,What%20architectures%20are%20supported)). Note, however, that some new KoboldCpp features may only work with GGUF models (for instance, **Context Shift** is available only for GGUF models) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Context%20Shifting%20is%20a%20better,To%20disable%20Context%20Shifting)). Other model weight formats like PyTorch `.pt/.bin` or Hugging Face Transformers safetensors are **not** directly usable; those need to be converted to GGUF/GGML first ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Other%20formats%20such%20as%20safetensors,see%20below)). 

**Model Architectures:** Virtually any model architecture available in GGUF format should run in KoboldCpp. This includes LLaMA and LLaMA-2 (and their fine-tunes like Alpaca, Vicuna, WizardLM, etc.), newer architectures like Mistral, Code models (StarCoder), chat-oriented models (Pygmalion, Metharme), instruction-tuned models, and more ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Deepseek%20and%20many%2C%20many%20more)). An incomplete list from the KoboldCpp documentation includes: *Llama, Llama2, “Llama3” (experimental), Alpaca, GPT4All variants, Vicuna, Koala, Pygmalion, Metharme, WizardLM, Mistral/Mixtral, MPT, Falcon, RWKV4, GPT-NeoX/Pythia, StableLM, Dolly 2, RedPajama, GPT-J, Cerebras, Phi-2/3, GPT-2, etc.* ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Deepseek%20and%20many%2C%20many%20more)). Practically, if a model has been converted to GGUF, KoboldCpp can likely load it.

**Where to Get Models:** The best source for GGUF (and older GGML) models is **Hugging Face**. Many community contributors (like TheBloke, KoboldAI, Pygmalion, etc.) host quantized models there. You can browse the HuggingFace Hub for model names – for example, KoboldCpp’s guide suggests easy starters such as **Airoboros Mistral 7B** (a smaller instruct model) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,is%20%2065%20Airoboros%20Mistral)), **Tiefighter 13B** (medium-sized model) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Deliberate%20V2%20or%20Dreamshaper%20SDXL)), or **Beepo 22B** (larger model) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Deliberate%20V2%20or%20Dreamshaper%20SDXL)). For chat or roleplay, models like **Pygmalion** or **Fimbulvetr 13B** are popular ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,correct%20one%20for%20your%20model)), and for general Q&A instruct you might try **L3 Stheno v3** ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,correct%20one%20for%20your%20model)). 

**Model Download Example:** To download a model from HuggingFace, you can use their website or `git`/`wget`. For instance, to get a small model via command line: 

```bash
wget https://huggingface.co/TheBloke/phi-2-GGUF/resolve/main/phi-2.Q2_K.gguf
``` 

This would download a 2-bit quantized Phi-2 model (very tiny for testing) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,2.Q2_K.gguf)). Once downloaded, you’ll have a single `.gguf` (or `.bin` for older) file that represents the model. **You only need one quantized file per model** – e.g., if a model page offers `model.Q4_0.gguf`, `model.Q5_1.gguf`, etc., pick the one matching your desired quantization; you do *not* need all of them ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=need%20them%20all%3F%20Which%20Quantization%3F,F16%3F%20Q4_0%3F%20Q5_1)).

**Quantization Formats:** Models on HuggingFace often come in multiple quantization levels. These indicate the precision and size of the model. Generally, higher-bit quantizations have better fidelity but larger size, while lower-bit are smaller/faster but slightly lower quality ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=ascending%20order,Read%20more%20here)). Common formats you’ll see are:

- **F16 (float16):** Full 16-bit precision. Highest quality (minimal quantization loss) but very large files (often 2× the size of 8-bit).
- **Q8_0:** 8-bit quantization. Almost lossless quality to original 16-bit, still quite large.
- **Q6_K:** 6-bit “K” quantization (a newer method). Good quality with significant size reduction. Often a sweet spot if available.
- **Q5_0 / Q5_1:** 5-bit quantizations (Q5_1 is a slightly improved variant) – good balance of quality and size. A Q5_1 model will be a bit larger and more accurate than a Q4_0 ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=ascending%20order,Read%20more%20here)).
- **Q5_K (Q5_K_M, Q5_K_S):** 5-bit with advanced algorithm (“K-sampling” variants, often labeled M=medium, S=small in some model names). These tend to have even better quality per bit than older Q5.
- **Q4_0 / Q4_1:** 4-bit quantizations (with Q4_1 being the improved version). Very popular for 7B-13B models on low-RAM systems; smaller but with some quality penalty compared to 5-bit ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=need%20them%20all%3F%20Which%20Quantization%3F,F16%3F%20Q4_0%3F%20Q5_1)).
- **Q4_K (Q4_K_M, Q4_K_S, etc.):** 4-bit with newer quantization techniques (K-sampling) offering improved fidelity compared to Q4_0/Q4_1 at roughly the same 4-bit size.
- **Q3_K / Q2_K:** 3-bit or 2-bit extreme quantizations. These have the smallest file sizes but much lower model fidelity and are generally experimental or only for very constrained environments ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,hundreds%20more%20GGUFs%20out%20there)).

**Quantization Comparison:** As a rough guide, quantizations can be ordered from **smallest/lowest quality** to **largest/highest quality** as: Q2_K < Q3_K (S/M/L) < Q4_0 < Q4_1 (and Q4_K variants) < Q5_0 < Q5_1 (and Q5_K variants) < Q6_K < Q8_0 < F16 ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,hundreds%20more%20GGUFs%20out%20there)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=ascending%20order,Read%20more%20here)). For example, a 13B model in Q4_0 might be ~7GB, whereas Q8_0 could be ~14GB and F16 28GB. Choose a quantization that fits your hardware: if you have plenty of RAM, Q5_1 or Q6_K can yield better outputs; if you are limited, Q4_0 or Q4_K_S might run where higher might not. *Tip:* You don’t need to manually quantize models yourself unless you have a custom model in PyTorch – the community provides many ready-made GGUF files on HuggingFace.

**Image and Audio Models:** KoboldCpp’s extended features also accept model files: for image generation, you need a Stable Diffusion `.safetensors` model (SD v1.5, SDXL, etc.) – these can be found on HuggingFace or CivitAI (for custom fine-tunes) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Gemma2%20%2F%20GPT,is%20%2065%20Airoboros%20Mistral)). Likewise, for speech recognition, you can download OpenAI Whisper GGML models ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Speech%3A%20TTS%20models%20for%20Narration)), and for text-to-speech (TTS), download the Oobabooga’s *OuteTTS* models and a corresponding wav tokenizer ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Speech%3A%20TTS%20models%20for%20Narration)). We will cover how to use these later.

**Conversion Tools:** If you have a model in PyTorch or Safetensors format that isn’t already in GGUF/GGML, you will need to convert it. Tools like **`transformers`->GGUF converters** (provided in llama.cpp repo) or community scripts can do this. However, given the abundance of pre-converted models, most users can simply download a ready GGUF. For advanced users, reference the llama.cpp repository or KoboldCpp discussions on conversion for guidance.

## 4. Installation 

KoboldCpp is distributed as a stand-alone package, meaning you don’t necessarily need to separately install Python libraries or build from source if you use the prebuilt binaries. Below we outline installation for each platform:

### 4.1 Windows Installation

**Prebuilt Executable (Recommended):** The simplest way to install on Windows is to use the prebuilt **KoboldCpp.exe** release. Download the latest `koboldcpp.exe` from the [official KoboldCpp releases page on GitHub】 ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Easiest)) – it’s a single file containing the entire program. Once downloaded, you can place it in any folder (for example, alongside your model files). Double-click `KoboldCpp.exe` to launch the GUI, where you can then select a model file to load. On first launch, Windows may warn you about running an unknown .exe – allow it to run. You should see a console window and shortly after a message that the UI is accessible (by default at http://localhost:5001).

If you prefer command-line, you can also run `KoboldCPP.exe` from a Command Prompt with arguments. For example, to see available options, run `KoboldCPP.exe --help` in the Command Prompt ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Easiest)). This prints usage information and available flags.

**GPU Support on Windows:** The Windows exe is bundled with support for NVIDIA CUDA and OpenCL by default ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,GPU%20acceleration%20with)). If you have a CUDA-capable GPU and proper drivers, you can use `--usecublas` for GPU acceleration (see **Performance Tuning** section). The Windows build includes the needed `koboldcpp_cublas.dll` for CUDA ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=the%20,correctly%20on%20a%20different%20PC)). For OpenCL (which can accelerate on AMD or Intel GPUs), the Windows build includes CLBlast libraries as well ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=other%20platforms%20,See%20compiling%20with%20Metal)), so `--useclblast` can work out-of-the-box. No extra installation is needed for these on Windows, though you should have the GPU’s drivers installed (e.g., NVIDIA drivers, or Intel/AMD OpenCL runtime which is usually part of graphics driver).

**Alternative: Build from Source (Windows):** If you want to compile KoboldCpp yourself (for example, to tweak something or to get the very latest code), you’ll need Visual Studio, CMake, and optionally the CUDA Toolkit if building with CUDA support ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,you%20may%20need%20to%20include)). Clone the GitHub repository and use the provided CMake project or the Makefile with Mingw. Building on Windows is advanced; most users won’t need to do this thanks to the prebuilt exe. (If you do build, after compiling you’d copy `koboldcpp_cublas.dll` next to `koboldcpp.py` as the instructions note ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,you%20may%20need%20to%20include)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=the%20,correctly%20on%20a%20different%20PC)).)

### 4.2 Linux Installation

On Linux you have a few choices: use the provided precompiled binary, run an automated installer, or compile from source.

**Precompiled Binary:** The KoboldCpp releases page offers a prebuilt binary for Linux (64-bit). It’s typically named something like `koboldcpp-linux-x64-cuda1150` (meaning built with CUDA 11.5) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Easy)). Download this file and give it execute permission (`chmod +x koboldcpp-linux-x64`). You can then run it directly: `./koboldcpp-linux-x64` to start the program ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Easy)). This binary should work on most modern Linux distributions. It’s packaged via PyInstaller and includes necessary dependencies. If you encounter an “GLIBC version” issue on an older distro, or if you want a build with different options (OpenCL, etc.), you might opt to compile.

**One-Line Auto Installer:** If you prefer not to manually download, KoboldCpp provides a convenient curl command. In your terminal, run: 

```bash
curl -fLo koboldcpp https://github.com/LostRuins/koboldcpp/releases/latest/download/koboldcpp-linux-x64 && chmod +x koboldcpp
``` 

This will fetch the latest Linux binary and name it “koboldcpp” ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=the%20binary%20,and%20run%20the%20generated)). Then you can execute `./koboldcpp`. This is an easy way to get started on Linux.

**Automated Build Script:** If the precompiled binary isn’t working for your system (e.g., due to library compatibility), there is an automated build script included in the repository. After cloning the repo, you can run `./koboldcpp.sh dist`. This script will set up a conda environment, install dependencies, build KoboldCpp from source, and produce a new standalone binary in the `dist` folder ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=linux,MacOS%20%28Precompiled%20Binary)). It’s meant to “just work” for users who need to build on Linux without manually handling all dependencies.

**Manual Compile (Linux/macOS):** You can always compile manually. Clone the GitHub repo (`git clone https://github.com/LostRuins/koboldcpp.git`), then inside run `make`. By default this builds a CPU-only version. You can enable various backends by adding flags to make: 

- `make LLAMA_CUBLAS=1` – compile with CUDA cuBLAS support (NVIDIA GPU) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=https%3A%2F%2Fgithub.com%2FLostRuins%2Fkoboldcpp.git%60%20,link%20OpenCL%20and%20CLBlast%20libraries)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,make%20LLAMA_VULKAN%3D1%20LLAMA_CLBLAST%3D1%20LLAMA_CUBLAS%3D1)). Ensure CUDA Toolkit is installed.
- `make LLAMA_CLBLAST=1` – compile with OpenCL/CLBlast support (AMD/Nvidia/Intel GPUs) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,or%20using%20the)). You may need OpenCL headers and CLBlast library (e.g., install `libclblast-dev` on Debian/Ubuntu, or `cblas` and `clblast` on Arch ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,or%20using%20the))).
- `make LLAMA_VULKAN=1` – compile with Vulkan support ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,link%20OpenCL%20and%20CLBlast%20libraries)) (requires Vulkan SDK).
- Combine them if you want multiple backends (e.g., `make LLAMA_CUBLAS=1 LLAMA_CLBLAST=1 LLAMA_VULKAN=1` for all) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,will%20need%20CUDA%20toolkit%20installed)).
- On Apple Silicon Mac, use `LLAMA_METAL=1` to enable Metal acceleration in the build ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=command%20%60koboldcpp.py%20%5Bggml_model.gguf%5D%20%5Bport%5D%60%20,MacOS%20Notes)).

After `make` builds the binaries, you run KoboldCpp via the Python launcher script: `python3 koboldcpp.py [model.gguf] [port]` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,port)) (or use `--model` flag). You can also package it with PyInstaller manually if needed.

**Dependencies:** If compiling, ensure you have a C++ compiler, Python 3, CMake, and any backend libraries (CUDA toolkit for cuBLAS, OpenCL dev for CLBlast, etc.). The KoboldCpp Wiki provides guidance for specific distros (e.g., how to install CLBlast) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,or%20using%20the)). For most users, the prebuilt binary or one-line installer will be simplest.

### 4.3 macOS Installation

**Prebuilt Binary (macOS):** KoboldCpp now offers precompiled binaries for modern macOS (specifically for Apple Silicon M1/M2/M3 chips) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=)). Download the latest ARM64 Mac release (e.g., `koboldcpp-mac-arm64` file) from the GitHub releases. After downloading, open a Terminal, `cd` to the download location, and make it executable: `chmod +x koboldcpp-mac-arm64`. Then run it: `./koboldcpp-mac-arm64` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Self%20Compile)). On first run, macOS Gatekeeper might block it since it’s not signed; if so, go to *System Preferences > Security* and you should see an option to “Allow anyway” for koboldcpp – allow it, then run again ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Self%20Compile)). 

This Mac binary is built for ARM and includes Metal support for GPU acceleration on Apple GPUs. If you’re on an **Intel Mac**, there might not be a prebuilt binary available. In that case, you can compile from source using Xcode or Homebrew toolchains. The code can use Apple’s Accelerate framework for BLAS (for CPU inference) and you could compile with OpenCL or Vulkan for discrete GPU (if you have AMD GPU). Apple Silicon users get best results using Metal (set `LLAMA_METAL=1` in make) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=command%20%60koboldcpp.py%20%5Bggml_model.gguf%5D%20%5Bport%5D%60%20,MacOS%20Notes)).

**Note:** The Mac prebuilt likely uses the Accelerate/Metal backends automatically. If you compile yourself on Mac, the build will detect Accelerate (which usually gives good CPU performance) and you can also include Metal. If BLAS via Accelerate causes any slowdowns, there’s an option to disable BLAS at runtime (`--noblas`) to try pure C++ as an alternative ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Metal,Vulkan%20is%20a)).

### 4.4 Android (via Termux) Installation

Running KoboldCpp *directly* on Android devices is possible using Termux (a Linux terminal emulator app). Keep in mind that performance will be limited by mobile hardware – often it may be more practical to run KoboldCpp on a PC and access it from the phone. However, for experimentation:

1. **Install Termux:** Download Termux from F-Droid or a trusted source (the Play Store version may be outdated). You can get it from the official F-Droid repository ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,make)).
2. **Update and Install Dependencies:** In Termux, first run `pkg update && pkg upgrade`. Then install basic packages: `pkg install wget git python` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,make)). Also install `pkg install clang make` if available, since you’ll need a compiler, and `apt install openssl` if required for any SSL (optional) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,make)).
3. **Get KoboldCpp Source:** `git clone https://github.com/LostRuins/koboldcpp.git` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=missing%20packages%29%20,2.Q2_K.gguf)). This will download the code to a folder.
4. **Compile:** `cd koboldcpp` and run `make`. This should build the KoboldCpp binary for your Android’s ARM architecture. It might take a while on-device.
5. **Download a Small Model:** Pick a very small model to test (larger models likely won’t run well on a phone). For example, download a 70M parameter model: `wget https://huggingface.co/TheBloke/phi-2-GGUF/resolve/main/phi-2.Q2_K.gguf` (as shown in official guide) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,2.Q2_K.gguf)).
6. **Run KoboldCpp:** Start the server with `python koboldcpp.py --model phi-2.Q2_K.gguf`. This launches KoboldCpp on default port 5001 on localhost ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,2.Q2_K.gguf)).
7. **Connect via Browser:** On the phone, open a web browser to `http://localhost:5001` to see the Kobold Lite UI. If you compiled without issues, you should be able to interact with the model (though it will be slow).

**Performance Tip:** Android devices have limited RAM and CPU. You likely need to stick to models 3B or smaller, and use heavy quantization (Q4 or lower). Expect slow generation speeds (perhaps a few tokens per second or less). Offloading to mobile GPU is not straightforward (OpenCL might work on some phones via CLBlast, but it’s untested). Most users use Termux mainly to run a small KoboldCpp that connects to a remote instance, or just as a novelty. For any serious usage, consider remote setups.

### 4.5 WSL (Windows Subsystem for Linux)

While you *can* run KoboldCpp in WSL as if on Linux, it’s generally unnecessary because the native Windows build works well. The KoboldCpp maintainers humorously note “you could [use WSL], but why would you want to?” ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=)). If you still prefer WSL (maybe for Linux environment familiarity):

- Ensure WSL is set up with a distro (Ubuntu, etc.) and has the necessary build tools or use the Linux auto-installer.
- Follow the **Linux Installation** instructions within WSL: either use the one-line installer or compile from source in the WSL terminal ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=)).
- Launch KoboldCpp in WSL. By default it will bind to localhost (which is accessible from Windows host as well). You might need to retrieve the IP or use `localhost` to access the UI from your Windows browser.
- Note that GPU access from WSL2 to Windows GPU is possible for CUDA (with NVIDIA drivers supporting WSL2) or DirectML for AI. However, using the Windows native build is simpler for GPU. If using WSL, you might be limited to CPU or have to fiddle with WSLg for GPU – beyond the scope of this guide.

In summary, WSL installation is possible but not generally recommended unless you have a specific need. The native Windows binary is a straightforward solution for Windows users.

## 5. Launching KoboldCpp: Options and Configuration Flags

After installing, you can launch KoboldCpp either by using its GUI or via command-line with various options. When you start KoboldCpp, it loads the model into memory and opens a local web server (by default at port 5001) serving the Kobold Lite UI and APIs.

**Basic Launch (GUI):** If you run the KoboldCpp executable with no arguments (e.g., double-click the exe or run `./koboldcpp-linux-x64`), it will present a simple launcher GUI. In this GUI, you can choose the model file (via a file picker) and adjust some settings like number of threads, GPU layer count, etc., then click a button to launch the server. This is convenient for beginners. The GUI settings correspond to command-line flags under the hood.

**Command-Line Launch:** You can also launch directly via terminal/command prompt for more control. The general usage is: 

```
koboldcpp [model_file] [port] [--flags...]
``` 

If you omit `model_file`, the GUI will prompt you; if you provide it, it auto-loads that model. You can also specify the port number (default 5001) as a second argument or via `--port` flag ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20port%20does%20Koboldcpp%20use%3F,the%20port%20that%20koboldcpp%20uses)). Any additional options are provided as `--flag value` pairs.

For example, launching a model with 10 layers offloaded to GPU might look like: 

```
koboldcpp.exe E:\models\wizardLM-13B.Q5_1.gguf --usecublas --gpulayers 10 --threads 6
``` 

This would start KoboldCpp with an NVIDIA GPU acceleration (cuBLAS) and offload 10 transformer layers to the GPU, using 6 CPU threads for the rest.

**Finding Available Flags:** To see all command-line options, run `--help`. E.g. `koboldcpp.exe --help` or `python3 koboldcpp.py --help`. This will print a list of all flags and their usage ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=match%20at%20L385%20You%20can,use%20the%20command%20line%20terminal)). The number of options is large, as KoboldCpp exposes many tunables. We will cover the most important ones by category:

### 5.1 Model and Context Options

- **`--model <path>`** – specify the model file to load (GGUF or GGML). If not given, the launcher will ask. You can also specify the model as the first positional argument without using `--model` flag.
- **`--contextsize <N>`** – sets the maximum context length (in tokens) for the model context ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Smart%20Context%20is%20enabled%20via,and%20%27shift%20up%27%20the)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=second%20prompt%20has%20more%20than,then%20the%20process%20repeats%20anew)). Default is typically 2048 (2k) for most models, but you can increase it if the model supports longer context (or if using RoPE scaling). For example `--contextsize 4096` for 4k context, `--contextsize 8192` for 8k. Note: setting higher context uses more RAM and requires allocating larger buffers ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=First%2C%20you%20need%20to%20allocate,be%20overriden%20to%20a%20higher)).
- **`--contextswap`** (if available) or related smart context options – *Deprecated in favor of new methods.* (Original Smart Context used part of context as buffer – see *Smart Context* in Advanced Features).
- **`--smartcontext`** – enable the older Smart Context mechanism (uses ~50% of context as a reusable buffer to avoid frequent reprocessing) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Smart%20Context%20is%20enabled%20via,and%20%27shift%20up%27%20the)). Not needed if using default context shifting (see later).
- **`--noshift`** – by default, **Context Shift** is *enabled* for GGUF models, which automatically shifts out old tokens without using extra context ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Context%20Shifting%20is%20a%20better,To%20disable%20Context%20Shifting)). If for some reason you want to disable this, `--noshift` will turn off context shifting so that Smart Context or default behavior is used.
- **`--nofastforward`** – by default, **Fast Forwarding** is on (skips re-processing of unchanged context for consecutive prompts). You can disable it if needed ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20FastForwarding%3F)).

- **`--prompt <\"text\">`** – run a one-time generation with the given prompt and exit (i.e., non-interactive mode). KoboldCpp will output the model’s completion for the prompt to the console and then quit ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)). This is useful for quick tests or scripting. You can limit the output length with `--promptlimit <N>` tokens ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)).
- **`--cli`** – launches KoboldCpp in an interactive **CLI mode** (no web UI) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20chat%20with%20KoboldCpp,Line%20%2F%20Command%20Prompt%20directly)). This lets you chat with the model in the terminal (similar to how you might use the llama.cpp CLI). It’s useful if you don’t want any GUI or web interface.
- **`--multimodel` / `--modeldir`** – *If supported*, these might allow managing multiple models (not sure if KoboldCpp supports concurrently loaded models; likely it supports switching via Admin mode instead, see Admin in advanced).

### 5.2 Performance and Hardware Flags

- **Threads:** `--threads <N>` – sets the number of CPU threads for inference ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=How%20to%20choose%20how%20many,blasthreads)). By default, KoboldCpp will choose a sensible default (often a bit less than total cores). You can manually set it – e.g., `--threads 8`. Typically set it to the number of physical cores. If you fully offload to GPU, you might reduce threads to 1 to avoid unnecessary CPU load ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=number%20of%20threads%20during%20BLAS,and%20should%20not%20be%20used)). There’s also `--blasthreads <M>` which if set will use a different thread count specifically for BLAS (matrix multiply) operations ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Set%20number%20of%20threads%20to,and%20should%20not%20be%20used)), though by default it mirrors `--threads` unless overridden.
- **GPU Acceleration Backends:** By default KoboldCpp tries to auto-select the “best” available backend for speed ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=By%20default%2C%20launching%20with%20no,options%20to%20Make%20It%20Fast)). However, you can explicitly choose:
  - `--usecublas` – use NVIDIA cuBLAS for GPU acceleration (requires an NVIDIA GPU and CUDA) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,for%20even%20faster%20GPU%20offloading)).
  - `--useclblast [platform_id] [device_id]` – use OpenCL via CLBlast for GPU (works with AMD, Intel, or Nvidia GPUs) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=other%20platforms%20,See%20compiling%20with%20Metal)). If you omit the optional IDs, it will list available platforms/devices in the console and usually default to 0,0 (first GPU) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=The%20two%20values%20to%20use,which%20will%20display%20the%20platform)). You can specify the platform and device index if you have multiple GPUs or OpenCL devices ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=The%20two%20values%20to%20use,which%20will%20display%20the%20platform)).
  - `--usevulkan` – use Vulkan for GPU. This backend works on many GPUs (Nvidia and AMD) and can be a good alternative to OpenCL ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,compared%20to%20the%20OpenCL%20backend)).
  - `--usemetal` – (Mac only) use Apple Metal for GPU on Apple Silicon ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=with%20%60,decent%20speeds%20by%20selecting%20the)).
  - `--usecpu` – force CPU-only mode ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,mode%2C%20everything%20is%20handled%20automatically)). By default if no GPU flag is given, it’s on CPU; this flag explicitly ensures only CPU (disables auto-detection).
  - *Note:* **cuBLAS** generally yields the fastest performance on NVIDIA cards, and **Metal** on Apple Silicon, whereas **CLBlast** and **Vulkan** are cross-vendor options. Only one of these “useX” flags should be specified at a time.
- **GPU Layer Offloading:** `--gpulayers <N>` – after choosing a GPU backend, this setting offloads *N* transformer layers of the model to the GPU memory for inference ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20does%20GPU%20layer%20offloading,many%20layers%20can%20I%20offload)). Offloading layers means those layers’ weight matrices will reside and be computed on GPU, reducing CPU load and speeding up generation. The number you can offload depends on your GPU VRAM and model size ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Just%20running%20with%20%60,RTX%202060%20can%20comfortably%20offload)). For example, a 6GB VRAM GPU can offload ~32 layers of a 7B model, or ~18 layers of a 13B model at 2048 context Q4_0 ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%28CUDA%2FCL%2FMetal%29%20that%20you%20are%20using,RTX%202060%20can%20comfortably%20offload)). If you set `--gpulayers -1`, KoboldCpp will try to “guess” the maximum layers it can offload automatically ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,and%20error%20for%20best%20results)), though manual tuning is often better. If not using GPU at all, you can ignore this. If using GPU, a good approach is to start with a low number and increase until you nearly exhaust VRAM (if you go too high and run out of VRAM, the program may error or swap back to CPU).
- **Memory Mapping:** `--usemmap` – enables memory-mapping the model file from disk ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20mmap)). This can reduce initial RAM usage since parts of the model are loaded on-demand from disk. It’s useful if you have limited RAM or want to load a model larger than RAM (with slower disk reads). On an SSD, mmap can be fairly fast. Without mmap, the entire model file is loaded into RAM upfront.
- **Lock Memory:** `--usemlock` – locks the model in RAM to prevent it from being swapped out by the OS ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=mmap%2C%20or%20memory,usemmap)). This is useful on Linux where inactive pages might be swapped – mlock keeps model data resident. If you have sufficient RAM, enabling mlock can prevent slowdowns due to swap. (Requires appropriate permissions; on Linux might need `ulimit -l` increase or sudo.)
- **Low VRAM for GPU:** Adding `lowvram` after `--usecublas` (like `--usecublas --lowvram`) reduces GPU memory usage by not offloading certain buffers (scratch/KV) to GPU ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20does%20lowvram%20do%20for,CuBLAS)). This can help fit models on smaller VRAM at the cost of some speed. Note: as of newer KoboldCpp versions, for latest GGUF models, `lowvram` might not have effect unless running older models ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=lowvram%20can%20be%20added%20to,offloaded%20at%20all%20if%20enabled)). But it has been re-enabled as of Jan 2024 to prevent KV cache on GPU if needed ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=lowvram%20can%20be%20added%20to,offloaded%20at%20all%20if%20enabled)).
- **MMQ (Quantized MatMul):** By default, KoboldCpp (with cuBLAS) enables an optimization called “MMQ” – quantized matrix multiplication kernels for prompt processing ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20does%20Quantized%20Mat%20Mul,do%20for%20CuBLAS)). This can slightly speed up prompt ingestion for 4-bit models and reduce memory usage. If needed, `--nommq` will disable it (falling back to standard FP16 cuBLAS multiplication) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,disable%20it%20instead%20if%20unwanted)). Generally you won’t need to change this unless you benchmark differences – the default (enabled) is fine for most cases.
- **Split for Multi-GPU:** `--tensor_split <r1> <r2> [...]` – if you have multiple GPUs (Nvidia) and use `--usecublas` without specifying a device, KoboldCpp will automatically distribute the model across all detected GPUs ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=How%20do%20I%20use%20multiple,GPUs)). The default split is equal or based on GPU VRAM sizes, but you can manually set ratios with this flag. For example, `--tensor_split 3 1` would split 75%/25% between two GPUs ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Multi,ratio)). *Row vs Layer Splitting:* Internally, KoboldCpp can split either by layers or by rows across GPUs. By default it uses layer-split which is usually best, but you can try row-split via an environment variable if needed (this is advanced; generally stick to layer split) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%27s%20the%20difference%20between%20row,and%20layer%20split)).
- **No AVX2:** `--noavx2` – as mentioned, for older CPUs. Forces use of slower instructions if your CPU crashes otherwise ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20older%20devices%20do%20not,all%20CPU%20intrinsics%20and%20GPU)).
- **Disable BLAS:** `--noblas` – if for some reason the optimized BLAS is causing issues or you want to force pure C++ math, this turns off the accelerated BLAS library usage ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Metal,Vulkan%20is%20a)). (On Mac, if Accelerate causes slowdowns for quantized models, this might help, but usually not needed.)

### 5.3 UI and Server Behavior Flags

- **`--port <num>`** – specify the server port (default is 5001) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20port%20does%20Koboldcpp%20use%3F,the%20port%20that%20koboldcpp%20uses)). Use this if 5001 is in use or you want multiple instances. E.g. `--port 5002`.
- **`--host <IP>`** – bind the server to a specific host/IP. By default KoboldCpp might bind to `127.0.0.1` (local only). If you want to allow other devices on your LAN to connect, you can bind to `0.0.0.0` (all interfaces) or your local network IP ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=3,over%20the%20internet%20as%20well)). For example, `--host 0.0.0.0` along with `--port 5001` will let you access KoboldCpp from other machines on your network (ensure your firewall allows it). If you plan to expose it over the internet, consider using a tunnel or proper security (discussed in *Network Access* section).
- **`--password <pass>`** – require a client API key for using the KoboldCpp server ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20add%20authentication%3F)). This flag enables simple authentication. When set, any API request must include an `Authorization: Bearer <pass>` header (or use it via the UI’s prompt for API key). This is useful if you open your KoboldCpp to the network and want to restrict access ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20add%20authentication%3F)). For example, `--password mysecret` will make the server expect “mysecret” as the key.
- **`--multiuser [N]`** – enable or configure **Multi-User mode**. By default, multiuser mode is **on** in KoboldCpp ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)). This mode allows multiple clients (browsers or API users) to connect to the same KoboldCpp instance simultaneously. It queues and handles requests so that responses go to the correct user session. You can specify a number `N` to limit max simultaneous users; e.g. `--multiuser 5` allows up to 5 active user connections. `--multiuser 0` would disable multiuser mode (single-user only) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Multiuser%20mode%20allows%20multiple%20people,to%20disable%20this)). Generally, leave this on if you might connect from multiple devices or share the instance.
- **`--admin`** – enables **Admin Mode**, which allows live switching of models and settings via an admin panel in the UI ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20Admin%20mode%3F%20Can,I%20switch%20models%20at%20runtime)). Admin mode requires you to also specify an `--admindir` (directory containing saved config files `.kcpps`) for available configurations, and optionally `--adminpassword` to secure the admin panel ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20Admin%20mode%3F%20Can,I%20switch%20models%20at%20runtime)). With admin mode, you can remotely change the loaded model or other launch params without restarting the process, which is useful for power users hosting multiple models. If you don’t need to hot-swap models, you can ignore this.
- **`--quiet`** – run in quiet mode, which suppresses logging of prompts and generations in the console ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)). Normally, the terminal will echo the text of prompts and outputs (for debugging). `--quiet` prevents that, which can be good for privacy or cleaner logs ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)).
- **`--foreground`** – on Windows, forces the console window to come to the foreground whenever a new generation starts ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)). This was added to avoid Windows potentially deprioritizing the process if it’s in the background. If you notice slowdowns because the console isn’t active, try `--foreground` (Windows only) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)).
- **`--showgui`** – by default, if you launch with command-line flags, the program might skip showing the launcher GUI and go straight headless. `--showgui` forces the GUI to appear even when flags are provided ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)). What it actually does is launch the GUI with those flags pre-filled, allowing you to visually confirm or tweak before final start ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)).
- **`--config <file.kcpps>`** – load a saved configuration file. KoboldCpp’s launcher can save profiles of settings in `.kcpps` files (full configs) or `.kcppt` (templates) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60,kcpps%20files)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20are%20)). Using `--config` lets you apply one of those presets directly on launch. This is handy to store different configs for different models. Similarly `--exportconfig` will save current flags to a file, and `--exporttemplate` saves a shareable template (excluding device-specific settings) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,exportconfig%60%20flag)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20are%20)).
- **`--preloadstory <file.json>`** – supply a KoboldAI Lite JSON save file to preload on server start ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)). This is an advanced feature that auto-loads a story (including memory, world info, etc.) for any client that connects. For example, if you want to share a scenario with someone connecting to your instance, you could launch with your pre-written story file, and they will see it on connect ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)).
- **`--savedatafile <database.db>`** – this sets a server-side database file for saving stories/sessions ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20save%20stories%20to,Server%20Side%20Saves)). By default, when using the web UI, any saved stories are stored in the browser (client-side). If you want server-side persistence (so that different clients or sessions can access the same saves), you can designate a SQLite DB file with this flag. Then the “Save” in UI will store into that DB on the server.
- **`--unpack`** – if you are using the PyInstaller one-file binary, this flag will unpack the internal files to a directory for inspection ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)). It’s mainly for developers who want to modify or replace internal files (like the UI HTML or other resources). After unpacking, you would run via the `koboldcpp.py` directly in that folder.

### 5.4 Generation Parameters and Sampling Flags

Most text generation tuning (like temperature, top-k, etc.) are set within the UI or via API requests, rather than fixed at launch. However, some default behaviors can be influenced by flags:

- **`--defaultgenamount <N>`** – sets a default max generation length (tokens) if the client doesn’t specify one ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=With%20Chat%20Completions%2C%20how%20do,many%20tokens%20the%20AI%20outputs)). For example, some third-party apps might not send a “max_tokens” in the OpenAI API calls; this prevents endless generation by using a safe default like 200 or so. If you find the AI stops too early or runs too long by default in certain integrations, you can adjust this.
- **`--usemirostat`** – *Deprecated* flag for enabling Mirostat sampling. Instead, you can set Mirostat in the UI or payload if needed. (Mirostat is described in the Advanced section, but the flag was deprecated ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=loses%20coherence,and%20should%20not%20be%20used))).
- **`--chatcompletionsadapter <file.json>`** – This flag is used to specify an *adapter file* to customize how the OpenAI Chat Completions API maps to the model prompt format ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=OpenAI%20compatible%20Chat%20Completions%20API,format%2C%20all%20fields%20are%20optional)). Typically not needed unless you use the OpenAI endpoint heavily and need to fine-tune how system/user/assistant messages are formatted for your model. The adapter JSON can define templates.
- **`--nsfw`** – (Not sure if present in KoboldCpp; some UIs have an NSFW filter toggle. KoboldCpp itself does not censor by default).
- **`--unbantokens`** – *Deprecated*; previously used to unban the end-of-stream token forcing the model to never stop (leading to “rambling”). This is now handled via UI setting “EOS Token Ban” instead ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20models%20will%20use%20a,be%20used%20in%20Story%20mode)).

Finally, note that any of these flags can also be set via the Launcher GUI’s fields or checkboxes (they correspond to options in the GUI). You can mix GUI and CLI: for example, you could launch the GUI with some flags pre-set using `--showgui`, or just always use CLI after testing out in GUI.

Now that we have KoboldCpp up and running, we can connect to its Web UI to chat or write stories with the AI.

## 6. Performance Tuning and Hardware Acceleration

Optimizing KoboldCpp’s performance involves choosing the right quantization for models, leveraging your hardware (CPU/GPU) effectively, and tuning parameters like threads and batching. Here we cover strategies for speeding up inference and making the best use of available resources.

### 6.1 CPU vs. GPU Execution

**CPU-Only Performance:** If you have no suitable GPU, KoboldCpp will run on CPU using multi-threaded matrix math. For best CPU speed, ensure you compiled with an optimized BLAS (Basic Linear Algebra Subprograms) backend. On x86 CPUs, this is usually handled by Xbyak (just-in-time AVX2) in llama.cpp by default, and linking OpenBLAS is no longer needed (OpenBLAS support was removed in favor of built-in) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=newer%20option%20that%20provides%20a,mode%2C%20everything%20is%20handled%20automatically)). On Apple, the Accelerate framework will be used automatically. The key parameter for CPU is the number of `--threads`. Too few threads underutilizes cores; too many can cause contention – best is around # of physical cores (or a little less) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=How%20to%20choose%20how%20many,blasthreads)). For example, a 6-core/12-thread CPU might do best with 6 threads for inference ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Set%20number%20of%20threads%20to,and%20should%20not%20be%20used)).

**GPU Acceleration:** KoboldCpp can significantly speed up inference using GPU libraries:
- On **NVIDIA GPUs**, use `--usecublas` to enable CUDA acceleration. This uses the GPU for both the initial prompt processing and optionally for generating tokens if layers are offloaded. Offloading layers (`--gpulayers`) is crucial to get per-token speedup, not just prompt speedup ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Just%20running%20with%20%60,RTX%202060%20can%20comfortably%20offload)). Even offloading, the model may not fully reside on GPU due to memory limits; it will split work between CPU/GPU.
- On **AMD GPUs** (or Intel iGPUs, or NVIDIA as well), `--useclblast` or `--usevulkan` can be used. Vulkan tends to give a good balance of speed and compatibility for AMD especially ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,compared%20to%20the%20OpenCL%20backend)). CLBlast uses OpenCL which works but might require selecting the correct device ID if you have multiple GPUs ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=The%20two%20values%20to%20use,which%20will%20display%20the%20platform)).
- On **Apple Silicon**, no flags are needed if you compiled with Metal; it will use the Metal backend. This offloads most work to the Apple GPU seamlessly (you can still control threads for any CPU part).
- Generally, **cuBLAS** is the fastest on NVIDIA, often followed by Vulkan on AMD. OpenCL (CLBlast) can be slightly slower but still better than CPU for large models. If one backend gives issues or suboptimal speed, try another (e.g., some have reported Vulkan can beat CLBlast on certain systems).

**Layer Offloading:** The number of layers to offload (`--gpulayers`) should be tuned empirically. Start low and increase until you approach your GPU’s VRAM limits. If you over-allocate, the program might crash or revert to CPU. KoboldCpp’s documentation provides a reference: with a 6GB GPU and Q4_0 2048ctx, about 32 layers of a 7B fit, 18 of 13B, 8 of 30B ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%28CUDA%2FCL%2FMetal%29%20that%20you%20are%20using,RTX%202060%20can%20comfortably%20offload)). If you have an 8GB card, you might offload ~40 layers of 7B, ~20+ of 13B, etc. Each layer of a 13B model is bigger than a layer of a 7B, hence fewer fit. Offloading as many layers as possible typically yields the best speed (the final few layers might give diminishing returns though). You can also try `--gpulayers -1` for auto mode, but it might not guess perfectly, especially if multi-GPU ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=match%20at%20L418%20,and%20error%20for%20best%20results)).

**Multi-GPU:** If you have multiple GPUs (Nvidia), KoboldCpp will automatically distribute across them when using `--usecublas` without a specific device ID ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=How%20do%20I%20use%20multiple,GPUs)). It splits model tensors between cards. You can monitor the GPU memory usage on each to see the split. If one card has more VRAM, you can adjust with `--tensor_split` ratios ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Multi,ratio)). Also, if one card is much slower (older), you might exclude it by selecting the faster one explicitly. Currently, multi-GPU is only officially supported with cuBLAS (Nvidia) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=How%20do%20I%20use%20multiple,GPUs)).

**Memory Considerations:** Offloading to GPU reduces *RAM* usage since a portion of the model resides in VRAM. If you have limited system RAM but a decent GPU, offloading can allow running a model that otherwise wouldn’t fit in RAM. For example, a 13B model needs ~16GB RAM in Q4_0, but if you offload half its layers to a GPU, you might cut RAM usage by a few GB ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,least%2064GB%20RAM)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Offloading%20layers%20to%20the%20GPU,section%20on%20GPU%20layer%20offloading)). However, GPU VRAM becomes the limiting factor.

### 6.2 Quantization Impact on Performance

Quantization affects speed and memory. **Lower-bit models (e.g., Q4, Q5)** not only use less memory but also perform less computation per token, often resulting in faster generation. For instance, a Q4_0 model will generally run faster than an F16 model of the same architecture because there’s less data to multiply. So if you need speed, using a smaller quantization (with acceptable quality trade-off) can help. According to discussions, Q4_0 is faster than Q5 or Q6 since it’s fewer bits, but Q6_K or Q5 can sometimes slightly slow generation due to extra calculation complexity. The differences aren’t huge, but if you’re purely optimizing speed, Q4_0 or Q4_K_S might be one of the fastest quantized options (besides extreme Q3 which suffers in output quality). 

That said, quantization’s effect is more on memory; actual *throughput* is often bound by compute (where GPU helps most). If using CPU, a Q8_0 might be slower than Q4 because it processes twice the data. If using GPU, the GPU may handle 8-bit and 4-bit with different efficiency. In general: choose the lowest precision that still gives you acceptable output quality, to maximize speed.

### 6.3 BLAS, Batch Size, and Advanced Settings

**BLAS Batch Size:** KoboldCpp uses batch processing for prompt ingestion. The parameter `--blasbatchsize` controls how many tokens of the prompt are processed at once in matrix ops ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=BLAS%20,generation)). The default is 512 for LLaMA models (256 for others) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=There%20are%20multiple%20backends%20this,generation)). A higher batch means more memory use but faster prompt loading; a lower batch can reduce peak memory but might slow down prompt processing. If you have *very low RAM* or see an out-of-memory during prompt, you could lower this (e.g., `--blasbatchsize 128`) to trade speed for memory ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=There%20are%20multiple%20backends%20this,generation)). For most, leave it default.

**Flash Attention:** `--flashattention` enables a faster attention mechanism when using GPU (cuBLAS) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Flash%20Attention)). FlashAttention can significantly speed up attention computation for large context windows by using efficient GPU kernels. If you compiled with support, turning it on can improve throughput and memory usage (especially with long contexts). It’s generally beneficial on supported GPUs.

**Quantized KV Cache:** By default, the key-value cache (the model’s memory of past tokens) is in FP16. You can use `--quantkv 1` or `2` to quantize the cache to 8-bit or 4-bit ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Quantized%20KV%20Cache)), saving memory at a potential minor hit to generation coherence. Note: fully quantizing both key and value cache requires FlashAttention to be enabled; otherwise only one of them will be quantized ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=You%20can%20now%20utilize%20the,K%20cache%20can%20be%20quantized)). This feature is experimental but can help keep memory use low for long conversations.

**RoPE and Extended Context:** If you raise `--contextsize` above the model’s trained context (usually 2048), KoboldCpp will by default apply NTK-aware RoPE scaling automatically ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=RoPE%20scaling%20%28via%20%60,can%20be%20combined%20if%20desired)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%2A%20NTK,contextsize%60%20value)). This is a technique that stretches out the position encoding to allow the model to utilize longer contexts at some cost in precision. You can manually control it with `--ropeconfig <scale> <base>` if needed ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=RoPE%20scaling%20%28via%20%60,can%20be%20combined%20if%20desired)):
- Example: `--ropeconfig 0.5 10000` would double context length (linear scaling) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,contextsize%60%20value)).
- Example: `--ropeconfig 1.0 32000` uses NTK scaling to roughly double context (since base 32000 vs default 10000) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%60,contextsize%60%20value)).
If using a model fine-tuned for longer context (like a 4K or 8K fine-tune), you might supply specific rope settings as given by the model author.

**Avoiding Bottlenecks:** If you notice generation is *slower than expected*, check a few things:
- Are you accidentally running on a single thread? (Use `--threads` appropriately).
- Is the GPU being utilized? (Use GPU monitoring tools. If not, perhaps a flag was missed or there’s an issue).
- For NVIDIA, ensure you’re not using an older driver that slows down compute.
- If CPU-bound, check that AVX2 is being used (if your CPU supports it). In PyInstaller builds, it should automatically use AVX2 unless `--noavx2` was invoked.
- If running on battery power (laptops), ensure high performance mode – these models max out hardware.

**Benchmark Mode:** KoboldCpp provides `--benchmark` which will run an automated speed test and output the results ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20benchmark%20my%20system,performance)). This can help you measure tokens per second etc. You can even combine it with `--prompt "text"` to benchmark on a custom prompt ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20%60)).

In summary, for best performance:
- Use GPU acceleration if available, with as many `--gpulayers` as fits.
- Use a reasonable quantization for model size (to fit in memory).
- Set `--threads` optimally for any CPU portion.
- Consider FlashAttention for large contexts.
- Monitor memory (RAM/VRAM) to avoid swapping or OOM issues (use `--usemmap` if low on RAM, or `--lowvram` if low on VRAM).
- Keep software (drivers, KoboldCpp version) up to date, as optimizations are continually added.

## 7. Web UI and Usage Modes

One of the standout features of KoboldCpp is its built-in web user interface, known as **Kobold Lite**. This UI is what allows users to easily interact with the model in various modes (story writing, chat, etc.) through a browser, without needing to write code or deal with JSON inputs. In this section, we explore the Web UI’s capabilities and the different usage modes it supports.

### 7.1 Accessing the Web UI (Kobold Lite)

Once KoboldCpp is launched (and the model finished loading), you will see a message in the console indicating the server is running on a certain address (e.g. `http://localhost:5001`). Open your web browser and navigate to that URL ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Kobold%20Lite%20is%20a%20lightweight%2C,Kobold%20Lite%20will%20be%20launched)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=pre,Kobold%20Lite%20will%20be%20launched)). The Kobold Lite interface will load. No separate installation or internet connection is required – the UI is served directly from the KoboldCpp application.

If you are running KoboldCpp on another machine (say a local server or another PC on your LAN), you would use that machine’s IP and the port. For example, `http://192.168.1.100:5001` (if you allowed it in the launch flags via `--host`). Likewise, if using a phone or tablet on the same network, you can connect using the PC’s IP and port ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=and%20then%20connect%20to%20it,should%20work%20out%20of%20the)). (For remote over internet, see the *Network Access* section).

**Kobold Lite** is a lightweight UI that should open quickly in the browser. It has no external dependencies (it’s all static HTML/JS, so it works offline) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20Kobold%20Lite%3F%20How,do%20I%20use%20it)). If the UI doesn’t load, double-check that KoboldCpp is running and that no firewall is blocking the port.

### 7.2 UI Layout and Features

The Kobold Lite interface typically consists of:
- A text area where the story or conversation is displayed.
- An input box where you type your text or message.
- A send/enter button (or you can press Enter) to submit input.
- Settings panels (which can be toggled open) for controlling parameters, mode, etc.
- Option buttons (like Save, Load, New, etc. for stories, or possibly image/audio controls if those features are enabled).

At the top, you might have buttons for **Memory**, **World Info**, **Authors Note**, etc., especially in Story/Adventure modes. These are features inherited from KoboldAI UI:
  - **Memory:** A field where you can write persistent information that the AI should always keep in context (it’s prepended to the prompt for every generation). Use this to remind the AI of key facts or the theme.
  - **Author’s Note:** A short directive (invisible to the user in story) that influences the style or tone. E.g. “The story is in a spooky tone.”
  - **World Info / Lorebook:** A system to add context based on keywords. You can define terms and their descriptions; if those keywords appear in the story, the description can be injected to help the AI maintain consistency.
  - **Characters/Scenarios:** You might see the ability to load predefined character personalities or scenario scripts. Kobold Lite allows you to start from a scenario file to set the stage for Adventure mode ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,crafted%20Scenario)).

The right-hand side typically has the **Settings** panel. Here, you can find:
  - **Basic Settings:** Which includes the **Format (Mode)** dropdown, a slider for max tokens, a temperature slider, top-k, top-p, etc., depending on UI.
  - **Advanced Settings:** Additional sampling settings like repetition penalty, phrase bans, and toggles for things like “allow generation after end of story” etc.
  - **Style Selector:** A dropdown to change the UI theme style (Classic, Messenger, Aesthetic, Corpo, etc.) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=In%20newer%20Kobold%20Lite%20versions%2C,are%20available%20in%20all%20modes)).
  - **Image/Audio tabs:** If image generation or TTS is enabled, tabs or sections to handle those (like selecting an image to send to AI, or recording audio input).

Many elements of the UI are interactive and have tooltips explaining them. Feel free to explore the UI with test inputs.

### 7.3 Interaction Modes: Story, Chat, Instruct, Adventure

Kobold Lite supports **four primary modes of interaction**. These can be selected usually from a *Format* dropdown in the Basic Settings panel ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Basic%20Modes%20of%20Kobold%20Lite)):

- **Story Mode:** This mode is for long-form creative writing. You enter narrative text, and the AI continues the story from where you left off ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,The%20best%20way%20to)). There’s no strict turn-taking; it’s like cooperative storytelling. For example, you write a paragraph of a novel and the AI writes the next paragraph. The AI will not insert a “you:” or distinguish speakers – it just treats everything as story text. Use this for writing novels, fanfiction, descriptions, etc. The AI in Story mode tries to maintain third-person narrative (or whatever perspective the story is in) and continues the prose.

- **Chat Mode:** This mode simulates a conversation or roleplay between the user and AI (often the AI acting as a character) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,The%20best%20way%20to)). The UI might label user messages as “You:” and AI responses as the character’s name or “Bot:”. Chat mode is turn-based: you input a message, then the AI responds, and so on. It’s great for general Q&A, having a dialogue, or roleplaying a persona. KoboldCpp will handle formatting under the hood such that the model gets an appropriate chat prompt format (it may use special tokens or separators, depending on the model). In Chat mode, by default, the **EOS (End of Stream) token ban** is often disabled or auto to let the model end responses naturally (more on EOS in Troubleshooting).

- **Instruct Mode:** This mode is akin to the style of ChatGPT or instruction-following models ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,The%20best%20way%20to)). You give the AI a direct instruction or question (like “Explain how a bill becomes a law.”) and it will produce an answer/fulfillment of that instruction. The interface might still look like a chat, but the expectation is a single-turn instruction → response. Instruct mode usually formats the prompt differently (often no “you:” prefix, just maybe a system role with the instruction). It might also unban EOS token so the model can stop when it’s done with the instruction ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20models%20will%20use%20a,be%20used%20in%20Story%20mode)). Use Instruct mode for factual questions, step-by-step instructions, or any case where you want a direct, task-oriented answer.

- **Adventure Mode:** This mode emulates a *Choose Your Own Adventure* or AI Dungeon style experience ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,crafted%20Scenario)). Typically, the user’s input is treated as an action or dialogue *within* a story, and the AI narrates the result. For example, you might input: “> Go north towards the abandoned cabin” and the AI responds with the narrative outcome of that action in second-person (“You head north...”). Adventure mode usually expects the user to prefix commands with a special token (like `>`) or it’s implicitly understood. Kobold Lite likely automates this formatting for you. It’s very similar to Story mode except with a frame that the user is the protagonist making actions. This mode works best if you load or write a scenario setup that describes the setting and initial situation. (The UI provides some built-in scenarios you can load to jumpstart an adventure ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,button)).)

**Behind the Scenes:** When you switch modes, the UI adjusts how it structures the text sent to the model. For instance, in Chat mode it might maintain a transcript with roles, in Story mode it just concatenates, in Instruct it might add a prompt prefix like “### Instruction:” depending on the model. This is all handled by KoboldCpp’s prompt templates. So, you should choose the mode that matches your intended interaction for best results.

### 7.4 UI Styles

Kobold Lite offers multiple UI styling options which change the look and feel (and sometimes layout) of the interface ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=UI%20Style%20Select)):
- **Classic:** The default simple text-based interface reminiscent of a text editor or plain chat. All modes support Classic UI ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,and%20feel%20of%20Corporate%20AI)).
- **Messenger:** A chat bubble style interface (like a messaging app) for Chat mode. It visually separates user and AI messages as bubbles, which some find more readable for dialogue ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,and%20feel%20of%20Corporate%20AI)).
- **Aesthetic:** A highly customizable style for Chat and Instruct modes, allowing colored text, custom fonts, portraits for characters, etc. ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,AI%20Assistants%20such%20as%20ChatGPT)). This is great for roleplay or simply making the interface pretty.
- **Corpo:** Mimics a corporate assistant style (like how ChatGPT or Bing might look), with a clean minimalistic design ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,AI%20Assistants%20such%20as%20ChatGPT)).

Not all styles are available in every mode (e.g., Messenger might not apply to Story mode) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=In%20newer%20Kobold%20Lite%20versions%2C,are%20available%20in%20all%20modes)). You can switch styles on the fly in the UI settings. Styles do not affect the model’s behavior, only the presentation.

### 7.5 Using the Web UI: Tips

- **Writing and Editing:** In Story/Adventure modes, you can actually edit the last AI output or your input if you want to change something (unless disabled). Kobold Lite supports editing and regen: you can modify any part of the story and then press a regen button to have the AI continue from the edited text.
- **Memory and Author’s Note:** Use these fields to steer the narrative. For instance, put important facts about the world or character in Memory so they aren’t forgotten. Update it as needed (but keep it concise due to context limits).
- **Scenario Library:** The UI may include a few example scenarios (short pre-written prompts to start a story or game). These are accessible via a “Scenarios” button ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,button)). It’s a quick way to test the system with a known setup.
- **Saving/Loading:** You can save your session/story at any time. This usually downloads a JSON file (for Kobold Lite) or saves in the browser storage. You can reload it later to continue where you left off. If you used `--savedatafile`, it might save on the server side too.
- **Tokenization Display:** Some UIs allow toggling a view of tokens or probabilities (like a debug mode). Kobold Lite might not have this visible by default, but via API you can get token probabilities (logprobs) if needed ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=warning%20for%20bad%20suboptimal%20sampler,203)).
- **Idle or OOC Responses:** In Chat mode, sometimes the AI might produce an out-of-character or extraneous reply if it’s confused (like it might start writing both sides of conversation). The UI has settings like “Chat – Multiline Replies” or “Idle Responses” to handle these ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%2A%20Chat%2FStory%20,218)). If the AI starts replying as the user or going on when it shouldn’t, you might need to correct the prompt format or clear some context.

Overall, the Web UI is designed to make using the AI model as straightforward as possible, whether you want a creative collaborator or a chat partner. It encapsulates many features from the original KoboldAI web client in a lightweight form.

## 8. Advanced Features

KoboldCpp goes beyond basic text generation with a suite of advanced features that enhance context handling, allow multi-modal inputs/outputs, and integrate new research techniques. Below we discuss some of these features and how to utilize them.

### 8.1 Smart Context and Context Shift

**Smart Context:** This was an earlier solution to manage limited context window. By launching with `--smartcontext`, KoboldCpp will reserve roughly 50% of the context length as a *buffer* for reused text ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Smart%20Context%20is%20enabled%20via,and%20%27shift%20up%27%20the)). Essentially, when the conversation/story gets near the maximum context (e.g., near 2048 tokens), Smart Context triggers: the program will automatically drop the oldest half of the conversation and reuse the remaining half as the new prompt prefix so it can continue without a full reprocess ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=How%20it%20works%3A%20when%20enabled%2C,insert%20the%20new%20text%20while)). It does this by sending a back-to-back prompt and generation sequence. The idea is to reduce how often the model has to churn through a full context from scratch, at the cost of halving the effective context window for new content ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Smart%20Context%20is%20enabled%20via,and%20%27shift%20up%27%20the)). 

*Analogy:* The documentation gives a bus analogy – instead of kicking out a few passengers at every stop when full (which is slow because you have to do it every time), Smart Context kicks out half the bus when it gets full, allowing a few stops of new passengers with no kicks, then repeats ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,in%20peace%20as%20there%20will)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=becomes%20slow%20due%20to%20kicking,That%27s%20smartcontext)). In practice, this means the model won’t repeatedly re-read the earliest parts of the conversation each turn; it will only reprocess from a midpoint once triggered, saving time.

Using Smart Context can help if you have a slow machine but want to have longer sessions – it trades some coherence (since only the last half of context is kept in detail) for speed. However, it’s largely superseded by Context Shift for GGUF models.

**Context Shift:** This is a more advanced method available **only for GGUF** models (not older GGML) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Context%20Shifting%20is%20a%20better,outputs%20may%20be%20different%20with)). Context shifting uses an internal technique to remove old tokens from the model’s **KV cache** directly, without needing to reprocess the remaining context. Essentially, it “shifts” the entire context window up by some amount. As long as the conversation isn’t edited mid-way and features like World Info aren’t injecting content at the beginning, context shift can prevent almost all reprocessing when you hit the context limit ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=models,noshift)). It’s like having a sliding window over the conversation.

Context Shift is enabled by default in KoboldCpp for GGUF models ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=max%20context,noshift)). If both SmartContext and ContextShift are on, ContextShift takes precedence (because it’s better) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=max%20context,noshift)). If for some reason you want to disable it (maybe for debugging or a model that doesn’t behave well with it), use `--noshift`. With context shifting active, you can effectively have continuous conversation beyond the nominal context length, with the model dropping the oldest data on the fly. The trade-off is that older context is truly gone (not even in hidden state), but if it was far back, presumably it was not needed. This feature is great for long chats where you don’t want to restart or truncate manually.

**Fast-Forwarding:** This is another context optimization that is **on by default** ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20FastForwarding%3F)). Fast-forwarding means if you send a prompt that contains a lot of the text that was already processed in the previous turn, the model doesn’t recompute those tokens’ embeddings from scratch; it reuses the recent cache. This often applies in chat where each new prompt includes the chat history. KoboldCpp will detect the overlapping tokens and skip re-evaluating them, which speeds up prompt processing in multi-turn conversations ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=second,updates%2C%20but%20requires%20a%20persistent)). If needed, `--nofastforward` can disable it (not commonly necessary).

**Practical usage:** You typically don’t need to manually invoke these features except Smart Context if you want it on older models. For GGUF, context shifting is automatic. The main user action is if you want to allow sessions longer than the model’s base context, increase `--contextsize` to something large (like 4096 or 8192) and let context shift manage it ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=First%2C%20you%20need%20to%20allocate,be%20overriden%20to%20a%20higher)). Keep in mind the memory trade-off: `--contextsize 8192` will allocate a lot more memory for cache (roughly 4× the 2048 context memory), so ensure your RAM can handle it.

### 8.2 Extending and Managing Context (beyond 2048)

Many models are trained on 2048 tokens context by default. KoboldCpp allows you to go beyond this:
- **16K / 32K context:** KoboldCpp supports up to 16k context for GGML models and 32k for GGUF models (the hard maximum) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=e.g.%20%60,value%20beyond%20the%20slider%20range)). If you try to set higher, it won’t allow.
- To use an extended context, set `--contextsize` on launch as mentioned. And in the UI, you may need to override the max tokens slider by clicking the number above it and typing a larger value, since the UI slider max is 2048 by default ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=You%20may%20also%20need%20to,value%20beyond%20the%20slider%20range)).
- **RoPE scaling:** As discussed, enabling larger contexts is possible via linear or NTK scaling of RoPE embeddings ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=RoPE%20scaling%20%28via%20%60,can%20be%20combined%20if%20desired)). KoboldCpp auto-applies NTK scaling if context > 2048 unless overridden ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%2A%20NTK,contextsize%60%20value)). If you’re using a known long-context model (like a 8k fine-tune that expects a certain RoPE base), you might supply the exact rope config given by the model’s documentation. For example, some 8K LLaMA 2 models use linear scale 0.5, others use NTK base 80000, etc. ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,contextsize%60%20value)).
- **Memory/Summary Insertion:** Outside of built-in features, a manual approach to “extend” context is periodically summarizing or noting important points in the Memory field so that even if context shifts, the key info persists. KoboldCpp doesn’t automate summarization, but nothing stops the user from doing it manually if needed.

### 8.3 Streaming Generation (Token Streaming)

By default, when using the web UI, text is generated in a “chunky” way – the UI polls the server and updates every second with new tokens ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%2A%20Polled,Agnaistic%2C%20available%20only%20via%20the)). This is called **Polled Streaming** and is the recommended method for Kobold Lite ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%2A%20Polled,Agnaistic%2C%20available%20only%20via%20the)). It’s simple and works out of the box, giving near-real-time feeling.

There are other streaming modes:
- **Pseudo-Streaming:** An older method where the client requests a certain number of tokens at a time via the API by adding `&streamamount=x` to the request ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,updates%2C%20but%20requires%20a%20persistent)). This reduces the interval between updates, but it had performance overhead and is not recommended now ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,This)).
- **True Streaming (SSE):** This uses Server-Sent Events to push each token as soon as it’s generated ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=tokens%20per%20request,according%20to%20their%20provided%20instructions)). KoboldCpp implements an SSE endpoint (`/api/extra/generate/stream`) for this ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,accurately%20measure%20how%20many%20tokens)). However, the built-in Kobold Lite UI and KoboldAI client do not use SSE by default ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=tokens%20per%20request,according%20to%20their%20provided%20instructions)). Only some third-party UIs like **SillyTavern** or **Agnaistic** support SSE streaming with KoboldCpp ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=append%20a%20,It%20uses%20a%20different%20API)). SSE provides a smoother character-by-character or token-by-token reveal. If using such a client, you would point it to KoboldCpp’s SSE endpoint. For example, SillyTavern can be configured to use streaming mode with Kobold’s API.
- **`--stream` flag:** KoboldCpp used to have a `--stream` flag but it’s deprecated ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=KoboldCpp%20now%20supports%20a%20variety,and%20should%20not%20be%20used)). Now streaming mode is controlled by the client. Kobold Lite toggles streaming internally if you enable it in settings (but again it uses the poll method).

For most users, the default polling (1-second interval) is fine, as the text appears in pieces relatively quickly. If you desire instantaneous per-token printing, consider using a client that supports SSE or use the CLI mode (the CLI will print tokens as they arrive, since it’s directly connected). 

### 8.4 Multi-GPU and High-End Setups

As mentioned, multi-GPU is supported for NVIDIA via `--usecublas`. For those with dual or more GPUs, this can allow loading very large models by splitting across cards. KoboldCpp splits by layers by default ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%27s%20the%20difference%20between%20row,and%20layer%20split)), which tends to utilize each GPU in turn for its chunk of layers. There’s also an environment variable or config to attempt a row-wise split, but generally not needed unless debugging performance issues.

If you have an AMD multi-GPU system, currently there is no built-in multi-GPU for CLBlast/Vulkan in the main KoboldCpp (some forks might attempt it). 

**Memory pooling:** There isn’t an explicit mention, but llama.cpp had introduced a memory pooling for multiple models; in KoboldCpp you’d typically run one model at a time per instance. If you needed to run multiple models concurrently (like one for draft model, one for main – see Speculative decoding below), KoboldCpp handles that with separate flags.

### 8.5 LoRA (Low-Rank Adaptation) Support

**LoRA** adapters are small files that adjust a base model’s weights to add a fine-tuned behavior (like style or specific training). KoboldCpp allows loading a LoRA on top of a model at runtime:
- Use `--lora <lora_file>` to specify a LoRA to apply, and `--lora-base <model_file>` if the LoRA was trained on a base model different from the one you are loading (e.g., if you only have the quantized model loaded but LoRA expects the full base) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20LoRA%20and%20LoRA,Base)). In practice, if you’re applying a LoRA to the model you’re loading, you might not need `--lora-base` unless KoboldCpp cannot apply it due to quantization mismatch.
- The devs caution that using LoRAs dynamically is not optimal – it’s better to merge the LoRA into the model offline (resulting in a new GGUF) for both quality and performance ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20LoRA%20and%20LoRA,Base)). This is because applying LoRA on a quantized model might introduce rounding differences vs if it was applied on an f16 base then quantized ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=LoRA%20is%20an%20adapter%20model,apply%20the%20LoRA%20to%20that)).
- KoboldCpp can load LoRAs in GGUF or older GGML format (make sure you have the right format; LoRAs can also be in safetensor, which would need conversion).

Use-case: If you have a general model and you want to try a LoRA (say a style or a character LoRA) without permanently merging, you can do so. But expect some quality difference from intended if the LoRA wasn’t meant for a quantized model.

### 8.6 Vision (Image) Features: LLaVA and Stable Diffusion

KoboldCpp is unique in adding **multimodal vision** support for compatible LLMs and integrated image generation:

**LLaVA and Image Understanding:** LLaVA (Large Language and Vision Assistant) is a model that can interpret images by using a visual encoding (a projector network) to convert images into embeddings the LLM can understand. KoboldCpp allows you to enable this in any model that was trained for vision (multimodal models) by using the `--mmproj` flag ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20LLaVA%20and%20mmproj)). 

- To use it, you need a **projector file** (.gguf) that matches your model’s architecture. The wiki provides a link to Hugging Face where various projectors are available ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%60,visionmaxres)). For example, if you have a Vicuna 13B model that’s vision-capable, you’d download the Vicuna 13B mmproj GGUF.
- Launch KoboldCpp with `--mmproj <projector.gguf>` along with your model. Once loaded, the UI will have an option (like clicking an image and selecting “Multimodal Vision”) to enable vision mode ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=images%20you%20send%20it,the%20model%20normally%20and%20it)). 
- You can then upload an image in the UI; the model will receive the image’s encoded representation and can output a description or respond about the image content. Essentially, you can ask it “What’s in this picture?” etc.
- The `--visionmaxres <N>` flag can adjust the maximum resolution (in pixels) of images it will process ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=architecture%21%20%28E,visionmaxres)). Higher resolution means more detail but more computation. 

This enables question-answering about images or image-aware chat if the model supports it. Remember, only certain models (those fine-tuned for vision, like LLaVA or some Mistral multimodal variants) will know how to handle the image embeddings.

**Stable Diffusion Integration (Image Generation):** KoboldCpp natively integrates **Stable Diffusion** image generation via *stable-diffusion.cpp*. This means you can load a diffusion model and generate images from text prompts, all within KoboldCpp.

To enable image generation:
- Provide a Stable Diffusion model file. Use `--sdmodel <model.safetensors>` at launch to load it ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20generate%20images%20with,KoboldCpp)). The model can be an SD1.5, SDXL, or related architecture. The integration supports **SD1.5, SDXL, and the newer SD3/Flux** models ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Supported%20flags)). Ensure you have a compatible file; recommended to use FP16 safetensors for quality ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,sdlora%60.%20FP16%20is%20recommended)).
- Once loaded, the UI will gain an “Image” generation tab. You can enter a prompt (and negative prompt, steps, etc.) and generate images. KoboldCpp’s integration provides an API compatible with AUTOMATIC1111 and ComfyUI, meaning you could even connect those interfaces or use them as frontends via the txt2img endpoint ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20generate%20images%20with,KoboldCpp)).
- **Image Gen Flags:** There are several flags for customizing the diffusion backend ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=)):
  - `--sdthreads <N>`: separate thread count for image generation (so you might not want to use all CPU cores if running text and image concurrently) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,a%20Stable%20Diffusion%20LoRA%20model)).
  - `--sdquant`: to load a quantized SD model if you only have one (SD models can be quantized to int8 etc. to save RAM) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,generation%20steps%20and%20resolution%20settings)).
  - `--sdclamped`: safe mode to clamp max resolution and steps for shared use (like if you expose it to others, to avoid someone requesting a huge 4K image that uses all VRAM) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,a%20Stable%20Diffusion%20LoRA%20model)). You can optionally specify a max size after it.
  - `--sdlora <file>`: to load a LoRA for the diffusion model (for styles, characters, etc.) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Disable%20VAE%20tiling)).
  - `--sdloramult <x>`: LoRA strength multiplier ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Set%20a%20custom%20VAE)).
  - `--sdvae <file>`: if you want a custom Variational Autoencoder for the image decoder (else the model’s default or an auto one is used) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Supported%20flags)).
  - `--sdvaeauto`: to use a built-in default VAE (the integration includes a fallback called `TAESD`) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Supported%20flags)).
  - `--sdnotile`: disables VAE tiling, which is an optimization for memory ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=for%20shared%20use,Disable%20VAE%20tiling)).
  - `--sdt5xxl` and `--sdclipl`: for SDXL/Flux models, which also require a text encoder (T5-XXL) and a CLIP vision model. If your SDXL model doesn’t have these baked, you need to load them with these flags ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,L%20model%20as%20well)). Some SDXL safetensors bundle them; if not, you point to the corresponding models.

Once everything is set, you can generate images as you would in a normal SD UI, but the cool part is you can do it in the context of your story or chat. For example, you could have the AI write a scene and then ask it to generate an image of that scene. The UI might allow directly using the text generation as prompt for image or vice versa.

**Interrogation:** Kobold Lite also mentions an “Interrogate” feature for images (getting a description from an image) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,to%20generate%20a%20simple%20description)). There are two methods listed: via AI Horde or via a local endpoint (if you had a reverse diffusion pipeline or BLIP model). Not sure if stable-diffusion.cpp supports interrogation, but possibly not directly. Likely it uses the Horde’s clip interrogator if at all. This is more an aside.

### 8.7 Speech: Whisper and Oobabooga TTS

**Speech-to-Text (Whisper):** KoboldCpp can integrate Whisper models to provide voice input. If you load a Whisper GGML model with `--whispermodel <whisper.bin>` on launch ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20Whisper%3F)), the Kobold Lite UI will show options for microphone input. You can press a Push-to-Talk button or enable VAD (voice activity detection) to have it automatically transcribe your speech to text and send that as your input to the AI ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Whisper%20is%20a%20speech,with%20the%20KoboldCpp%20transcription%20endpoint)). Everything runs locally – the audio is recorded in your browser, processed via WebAssembly perhaps, then sent to KoboldCpp which does the transcription with Whisper and returns the text.

To use:
- Download a Whisper model (tiny, base, small, medium, or large) in GGML format ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Speech%3A%20TTS%20models%20for%20Narration)). The larger the model, the better the accuracy (large = ~1.5GB and more accurate, tiny = ~30MB but only good for very short/simple tasks).
- Launch with `--whispermodel path/to/whisper.bin`.
- In the UI settings, find the **Audio/Voice** section and ensure microphone is enabled (your browser might ask permission for microphone use).
- Choose push-to-talk or hands-free. In push-to-talk, you hold a key or button to record; in hands-free, it will continuously listen and detect when you speak (VAD).
- When you speak, the recognized text will appear in the input box (you might see partial results depending on how they implemented it) and then it will send as if you typed it.

This allows a voice conversation with the AI. Keep in mind transcription takes some time (depending on model size and length of speech).

**Text-to-Speech (TTS with OuteTTS):** KoboldCpp integrates **Oobabooga’s OuteTTS**, which is a model for generating speech from text (so the AI can talk back with a voice). To use it:
- Download two models from the HuggingFace link given ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20OuteTTS%20Text%20To,Speech)): the main OuteTTS model (GGUF format) and a WavTokenizer model. The wav tokenizer is needed to generate the final audio waveform.
- Launch with `--ttsmodel <outetts.gguf>` and `--ttswavtokenizer <wavtokenizer.gguf>` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20OuteTTS%20Text%20To,Speech)).
- In the UI, there will be an option to enable narration or to play responses as audio. You might see a speaker icon or a setting for TTS.
- The first time the AI generates text, it will also generate audio for it (this is slower than text generation; generating a long audio response can take some seconds). The audio will then be playable or auto-play in the browser.
- Flags for TTS: 
  - `--ttsgpu` to run the TTS model on GPU if you have one (recommended for speed) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=WavTokenizer%20GGUF%20which%20you%20can,ttswavtokenizer)).
  - `--ttsthreads <N>` to set threads if using CPU ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=WavTokenizer%20GGUF%20which%20you%20can,ttswavtokenizer)).
  - `--ttsmaxlen <N>` to limit the length (in audio tokens) of the generated speech ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%2A%20Use%20%60,change%20speakers%20or%20use%20voice)). This prevents the TTS from babbling on too long. Adjust according to typical response length.

Now you can have a fully voice conversation: speak to the AI (via Whisper) and hear it respond (via OuteTTS). This is all offline and local.

The quality of TTS depends on the model; OuteTTS is okay, but not as natural as big TTS systems. It might have a somewhat robotic or monotonic sound, but it’s improving. You can possibly choose between voices if the model supports it (the “speakers” in multi-speaker TTS). The API allows changing voice IDs, which might be an advanced use (the wiki hints at “see API documentation to change speakers” ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=%2A%20Use%20%60,change%20speakers%20or%20use%20voice))).

### 8.8 Speculative Decoding (Draft Model)

Speculative decoding is an advanced technique where a smaller “draft” model generates some tokens ahead, and the larger model then either accepts or corrects them, to speed up overall generation. KoboldCpp supports this:
- You load a secondary small model with `--draftmodel <model.gguf>` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Speculative%20Decoding%20)). For example, you might use a 3B model as the draft for a 13B model. The draft model must have the same vocabulary as the main model.
- The idea: the draft model generates say 2 tokens very quickly, and the main model then uses those as a starting point to continue if they are likely, thereby skipping some work. This can boost throughput if the draft is much faster.
- Additional flags:
  - `--draftgpulayers <N>` – you can offload layers of the draft model to GPU too ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=speed%20up%20inference%20by%20guessing,select%20the%20speculative%20decoding%20model)).
  - `--draftgpusplit ...` – if multi-GPU, how to split the draft model (usually not needed if it’s small) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=speed%20up%20inference%20by%20guessing,select%20the%20speculative%20decoding%20model)).
- This is an experimental feature. If the draft model’s suggestions are poor, it could degrade quality. It’s mainly for speed freaks who want every bit of performance.

### 8.9 Mixture-of-Experts (MoE) Models

Some large models use Mixture-of-Experts layers (like some Switch-Transformer variants). KoboldCpp has a flag `--moeexperts <N>` to override the number of experts used ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Overriding%20MoE%20models)). This is quite niche – unless you specifically have a model with MoE and want to limit experts to save memory or test performance, you won’t use this. It’s available if needed.

### 8.10 OpenAI/ChatGPT Emulation (Reverse Proxy Mode)

KoboldCpp can act like an OpenAI model for compatibility (this overlaps with the API integration section, but we mention it as a feature):
- Using the OpenAI-compatible API (enabled by default in recent versions at `/v1/chat/completions` and `/v1/completions`) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Is%20there%20an%20OpenAI%20API%3F)), you can point OpenAI-dependent apps to KoboldCpp by setting the API base URL to your KoboldCpp instance and using a dummy API key (or the `--password` key).
- This way, you can use local models with software that normally requires an OpenAI API key (if that software allows changing the endpoint, e.g., some chat UIs, plugins, etc.). More in the API section to follow.

### 8.11 “Admin Mode” for Live Model Switching

The `--admin` mode previously mentioned allows you to host an “admin panel” on the Kobold Lite UI where you can select from prepared configs to switch model or settings without restarting ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20Admin%20mode%3F%20Can,I%20switch%20models%20at%20runtime)). This is advanced but useful if you want to remotely control the instance or quickly swap between models (say 7B and 13B) depending on need. Just remember to provide the directory of configs (`--admindir`) and possibly a password (`--adminpassword`) for safety ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=You%20can%20switch%20models%2C%20settings,relaunch%20to%20a%20new%20config)).

### 8.12 WebSearch and Document Q&A

KoboldCpp introduced a **WebSearch proxy** and a rudimentary **TextDB** for documents:
- **WebSearch:** If launched with `--websearch`, KoboldCpp can handle a special endpoint to perform a web search query (using DuckDuckGo) and feed results into the model ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20WebSearch)). This is meant to augment the model with real-time info. Kobold Lite doesn’t use it by default, but third-party clients might. Essentially, your local KoboldCpp can fetch web content for the model when asked, acting like a mini-search engine. Use responsibly since it’s actually performing internet searches.
- **TextDB (Document Q&A):** In Kobold Lite UI, under Context > TextDB, you can paste a large text (like an article or some notes). KoboldCpp will index it and then, when you ask questions, it retrieves relevant chunks and appends to context ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20Talk%20To%20or,Search%20my%20Documents)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=KoboldCpp%20offers%20a%20TextDB%20Document,minisearch%20for%20retrieval%20scoring%20instead)). This is a form of Retrieval-Augmented Generation. It’s not a vector database but uses a simpler search (lunr.js) under the hood to find similar text. It allows you to “chat with your documents” in a basic way. No separate embedding model is needed for this as it uses pure text matching.

These features allow some degree of interactivity with outside data, making KoboldCpp more versatile (you can update the knowledge base by searching or adding docs on the fly).

This covers many advanced features. As development continues, more could be added, but already KoboldCpp stands out by integrating these capabilities in one package.

## 9. Network Access and Remote Use

While KoboldCpp is designed to run locally, you may want to access it from another device or allow others to use your instance. There are a few methods to use KoboldCpp over a network.

### 9.1 Local Network (LAN) Access

If your PC running KoboldCpp and the device you want to use (e.g., a laptop, phone, or VR headset) are on the same local network (Wi-Fi or Ethernet), you can connect to KoboldCpp by IP:
- By default, KoboldCpp’s web server might only bind to localhost (127.0.0.1). To allow other devices, you should launch with `--host 0.0.0.0` (bind all interfaces) or `--host [your local IP]` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=3,over%20the%20internet%20as%20well)). Example: `koboldcpp ... --host 0.0.0.0 --port 5001`.
- Make sure your firewall isn’t blocking the port. On Windows, when first running, allow KoboldCpp through the firewall for private networks.
- Find your PC’s LAN IP (e.g., 192.168.x.x). On the other device’s browser, go to `http://<PC_IP>:5001`. You should see the Kobold Lite UI and can use it as if on the host.
- If this doesn’t connect, double-check the host flag and firewall. KoboldCpp’s console will also print if it’s listening on 0.0.0.0 or only localhost.
- Once working, you can effectively chat using your phone or another computer anywhere in your house.

**Note:** Multi-user mode being on means multiple devices connecting won’t interfere with each other’s chats; each will be queued appropriately ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Multiuser%20mode%20allows%20multiple%20people,to%20disable%20this)).

### 9.2 Access Over the Internet

Directly exposing KoboldCpp to the internet is **not recommended** for security reasons (and because your home IP being public has risks). However, if you need to use it remotely (say from your office or on mobile data), you have a couple of options:

**Cloudflared Tunnel:** The KoboldCpp repository includes a helper to use Cloudflare’s Argo Tunnel (which provides a quick way to host a service without opening ports on your router). 
- In the repo, there is a `Remote-Link.cmd` script for Windows that, when run, starts a Cloudflare tunnel and gives you a temporary `trycloudflare.com` URL ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=1,version%20of%20Kobold%20Lite%20at)).
- Alternatively, if using Linux or Mac, you can install `cloudflared` and run `cloudflared tunnel --url http://localhost:5001`.
- Newer KoboldCpp versions have a direct flag `--remotetunnel` which will internally start a cloudflared tunnel for you when launching ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=1,You%20can%20start%20a%20worker)).
- Once the tunnel is up, it will output a URL like `https://abcdef-trycloudflare.com`. You can open that URL from anywhere and it proxies to your KoboldCpp. This URL is not guessable, but note that it’s essentially public (anyone who knows it can access). Use it carefully and consider adding `--password` to require a key if using this method ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20add%20authentication%3F)).
- Cloudflared is convenient because you don’t have to mess with router settings or worry about your ISP blocking ports.

**Port Forwarding:** The traditional way is to forward port 5001 on your router to your PC, and perhaps use a dynamic DNS to get an address. If you do this:
- Definitely set `--password` so that not just anyone can use your model ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20add%20authentication%3F)).
- Possibly serve over HTTPS (`--ssl cert.pem key.pem`) if exposing to internet to encrypt traffic ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=You%20can%20now%20import%20your,your%20own%20self%20signed%20certificate)).
- Share the address carefully. It might be http://your-public-ip:5001 or a DNS name if you have one. This is more advanced and outside the scope to detail fully, and again, be cautious as this can expose you to potential misuse or attacks.

**AI Horde (Remote Access via Horde):** A novel approach if you want someone (or yourself remotely) to use your model without direct network exposure is using the **AI Horde**. KoboldCpp has an embedded Horde worker mode (discussed in Integrations) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=there%27s%20a%20helper%20command%20to,85%3A5001)). In short:
- You run `--hordeworker` (implied by providing hordekey etc.) on KoboldCpp to join the volunteer network.
- On another device, you can use a client that connects to AI Horde (like the KoboldAI Lite *web* version at lite.koboldai.net) and specifically request your model by the name you set ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,an%20AI%20Horde%20API%20key)). Because you’re running a worker serving that model, the Horde will route your requests to it.
- This method means your model’s responses go through the Horde server (it’s anonymized and distributed). It’s a bit complex to set up, but it avoids direct connection and also lets you contribute to the community if you want.

For personal remote use, Cloudflare tunnel is probably easiest. Many users use it to chat with their home KoboldCpp from their phone on the go.

### 9.3 API Access

Aside from the web UI, you might want to use KoboldCpp via API from another program or script. KoboldCpp implements the **KoboldAI API** and partial **OpenAI API**:
- **KoboldAI API:** The endpoints are under `/api`. For example:
  - `/api/v1/generate` expects a JSON with prompt and parameters and returns generated text ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=How%20can%20I%20use%20the,from%20the%20KoboldAI%20United%20API)).
  - `/api/v1/model` can list model info, `/api/v1/config/...` to get config values.
  - Additional ones like `/api/extra/generate/stream` (for SSE streaming) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,accurately%20measure%20how%20many%20tokens)), `/api/extra/tokencount` (to count tokens of a string) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=progress%20generation%20,length%20loaded%20from%20the%20launcher)), `/api/extra/perf` (to get performance metrics) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,the%20actual%20ctx%20length%20loaded)), etc. These help integrate with clients like SillyTavern which expect those endpoints.
  - The KoboldAI API is richer in features (supports all the sampling settings, memory, etc.), so if writing a custom integration, prefer this over the OpenAI one ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Yes%2C%20as%20of%20v1,it%20has%20many%20more%20settings)).
- **OpenAI-Compatible API:** Under `/v1` path (like OpenAI’s), e.g. `POST /v1/completions` or `/v1/chat/completions` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Yes%2C%20as%20of%20v1,You%27re%20still%20recommended%20to)). This expects payloads similar to OpenAI’s, e.g. a JSON with `model`, `messages` (for chat) or `prompt` (for completions) and returns a response in OpenAI format. KoboldCpp added this in v1.45.2 ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Is%20there%20an%20OpenAI%20API%3F)). It allows using KoboldCpp with tools that only know how to talk to an OpenAI API (for example, some chat UIs or plugins).
  - For chat, you’d send system/user messages; KoboldCpp internally formats them to its own prompt style.
  - Not all OpenAI fields are supported but basic ones like temperature, max_tokens should work. The adapter system (`--chatcompletionsadapter`) can customize formatting if needed ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=OpenAI%20compatible%20Chat%20Completions%20API,format%2C%20all%20fields%20are%20optional)).
- **Ollama API:** There’s also an Ollama compatibility (Ollama is another local LLM runner). If an app specifically expects an Ollama interface, KoboldCpp has a basic implementation toggled via a flag (possibly auto, or maybe a separate endpoint).
- **Connecting with third-party clients:** Many community chat UIs support connecting to a “KoboldAI API URL”. For example, SillyTavern’s docs show how to configure a KoboldAI endpoint ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=SillyTavern%20and%20Agnaistic%20are%20third,and%20SillyTavern%20together%20with%20KoboldCpp)). You’d use `http://<server>:5001` as the root, and possibly need to enter an API key if you set `--password`. Once connected, you can chat in those UIs using KoboldCpp as the backend.
- Always ensure if you expose the API outside your machine, you secure it (with `--password` or behind a VPN etc.), because otherwise someone could abuse your model or even consume your system resources.

One more network-related feature:
- **SSL/HTTPS:** If you need secure remote access, use `--ssl cert.pem key.pem` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=You%20can%20now%20import%20your,your%20own%20self%20signed%20certificate)) to have KoboldCpp serve over HTTPS. You need a proper certificate (self-signed or from Let’s Encrypt). This is advanced, but it means your traffic is encrypted. Without it, the communication is plain HTTP.

In summary, network access can range from simple LAN use to global access. Choose a method that fits your security comfort. The easiest safe way for personal use is a Cloudflare tunnel with a password set on KoboldCpp, giving a secret URL and required key to use it.

## 10. API Integration (KoboldAI, OpenAI, Ollama, etc.)

KoboldCpp’s flexibility extends to working as a backend for various AI frontends or services by emulating their APIs:

### 10.1 KoboldAI API Integration

The **KoboldAI API** is the native interface originally used by the KoboldAI UI and similar clients. KoboldCpp fully implements this API, which means you can use KoboldCpp as a drop-in replacement for a KoboldAI server.

- **KoboldAI Client:** The full KoboldAI Client (the heavier frontend) can connect to KoboldCpp. In KoboldAI’s “AI > Load Model > Online Services > KoboldAI API” menu, you can enter your KoboldCpp URL. For example, `http://localhost:5001` (or the LAN/Cloudflare URL if remote). KoboldCpp was designed to be compatible so the client likely won’t know the difference, except everything is running on KoboldCpp’s side.
- **Third-party UIs:** Projects like **SillyTavern** and **Agnaistic** mention KoboldAI API support. As the wiki notes, SillyTavern and Agnaistic specialize in character chat and can use KoboldCpp via this API ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20SillyTavern%3F%20What%20is,How%20do%20I%20use%20them)). To integrate:
  - In SillyTavern settings, select “KoboldAI” API, and input the endpoint (e.g., `http://192.168.1.10:5001`), choose streaming if desired (SSE if SillyTavern supports it) and provide the API key if you set one on KoboldCpp. After that, SillyTavern will send requests to KoboldCpp, allowing you to use its richer UI and features (like character avatars, memory, etc.).
  - Agnaistic (another chat UI) similarly can point to a KoboldAI endpoint.
- **Features via API:** The KoboldAI API has fields for all generation parameters (temperature, top_p, etc.), for memory, for author’s note, etc. So those UIs can control things just like Kobold Lite does. KoboldCpp also added some endpoints (like token count) that such UIs may leverage ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,length%20loaded%20from%20the%20launcher)).
- **Advantages:** Using these UIs can provide a different user experience – e.g., SillyTavern is focused on character-based RP and is highly customizable with themes, while Kobold Lite is a more general all-in-one. It’s great that you can choose your preferred interface while using the same KoboldCpp backend.

### 10.2 OpenAI-compatible API Integration

Starting with version 1.45.2, KoboldCpp can mimic an OpenAI API endpoint ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Is%20there%20an%20OpenAI%20API%3F)). This is extremely useful for integrating with applications or libraries that expect to talk to OpenAI’s servers (for GPT-3/4, etc.), but you want to route them to your local model instead.

- **How to Use:** After starting KoboldCpp, you have endpoints at:
  - `http://<host>:5001/v1/completions` – corresponds to the OpenAI *text completion* API.
  - `http://<host>:5001/v1/chat/completions` – corresponds to the OpenAI *ChatCompletion* (ChatGPT-style).
  - Optionally, `v1/embeddings` if an embeddings model is loaded via `--embeddingsmodel` (so KoboldCpp can return embedding vectors).
- When an application asks for an OpenAI API key, you can typically put any string (unless you set `--password`, in which case use that as the key). KoboldCpp doesn’t check the content of the key unless password is set (then it matches) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Can%20I%20add%20authentication%3F)).
- **Use Cases:** This allows you to use local LLMs with:
  - OpenAI Python libraries (by setting `openai.api_base = "http://localhost:5001/v1"` and `openai.api_key = "anything"` in code).
  - Apps like LangChain, which have OpenAI agents – point them to your KoboldCpp.
  - Chat frontend like ChatGPT UI replacements that allow a custom API (some UIs like “Chatbot UI” or mobile apps allow entering a custom endpoint).
  - Plugins or tools that integrate via OpenAI (for example, some VSCode extensions for code completion which allow custom endpoint).
- Remember that the OpenAI API is simpler; it might not expose fine-grained controls. Some third-party clients also might not send all parameters (like max_tokens). If a client doesn’t send `max_tokens`, KoboldCpp will use `--defaultgenamount` fallback ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=With%20Chat%20Completions%2C%20how%20do,many%20tokens%20the%20AI%20outputs)). You may want to adjust that.
- **Compatibility:** KoboldCpp returns responses in the format the client expects (with `choices` array, message content for chat, etc.). It should satisfy most clients. However, not every OpenAI feature is present (e.g., function calling, or some newer fields).
- If an application needs the OpenAI API but doesn’t allow custom endpoints, one trick is to use something like an **OpenAI reverse proxy** (the wiki references one ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=You%20can%20add%20a%20password,proxy))). But if possible, direct point is easier.

### 10.3 Ollama API Integration

**Ollama** is another local LLM runner with its own simple HTTP API. KoboldCpp has a basic compatibility with it ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Is%20there%20an%20Ollama%20API%3F)). This might not be widely used, but if a tool expects an Ollama server, you can potentially make it work:
- The Ollama API typically listens on `localhost:11434` and has endpoints like `/api/generate` etc. If KoboldCpp implements these, you would launch it in a mode to mimic that (or perhaps just by the nature of how it’s built on llama.cpp, it can accept similar calls).
- The wiki says it exists for “larger ecosystem compatibility” but is not recommended for normal use ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Is%20there%20an%20Ollama%20API%3F)) – which suggests use only if needed for a specific integration.

### 10.4 Other Integrations and Tools

- **LangChain / Python**: You can integrate KoboldCpp into Python pipelines. For example, using `requests` to call the KoboldAI API or OpenAI API endpoints from your Python code (for building chatbots, agents, etc.). Since it’s local, it’s fast for such iterative calls.
- **Plugins and Extensions**: Some browser extensions or chatbot frameworks allow connecting to a custom API. Look for ones supporting KoboldAI or OpenAI custom endpoints.

One specific integration:
- **Voxta (Voice Assistant)**: The search results showed a Voxta doc mentioning KoboldCpp ([KoboldAI - Voxta Documentation](https://doc.voxta.ai/docs/koboldai/#:~:text=KoboldCpp%20is%20an%20easy,self%20contained%20distributable%20from%20Concedo)). Likely, voice assistant frameworks can use KoboldCpp for their brain by connecting to its API. With speech and TTS integration, KoboldCpp is well-suited for powering a voice AI system.

### 10.5 Embeddings and Vector DB Integration

If you load an embeddings model with `--embeddingsmodel`, KoboldCpp opens an `/api/extra/embeddings` or `/v1/embeddings` endpoint ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20are%20Embeddings%20and%20how,can%20they%20be%20used)). This outputs vector embeddings given an input text. It’s intended for things like semantic search or feeding a vector database:
- For instance, if you want to use Milvus, Pinecone, etc., you could generate embeddings for your documents through KoboldCpp, then query them.
- It’s not a fully fledged vector DB itself (except the small TextDB feature).
- The advantage is you can use the same infrastructure for both generating answers and computing embeddings (with a smaller model like text-embedding-ada or similar in GGUF).
- That said, currently the UI doesn’t use embeddings directly – this is more for external integration via API.

In summary, KoboldCpp’s API flexibility means it can plug into many setups:
- It can act like **KoboldAI** for roleplay UIs.
- It can act like **OpenAI** for general AI apps and dev frameworks.
- It can work with **Ollama** or others as needed.
- It even covers **embedding generation** for advanced retrieval systems.

This makes KoboldCpp a kind of universal adapter in the local LLM ecosystem, which is exactly one of its goals: “unparalleled larger ecosystem compatibility” ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Is%20there%20an%20Ollama%20API%3F)).

## 11. Troubleshooting and Common Errors

When using KoboldCpp, you might encounter some common issues. Here’s a guide to troubleshoot them:

### 11.1 Application Startup and Model Loading Issues

- **Console/Window Closes Immediately:** If you double-click the .exe and it closes without showing the UI, it likely crashed on startup. To diagnose, run it via Command Prompt (open `cmd`, navigate to the folder, run `koboldcpp.exe`) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=The%20program%20just%20closes%20and,nothing%20is%20shown)). The error message will remain in the console. Common causes:
  - Missing model file or incorrect path (it might say it cannot find the model).
  - AVX2 not supported error (on an old CPU, see *No AVX2* below).
  - DLL missing (if a dependency failed to load – not common with the packaged exe, but if compiled yourself, ensure all needed DLLs are present ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=the%20,correctly%20on%20a%20different%20PC))).
- **Model Fails to Load (WinError or Crash):** If the program crashes during model load:
  - Check if your model file is correct and not corrupted. Sometimes downloads fail; try re-downloading from a reliable source ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Some%20architectures%20are%20not)).
  - If the error mentions “not enough space in scratch buffer” or similar allocation failure ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=I%20saw%20some%20error%20message,context%20memory%20%2F%20failed%20allocation)), your `--contextsize` might be too high for available RAM or `--blasbatchsize` too high. Try reducing context or batchsize ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=I%20saw%20some%20error%20message,context%20memory%20%2F%20failed%20allocation)).
  - If on Windows and the error is something like illegal instruction, and you have an older CPU (pre-2013 or so), you likely need `--noavx2` mode ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20older%20devices%20do%20not,all%20CPU%20intrinsics%20and%20GPU)).
- **Old CPU / No AVX2:** On some older PCs (e.g., older AMD or very old Intel), the default binary might use instructions not supported. KoboldCpp provides `--noavx2` to run in a compatibility mode using only AVX1 or even no SIMD ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20older%20devices%20do%20not,all%20CPU%20intrinsics%20and%20GPU)). If you suspect this:
  - Launch from command line with `--noavx2` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20older%20devices%20do%20not,all%20CPU%20intrinsics%20and%20GPU)). If that still fails, try `--failsafe` in addition ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=mode,CPU%20intrinsics%20and%20GPU%20usage)). Failsafe will be extremely slow but confirms if instruction set was the issue.
  - On GUI, select the preset “Old CPU, no AVX2” before launching ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20older%20devices%20do%20not,all%20CPU%20intrinsics%20and%20GPU)).
  - Note that `--noavx2` and `--failsafe` disable GPU offloading as well, so you’ll be CPU-only ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=resort%2C%20you%20can%20try%20enabling,CPU%20intrinsics%20and%20GPU%20usage)).
- **Windows 7:** If you try running on Win7, you might get missing functions (K32GetProcessMemoryInfo, etc.). The solution is similar: use `--noavx2` or `--failsafe` because those use older instruction paths ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Koboldcpp%20is%20not%20working%20on,windows%207)). But performance will be poor. Official stance: upgrade to Win10+ ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Windows%207%20is%20not%20a,your%20OS%20to%20Windows%2010)) if possible.
- **Model Detected Wrong / Not Working:** If you load a GGML model and get an error like “wrong model type or version”:
  - Possibly the model was converted with a newer format that’s unsupported. However, KoboldCpp supports a wide range of file versions and can often auto-detect. Check the console log: it might say it assumed a certain format and then failed.
  - You can force a particular format version with `--forceversion N` ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=This%20can%20happen%20if%20the,reference%20of%20currently%20supported%20fileversions)), where N corresponds to the known version codes (like 6 for GGUF v1/v2) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,GPTJ_3%3D102%20uses%20new%20ggml%20lib)). The wiki lists many codes for internal use, but ideally stick to known stable models.
  - If the model is truly incompatible (some exotic architecture), KoboldCpp might not support it. Check if it’s in the supported list (see if it’s a variant of something known).
  - Another cause is mislabeled file extension – e.g., a `.gguf` named `.bin` or vice versa. Renaming to correct extension might help detection.
- **Horde Worker SSL Errors:** If running as a Horde worker and you see SSL certificate errors connecting to the Horde, try launching with `--nocertify` to ignore cert verification ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=I%27m%20encountering%20SSL%20errors%20with,my%20horde%20worker)).

### 11.2 Generation Issues

- **AI Rambling Past Stop:** You stop speaking but the AI keeps generating when it should have stopped (for example, continues writing both user and AI parts). This often is due to the *End-of-Stream (EOS) token* being accidentally prevented. Many models have a special token to denote the end of response. If that token is banned, the model will never terminate naturally ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20models%20will%20use%20a,be%20used%20in%20Story%20mode)). KoboldCpp had an `--unbantokens` option historically to allow endless generation, but it’s deprecated now ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20models%20will%20use%20a,be%20used%20in%20Story%20mode)).
  - Solution: In the UI settings, find **EOS Token Ban**. If it’s set to “Ban” always, change it to “Auto” or “Unban” such that in the current mode, the EOS token is *not* banned ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20models%20will%20use%20a,be%20used%20in%20Story%20mode)). Generally, for story you ban (to let story flow continuously), but for chat/instruct you unban (so it stops at end of answer) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20models%20will%20use%20a,be%20used%20in%20Story%20mode)).
  - If using API, ensure `use_default_badwords_ids` is set to false if you want EOS allowed ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Some%20models%20will%20use%20a,should%20not%20be%20used%20in)).
  - Or simply, if the AI doesn’t stop, hit the “Stop” button and consider adjusting that setting next time.
- **Nonsense or Garbage Output:** If the AI output is gibberish or a mess:
  - First, rule out a bad model/quantization. Some incorrectly converted models will produce junk. Try another model to compare ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Some%20architectures%20are%20not)).
  - Check if **RoPE settings** are correct. If you manually set a ropeconfig that isn’t suitable, the position encodings might be off leading to incoherent text ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,P%3D0.92)). For extended context fine-tunes, they usually specify what rope scaling to use. If in doubt, remove any custom `--ropeconfig` and let KoboldCpp default (NTK based on context).
  - Some architectures might not be fully supported – e.g., if you attempt a ChatGLM or something exotic in GGML, it might not produce correct output. Ensure the model type is known to be working with llama.cpp derivatives.
  - Sampler settings can also cause weird output if set to extreme values. If you get nonsense, check that your temperature, top_p, etc., are in reasonable ranges. The wiki suggests good defaults like temp ~0.7, top_p 0.9, rep pen 1.1 ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=Samplers%20are%20basically%20how%20the,default)) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,6%2C0%2C1%2C3%2C4%2C2%2C5)). If someone set top_p=1 and top_k=0 and temperature high, output can degrade.
  - If it’s a bug (something clearly wrong beyond these, like consistently output is broken), consider reporting it on Github with details.
- **Premature or No Output:** If you submit a prompt and get no response or it immediately finishes:
  - Possibly the prompt included an EOS token inadvertently (so the model thought it should end). Check if using instruct mode on a model not tuned for it, sometimes it might output EOS token after the prompt.
  - If it happens with certain content, maybe some tokenization quirk or the model not knowing how to respond. Trying a different phrasing can help determine if model or system issue.
  - Ensure you have `max_length` or Generation Length high enough. If max tokens is 0 or very low, it might stop. Check the UI slider “Amount to Generate” ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Thinking%20Tags)) or in API ensure a reasonable max_tokens is set.
- **Repetition / Forgetfulness:** These are model limitations mostly, but:
  - Use **Repetition Penalty** settings (e.g., set to 1.1 or 1.2) to reduce verbatim repeats.
  - Utilize the Memory field to remind the AI of key points if it forgets.
  - If it repeats the user’s last message often, that might be a format issue (model isn’t properly separating user vs AI). Using Chat mode with correct prompt templates usually solves that.

### 11.3 Performance Issues

- **Low Speed or Utilization:** If generation is much slower than expected:
  - Check if GPU is actually being used (if you intended it). The console will indicate if layers offloaded. Also, tools like nvidia-smi can show GPU usage. If not used, maybe a flag was wrong or missing (e.g., forgot `--usecublas`).
  - If GPU is used but still slow, maybe you didn’t offload enough layers (`--gpulayers`). Offloading too few means CPU still does a lot of work.
  - High `--contextsize` will slow down generation because the model always reserves that much memory and time to compute attention even if not filled; don’t set contextsize arbitrarily high unless needed.
  - Too many threads on CPU can ironically slow down if it causes excessive context switching – try lowering `--threads` a bit if you set it to a high number and see if speed improves.
  - On Nvidia, ensure you are using a release build (PyInstaller exe is fine). If you compiled with MSVC, note that a debug build would be slow.
- **Memory Leaks or Usage Creep:** If you notice memory usage growing or not being freed between uses:
  - There were historically issues with certain features causing memory fragmentation. Typically the process memory will remain high after loading model (which is expected). But if it grows over time massively, it could be a bug.
  - Using `--mmap` helps keep only needed parts in RAM, potentially smoothing memory usage.
  - Also, if generating extremely large outputs in one go, memory can spike for temporary buffers.
- **GPU Out of Memory Mid-run:** If you see a CUDA out-of-memory error after some generation, maybe the model tried to allocate for something like KV cache on GPU:
  - Using `--lowvram` can help by not putting KV cache on GPU ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=lowvram%20can%20be%20added%20to,offloaded%20at%20all%20if%20enabled)). The update note says for newest models it’s not triggered, but then re-enabled in Jan 2024 ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=lowvram%20can%20be%20added%20to,offloaded%20at%20all%20if%20enabled)).
  - Or reduce `--gpulayers` so it uses a bit less VRAM.
  - Also, `--nommq` might slightly increase VRAM usage but if MMQ was on, turning it off can increase usage actually. Actually, MMQ off uses more VRAM, so ensure you *don’t* disable MMQ if VRAM is tight ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,disable%20it%20instead%20if%20unwanted))# KoboldCpp Comprehensive User Manual

## 1. Overview of KoboldCpp

KoboldCpp is an easy-to-use local AI text generation software for running large language models (LLMs) in the GGML/GGUF format. Inspired by the original KoboldAI, it provides a single self-contained application that bundles a text-generation backend (built on the llama.cpp library) with a web-based **Kobold Lite** UI and multiple integrated features. KoboldCpp allows you to load **LLM models** (such as LLaMA, LLaMA-2, Mistral, Pygmalion, etc.) in a lightweight format and interact with them through a browser UI or via API endpoints. In addition to text generation, KoboldCpp extends functionality with:

- A **web UI (Kobold Lite)** that supports persistent stories, editing tools, memory, world info, character profiles, scenario presets, and multiple **interaction modes** (Story, Chat, Instruct, Adventure).
- Built-in **KoboldAI API** compatibility (so other applications can use KoboldCpp as if it were a KoboldAI server), as well as **OpenAI-compatible** and **Ollama-compatible** API endpoints for broad integration support.
- Support for **Stable Diffusion** image generation (via stable-diffusion.cpp integration) and **speech processing** (speech-to-text via Whisper, text-to-speech via Oobabooga’s OuteTTS).
- Full **offline operation for privacy** – all processing is local by default (no internet required) – and **open-source** code (AGPLv3) available for review or self-compilation.
- **Cross-platform support**: KoboldCpp runs on Windows, Linux, macOS (including Apple Silicon), Android (via Termux), and WSL, with both CPU-only and GPU-accelerated execution options.

In summary, KoboldCpp provides an all-in-one solution to run LLMs on your own hardware with an intuitive interface and rich feature set, combining the efficiency of llama.cpp’s inference with KoboldAI’s user-friendly storytelling/chat capabilities. The following sections detail system requirements, model formats, installation steps, configuration, usage, advanced features, networking, API integration, troubleshooting, and security considerations to help you get the most out of KoboldCpp.

## 2. System Requirements

**Operating System:** KoboldCpp is cross-platform and runs on Windows (10 or later recommended), Linux (64-bit), macOS (Intel or Apple Silicon), and Android (via a Linux environment app like Termux). It can also be compiled and run under Windows Subsystem for Linux (WSL). *Note:* Windows 7 is not officially supported; it lacks modern CPU instruction support required for performance, though a fallback mode may allow it to run slowly.

**CPU:** A 64-bit CPU is required. For optimal performance, a processor with AVX2 support is recommended (most Intel/AMD chips from ~2015 onward). KoboldCpp uses AVX2 instructions for fast matrix math. On older CPUs without AVX2, it provides a “No AVX2” fallback mode (`--noavx2` flag or a special GUI preset) that uses AVX or basic instructions. There’s also a `--failsafe` mode which disables all SIMD optimizations as a last resort. These modes will significantly reduce speed. In general, more CPU cores are beneficial since you can configure multiple threads for generation (often set threads ≈ number of physical cores). 

**Memory (RAM):** Memory requirements depend on the size of the model and quantization level (see *Model Formats and Quantization*). As a rule of thumb, for a standard 2048-token context and a 4-bit quantized model (Q4_0), you will need approximately:

- **4 GB RAM** for a **3B** parameter model.
- **8 GB RAM** for a **7B** model.
- **16 GB RAM** for a **13B** model.
- **32 GB RAM** for a **30B** model.
- **64 GB RAM** for a **65B** model.

Using higher precision models (e.g. 8-bit or 16-bit) or larger context windows (4096, 8192 tokens, etc.) will increase RAM needs, whereas using smaller quantization (3-bit, 4-bit) reduces RAM at the cost of some model quality. **Disk Space** required depends on model file size; quantized models range from a few GB (for 7B) up to 30+ GB (for 65B in F16). Ensure you have enough storage for at least one model file and any additional components (like image or audio model files if you use those features).

**GPU (Optional):** KoboldCpp can run purely on CPU, but if you have a compatible GPU, it can leverage it for acceleration. Supported are:
- **NVIDIA GPUs** – via CUDA/cuBLAS (compute capability 3.5+ and CUDA 11+ drivers).
- **AMD GPUs** – via OpenCL (CLBlast) or via Vulkan.
- **Intel GPUs** – via OpenCL (CLBlast) or possibly Vulkan.
- **Apple Silicon GPUs** – via Metal acceleration.
However, a GPU is *not* required – even without one, KoboldCpp will use CPU threads for all computations. If you plan to use GPU offloading, see the *Performance Tuning* section for any necessary libraries (e.g., NVIDIA CUDA Toolkit on Linux for cuBLAS, OpenCL drivers for CLBlast). On Windows, the packaged build includes the needed CUDA and CLBlast DLLs, so generally it works out-of-the-box with your GPU drivers installed.

**Additional Requirements:** If using the **Stable Diffusion** image generation feature, you will need a compatible Stable Diffusion model file (e.g., an SD1.5 or SDXL `.safetensors`) and sufficient VRAM/RAM for image generation. For **Whisper** speech-to-text or **TTS** (text-to-speech) audio generation, you will need to download the respective model files (Whisper GGML for transcription, OuteTTS GGUF and a WavTokenizer for TTS). We will cover obtaining these in later sections. Finally, a modern web browser (Chrome, Firefox, Safari, Edge) is needed to use the Kobold Lite Web UI; the UI is served locally via a web server.

## 3. Model Formats and Obtaining Models

**Supported Model Formats:** KoboldCpp supports the **GGUF** model format natively, which is the latest generation of GGML-type quantized LLM formats. GGUF (.gguf) is a container that includes model weights and metadata; it’s now the recommended format and actively maintained. KoboldCpp also retains backward compatibility with older **GGML** `.bin` model files (from llama.cpp’s earlier versions). This means you can still load older models if needed, though some new features might not work with them. **Other formats** (like PyTorch `.pt` or `.safetensors` for Transformers) are not directly supported – those must be converted to GGUF/GGML first. In short, **if it’s GGUF, it will work**, and if it’s legacy GGML, it will likely work too (KoboldCpp can autodetect various GGML versions).

**Model Architectures:** Virtually any model architecture available in GGUF format should run in KoboldCpp. This includes:
- Meta AI’s **LLaMA** and **LLaMA-2** models (and their fine-tunes like Alpaca, Vicuna, WizardLM, etc.).
- Newer models like **Mistral** (7B), **Qwen** (ChatGLM derivatives), **MPT**, **Falcon**, **StarCoder** (for code), etc.
- Chat-oriented models e.g. **Pygmalion**, **Metharme** for roleplay, **Wizard Vicuna** for instruction tuning.
- Older ones like **GPT-J**, **GPT-NeoX** (including Dolly, RedPajama), **Pythia**, **Cerebras**, **RWKV** (if converted to GGUF).
- Even experimental ones like **Llama-3** (if any), or **Yi / Gemma** (Chinese LLMs).
The KoboldCpp wiki lists many supported families and states: *“if it’s GGUF, it should work.”* 

**Where to Get Models:** The best source for GGUF (and older GGML) models is **Hugging Face** Hub. Many community contributors (like TheBloke, KoboldAI, etc.) host quantized models there for easy download. You can browse models on HuggingFace and look for files with `.gguf` or `.bin`. For example, KoboldCpp’s guide suggests some starter models:
- **Airoboros Mistral 7B** – a smaller instruct model (GGUF).
- **Tiefighter 13B** – a larger general model.
- **Beepo 22B** – even larger, high-quality model.
- Other recommended ones: **L3-8B Stheno v3.2**, **Fimbulvetr 11B v2** for text generation.
For AI image generation (Stable Diffusion models), **CivitAI** or HuggingFace have `.safetensors` you can use. For speech models, HuggingFace has Whisper GGML files and TTS models ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Speech%3A%20TTS%20models%20for%20Narration)).

To download a model from HuggingFace:
- You can use the website’s “Download” buttons. Often, you’ll see multiple quantized files listed (e.g., model.Q4_0.gguf, model.Q8_0.gguf, etc.). Choose **one** file corresponding to the precision you want (see next subsection) – you do *not* need all of them ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=need%20them%20all%3F%20Which%20Quantization%3F,F16%3F%20Q4_0%3F%20Q5_1)).
- Or use `git lfs` in a git clone, or direct links via `wget/curl`. For example:  
  `wget https://huggingface.co/TheBloke/phi-2-GGUF/resolve/main/phi-2.Q2_K.gguf`  
  would download a very small Phi-2 model (2-bit quant).
- Ensure the file extension remains `.gguf` or `.bin`. KoboldCpp looks at this to help determine format.

**Quantization Formats:** Each model is typically available in various **quantization levels**. Quantization reduces model size (and speed/memory) by using lower precision for weights. Common quantization formats (from lowest quality/smallest size to highest quality/largest size) include:

- **Q2_K** – 2-bit experimental quantization (very small, significant quality loss).
- **Q3_K (S/M/L)** – 3-bit “K”-quant (improved 3-bit method; S=small, M=medium, L=large variants). Very low size, noticeable quality loss, but sometimes used for tiny devices.
- **Q4_0** – Original 4-bit quantization (simplest method). Small (~25% of full model size), some quality drop.
- **Q4_1** – Improved 4-bit (uses a finer scaling per block). Slightly larger than Q4_0, better quality.
- **Q4_K (S/M/L)** – 4-bit K-quantization variants. These have even better fidelity at 4-bit. For example, Q4_K_M (medium) often retains more accuracy than Q4_1.
- **Q5_0** – 5-bit quantization. Better quality than any 4-bit, moderate size (~35-40% of full).
- **Q5_1** – Improved 5-bit. Often a sweet spot of quality vs size; many find Q5_1 almost as good as 8-bit.
- **Q5_K (S/M)** – 5-bit K-quants (small/medium). Further refined 5-bit.
- **Q6_K** – 6-bit “K-bit” quant. Closer to 8-bit in quality, about ~50% of full size.
- **Q8_0** – 8-bit quantization. Very high fidelity (almost no loss vs half-precision), but file sizes are ~ half of F16.
- **F16** – 16-bit float (not really quantized; essentially the model’s original weights in half precision). Best quality, but largest size.

Generally, higher bit = better output quality but more memory usage. The naming scheme: Q<bit>_<variant>. If it has _K or a second number, it’s a newer quant method which usually means improved quality for the same bits (developed by Georg Wiese). For example, a **Q4_K_M** will outperform a Q4_0 at the same size.

Below is a **comparison table** illustrating quantization trade-offs (taking LLaMA2 7B as an example):

| **Format** | **Approx Size (7B)** | **Speed** | **Quality vs F16** | **Notes** |
|------------|----------------------|-----------|--------------------|-----------|
| Q3_K (M)   | ~2.7 GB              | Fast      | Noticeably lower   | Very tiny model footprint, use only if memory is extremely limited. |
| Q4_0       | ~3.9 GB              | Very fast | Moderate drop      | Good for experimentation and low-end hardware. |
| Q4_1       | ~4.2 GB              | Very fast | Slight drop        | Better coherence than Q4_0. |
| Q4_K_M     | ~4.3 GB              | Fast      | Minimal drop       | One of the best 4-bit methods (close to 5-bit quality). |
| Q5_1       | ~4.8 GB              | Fast      | Very minor drop    | Often indistinguishable quality in many tasks, great balance. |
| Q6_K       | ~5.5 GB              | Moderate  | Nearly full        | 6-bit K-quant, very high fidelity. |
| Q8_0       | ~7.2 GB              | Slower    | Virtually full     | 8-bit; near-original accuracy, but larger and a bit slower. |
| F16        | ~13 GB               | Slowest   | Full (baseline)    | Highest quality, requires most RAM; often unnecessary if a good quant is available. |

*Note:* Speeds assume CPU; on GPU, smaller quantization also often yields speedups due to less data transfer. “Quality” is qualitative; exact differences depend on the model and task. K-quants (and other newer quant methods) aim to minimize perplexity loss. In practice, many users find **Q5_1** or **Q4_K_M** hits a sweet spot for chat models, whereas Q8_0 or F16 is rarely needed unless maximum fidelity is required.

**Choosing a Quantization:** You **do not need multiple quantized files** of the same model ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=need%20them%20all%3F%20Which%20Quantization%3F,F16%3F%20Q4_0%3F%20Q5_1)). Pick one that fits your hardware constraints. For a given model:
- If you have plenty of RAM (or a strong GPU), you might go for Q5_1 or Q6_K for quality.
- If you have limited RAM (e.g., 8GB total), Q4_0 or Q4_1 for a 7B/13B model might be necessary.
- Very low bits (Q3, Q2) are generally not recommended unless absolutely needed, due to significant output degradation.
- It’s easy to swap quant files if you want to compare quality later; they’re interchangeable as long as they’re the same model in different precisions.

**Converting Models:** If you have a model in a format like PyTorch or `.safetensors` (for LLMs), you’ll need to convert to GGUF. Tools exist (the llama.cpp repository provides conversion scripts for LLaMA-based models). The conversion typically requires the original model and a quantization script. Many popular models are already converted by community, so check HuggingFace first to save time. For **Stable Diffusion** models (used for image gen), conversion isn’t needed – KoboldCpp uses them directly in their safetensor format with its integrated stable-diffusion.cpp.

**Image and Audio Models:** For **image generation**, KoboldCpp supports Stable Diffusion 1.x, 2.x, SDXL, and similar diffusion models. You can download these from HuggingFace (e.g., the Stable Diffusion 1.5 model, SDXL Base, etc.) or community fine-tunes from CivitAI. They typically come as `.safetensors` or `.ckpt`. KoboldCpp expects a safetensors path via `--sdmodel`. For **Whisper** speech recognition, models are on HuggingFace in GGML format (tiny.en.bin, base.bin, etc.). For **OuteTTS** (TTS), the voice model and the WavTokenizer are available as GGUF on KoboldAI’s profile (linked in wiki). 

Ensure when downloading models to verify checksums if provided (to avoid corrupted downloads). Once you have the model files, you’re ready to run them with KoboldCpp.

## 4. Installation 

KoboldCpp is distributed as a stand-alone package. Depending on your platform, you can use prebuilt binaries (easiest) or compile from source. Below are cross-platform installation steps:

### 4.1 Windows Installation

**Prebuilt Executable (Recommended):** Download the latest **KoboldCpp.exe** from the [official GitHub releases page】 (the file is usually just `koboldcpp.exe`). This is a single-file program – no installation required. Place the `.exe` in a folder of your choice (you might create a directory like `C:\KoboldCpp` and put it there, along with your model files). Double-click `KoboldCpp.exe` to launch it. 

- On first run, Windows SmartScreen might warn about an unrecognized app. You can click “More info” and “Run anyway” as needed. 
- You should see a console window open and the KoboldCpp Launcher GUI appear. From there, you can select a model file to load, adjust settings (like threads or GPU layers), and then click the button to launch the main server.

If you prefer to run from command prompt or want to see debug output, you can also launch KoboldCpp via terminal:
- Open Command Prompt, `cd` to the directory with `koboldcpp.exe`. 
- Run `koboldcpp.exe --help` to see available command-line options.
- Or directly run a command with flags, e.g.:  
  `koboldcpp.exe "E:\LLM Models\wizardLM-13B.Q5_1.gguf" --usecublas --gpulayers 15 --threads 8`  
  (This would load that model with CUDA acceleration, offloading 15 layers to GPU, using 8 CPU threads.)

**GPU Support on Windows:** The Windows binary is packaged with support for both NVIDIA (cuBLAS) and OpenCL (CLBlast) backends built-in. If you have a CUDA-capable NVIDIA GPU, you can use `--usecublas` without needing to install anything else (just ensure you have updated NVIDIA drivers and the NVIDIA CUDA runtime, which is usually included in the driver). For AMD or Intel GPUs, you can try `--useclblast`. The Windows build includes the necessary DLLs for CLBlast and Vulkan as well, but you might need to install the Vulkan SDK if you want to compile with Vulkan yourself. Most users can simply launch with the appropriate flag and it will work (if it doesn’t, see Troubleshooting).

**Compiling from Source (Windows):** If you want the absolute latest version or custom build:
- Install **Visual Studio 2022** (with C++ Desktop development) or Visual Studio Build Tools, and **CMake**.
- Install the **CUDA Toolkit** if building with CUDA (for cuBLAS).
- Clone the repo: `git clone https://github.com/LostRuins/koboldcpp.git`.
- Open the project’s CMakeLists in Visual Studio or use CMake to configure.
- Build the solution. Copy the compiled `koboldcpp_cublas.dll` next to `koboldcpp.py` if needed (the build outputs might put it in a Release folder) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,you%20may%20need%20to%20include)).
- This is only recommended for advanced users. The provided .exe already bundles everything for you.

**WSL Note:** You generally don’t need WSL on Windows since the native .exe works well, but if you prefer a Linux environment, see Linux instructions and consider just using those in WSL. The KoboldCpp devs jokingly suggest there’s no strong reason to use WSL for it.

### 4.2 Linux Installation

On Linux, you have several options: use a precompiled binary, use an automated installer script, or compile from source.

**Precompiled Binary:** On the GitHub releases page, look for a file like `koboldcpp-linux-x64` or similar ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Easy)) (there may be variants like `koboldcpp-linux-x64-cuda1150`, which indicates it’s built with CUDA 11.5 support). Download that file. In a terminal, `chmod +x koboldcpp-linux-x64` to make it executable, then run it: `./koboldcpp-linux-x64`. This should launch the KoboldCpp program (you might run it from a terminal to see logs, or double-click in a desktop environment which should also open a terminal window with the UI). If it complains about missing libraries, you may need to ensure you have basic system libraries installed (most should be static, but some older glibc systems might not be compatible).

If your distro is fairly up-to-date, the binary should work. For older glibc versions, you might have to use the build script.

**One-Line Auto Installer:** The developers provided a convenient curl command to fetch the latest binary. From your terminal: 

```bash
curl -fLo koboldcpp \
  https://github.com/LostRuins/koboldcpp/releases/latest/download/koboldcpp-linux-x64 && chmod +x koboldcpp
``` 

This downloads the latest Linux x64 build to a file named `koboldcpp` in your current directory ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=the%20binary%20,and%20run%20the%20generated)). Then run it with `./koboldcpp`. This method is quick and ensures you have the latest version without manual browsing.

**Automated Build Script:** If the precompiled binary doesn’t run (due to GLIBC mismatch or you’re on a different architecture, etc.), there is an included script `koboldcpp.sh` that can build it for you. Steps:
- Ensure you have **conda** or **mamba** installed (the script seems to rely on conda to manage dependencies).
- Run `./koboldcpp.sh dist`. This will fetch dependencies via conda (like Python, compilers, etc.) and compile KoboldCpp from source, then package it into a standalone binary in a `dist` directory ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=linux,MacOS%20%28Precompiled%20Binary)).
- This process might take some time (especially building llama.cpp, etc.). After completion, you’ll have a new binary that should work on your system.

**Compile from Source (Manual):** You can also compile manually without the script:
- Install development tools: on Debian/Ubuntu `sudo apt install build-essential python3-dev git`, etc. Also get `cmake` and any libraries you need (OpenCL headers if using CLBlast, etc.).
- Clone the repo and run `make` inside the `koboldcpp` directory. By default, this compiles a CPU-only version.
- To enable GPU acceleration: see the make flags. For example:
  - `make LLAMA_CUBLAS=1` to include CUDA support (you need the CUDA toolkit installed).
  - `make LLAMA_CLBLAST=1` to include OpenCL/CLBlast (install `libclblast-dev` on Debian, or `clblast` on Arch first).
  - `make LLAMA_VULKAN=1` to include Vulkan support (install Vulkan SDK or dev libraries).
  - You can combine flags for a multi-backend build: e.g. `make LLAMA_CUBLAS=1 LLAMA_CLBLAST=1 LLAMA_VULKAN=1` to include all.
- Once compiled, you can run `python3 koboldcpp.py --model <yourmodel>` from the repo directory to launch (the `koboldcpp.py` is the main launcher script). You can also freeze it with PyInstaller if you want a single binary (the script for that is in `koboldcpp.sh`).

**Dependencies:** On Linux, if compiling with GPU support:
- For **cuBLAS**: install CUDA Toolkit (make sure to match the version expected, e.g., if you want to use `LLAMA_CUBLAS=1`, having CUDA 11.8 or 12 installed).
- For **CLBlast**: ensure OpenCL ICD loader and CLBlast library are installed (`sudo apt install ocl-icd-opencl-dev libclblast-dev` on Ubuntu, for instance).
- For **Vulkan**: install Vulkan SDK or at least `vulkan-headers` and a loader.
If not using those, a plain `make` should just build a CPU version which has no external deps beyond a C++ compiler.

### 4.3 macOS Installation

**Prebuilt Binary (macOS):** KoboldCpp offers precompiled binaries for modern macOS, particularly for Apple Silicon (M1/M2/M3) Macs ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=)). Download the latest **macOS ARM64** binary from the releases (e.g., a file named `koboldcpp-mac-arm64`). After downloading:
- Open Terminal, `chmod +x koboldcpp-mac-arm64` to make it executable ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Self%20Compile)).
- Run it: `./koboldcpp-mac-arm64`. 
- On newer macOS, you might get a security prompt saying it’s from an unidentified developer. If it refuses to run, go to *System Settings > Privacy & Security*, look for the blocked app notice and click “Open Anyway” ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=,Self%20Compile)). Then run the binary again.
- It should open a Terminal window with the KoboldCpp UI. If it exits or errors, ensure you’re on a relatively recent macOS version (Monterey or later ideally).

This binary includes Metal support for Apple GPUs and uses Apple’s Accelerate framework for BLAS on CPU by default, which is optimal for macOS.

If you’re on an **Intel Mac**, there might not be an official binary. You would need to compile from source:
- Install Xcode (or Command Line Tools) and Homebrew (for any libraries).
- Follow similar steps as Linux: `git clone`, then likely use `make LLAMA_ACCELERATE=1 LLAMA_CUBLAS=1` if you have an NVIDIA eGPU (rare on Mac), or just `make` for CPU+Accelerate. For Apple Silicon, `LLAMA_METAL=1` is automatically used if building on that platform.
- Running from source: `python3 koboldcpp.py --model model.gguf`.

**macOS Notes:** Apple Silicon users get great performance using Metal (for GPU) and Accelerate (for CPU). If for some reason BLAS on Mac is slow (sometimes Accelerate might not be tuned for very low precision ops), you can try launching with `--noblas` to use the built-in implementation as a test, but generally it’s not needed.

### 4.4 Android (Termux) Installation

Running KoboldCpp *directly* on Android is possible using Termux (a terminal emulator app providing a Linux-like environment). Keep expectations modest: performance on a phone will be limited and large models may not run due to memory constraints. Still, for experimentation or running small models, here’s how:

1. **Install Termux:** Download Termux from F-Droid (the Play Store version is outdated). You can get it via the F-Droid app or its APK. Once installed, open Termux.

2. **Update and Install Packages:** In Termux, run:  
   `pkg update && pkg upgrade`  
   Then install necessary packages:  
   `pkg install wget git python clang make cmake openssl`  
   (openssl might be needed for some builds). This prepares a minimal dev environment.

3. **Get KoboldCpp Source:** Run:  
   `git clone https://github.com/LostRuins/koboldcpp.git`  
   Then `cd koboldcpp`.

4. **Build KoboldCpp:** Run: `make`. This will compile the program for ARM (it will detect the architecture). It may take some time on a phone. If you run into memory issues while compiling, you might try enabling swap in Termux or cross-compile on a PC (advanced).

5. **Download a Model:** Choose a very small model to test (to avoid huge downloads on phone and heavy load). For instance:  
   `wget https://huggingface.co/TheBloke/phi-2-GGUF/resolve/main/phi-2.Q2_K.gguf`  
   (This is a tiny 45MB 2-bit model).

6. **Run KoboldCpp:** Once compiled, start the server with:  
   `python koboldcpp.py --model phi-2.Q2_K.gguf`  
   (adjust model path/name accordingly). If you want to try without the UI (to save memory), you could add `--cli` for command-line mode, but let’s assume using the UI.

7. **Connect to UI:** Termux by default allows connecting to `localhost` if you’re on the device. To view the UI on the phone itself, you might need a Termux add-on like Termux:GUI or just use a browser in Termux (not ideal). A simpler method: once KoboldCpp is running in Termux, it’s on `localhost:5001`. Now open your Android web browser and go to `http://127.0.0.1:5001`. If that doesn’t work (Android might not allow loopback to Termux by default), you can do a workaround:
   - Run `ifconfig` or similar in Termux to find the IP (often `10.0.2.15` if using Termux from inside Android).
   - Or use `--host 0.0.0.0` when launching KoboldCpp in Termux, then find your phone’s IP on WiFi and navigate to `http://phone_ip:5001` from the phone’s browser.
   If all else fails, consider using the Cloudflare tunnel method (see Remote section) from Termux.

**Performance on Android:** It will be *slow*. A 7B Q4 model might only generate <1 token/sec on a high-end phone CPU. If you compiled with OpenBLAS or similar it could help a bit. There is currently no mobile GPU support (no OpenCL for Adreno in this setup easily). For any prolonged use, running on a PC and accessing from the phone (Section 9) is recommended.

### 4.5 Installation Summary and Platform Tips

To summarize cross-platform:
- **Windows:** Use the .exe for easiest setup. Ensure Visual C++ runtime (if needed) is installed (usually not needed for PyInstaller exe). GPU usage should work out-of-box for NVIDIA.
- **Linux:** Prebuilt binary or one-line install is quick. If compiling, install required dev libs first.
- **Mac:** Use provided ARM64 binary for Apple Silicon, or compile for Intel. Grant permission in security settings if blocked.
- **Android:** Use Termux and compile inside it (mostly for enthusiasts or light models).
- **WSL:** It’s possible to follow Linux steps in WSL, but it’s generally simpler to use Windows .exe directly to utilize GPU properly.

Now that you have KoboldCpp installed, let’s move on to launching it and configuring various options.

## 5. Launching KoboldCpp: Options and Configuration Flags

Once installed, you can launch KoboldCpp using its GUI or via command-line with various flags. When KoboldCpp starts, it loads the model into memory and opens a local web server (default port 5001) serving the Kobold Lite UI and APIs.

**Basic Launch (GUI):** If you start KoboldCpp without any arguments (e.g., by double-clicking on Windows or running the binary without flags), it will show the Launcher GUI. In this launcher, you can:
- Select the **Model file** (via a file picker dialog).
- Adjust **Presets** or settings like number of threads, GPU offload layers, etc., typically available as dropdowns or text fields.
- Then click **Launch** to load the model and start the UI server.
While using the GUI is straightforward, some advanced options might not be exposed and would need command-line flags.

**Command-Line Launch:** You can bypass the GUI or provide extra parameters using the terminal. The general invocation is:

```
koboldcpp [model_file] [port_number] [--flag1 value1 --flag2 value2 ...]
```

- `model_file` is the path to your GGUF/GGML model. If omitted, the GUI will prompt for it.
- `port_number` is optional; default is 5001. (Alternatively use `--port` flag.)
- The `--flag` options let you configure behavior (GPU usage, context length, etc.).

For example, launching a model with GPU acceleration and some specific settings:
```
./koboldcpp-linux-x64 ~/models/gguf/WizardLM-7B.Q5_1.gguf --useclblast --gpulayers 20 --threads 6 --contextsize 4096
```
This would load the 7B model, try OpenCL (CLBlast) for GPU acceleration, offload 20 layers to the GPU, use 6 CPU threads, and set context length to 4096 tokens.

**Discovering Flags:** To see all available options, run the program with `--help`. For instance:
```
koboldcpp.exe --help
``` 
This will print usage information and a list of flags with brief explanations. KoboldCpp has **many** options, which might feel overwhelming. They can control everything from model performance, to UI behavior, to experimental features. We will highlight the most important ones by category:

### 5.1 Model and Context Options

- **`--model <path>`** – specify the model file to load. Not needed if you provide the model as first argument or use the GUI.
- **`--port <number>`** – set the port for the web UI/API (default 5001). Use this if 5001 is busy or if you want multiple instances on different ports.
- **`--host <IP>`** – specify the network interface to bind (default is usually `127.0.0.1`). For example, `--host 0.0.0.0` will allow other devices on your network to connect (see Section 9).
- **`--contextsize <N>`** – set the maximum context length (in tokens) for the model. Default is 2048 for most models. You can increase this (e.g., 4096, 8192) if you have a model that supports or if using RoPE scaling to extend context. Note: increasing context uses more RAM – roughly linear with the increase. KoboldCpp supports up to 16k for GGML models and 32k for GGUF models.
- **`--smartcontext`** – enables **Smart Context** mode. This reserves ~50% of context as a buffer to reduce reprocessing (discussed in Advanced Features). It effectively halves available context for new content but can improve speed in long sessions. Not needed if using default context shifting on GGUF.
- **`--noshift`** – disable **Context Shift**. By default, KoboldCpp uses context shift for GGUF models (a superior mechanism to handle full context). If you want to turn it off for some reason, use this flag.
- **`--nofastforward`** – disable **FastForwarding**. FastForwarding skips re-evaluating unchanged prefix tokens in consecutive prompts. It’s on by default; disabling it will make each turn reprocess from scratch, which is usually slower.
- **`--prompt "<text>"`** – run a one-shot generation with the given prompt text and exit. KoboldCpp will output the model’s completion to the console and quit. This is useful for quick CLI usage or scripting. Combine with `--promptlimit <N>` to restrict output length.
- **`--promptlimit <N>`** – when using `--prompt`, limit the number of tokens in the generated response. E.g., `--promptlimit 100` ensures at most 100 tokens are generated for the `--prompt` text.
- **`--cli`** – launch in **interactive command-line mode** (no web UI). This allows you to chat with the model directly in the terminal. It’s useful on systems without a browser or if you prefer a console interface. (In CLI mode, just type your prompt and press enter; the model’s reply will print).
- **`--noavx2`** – run in No AVX2 mode (for old CPUs). Forces use of slower instructions for compatibility. Use only if KoboldCpp fails to run on your machine due to illegal instruction errors.
- **`--failsafe`** – ultimate fallback mode for compatibility. Disables all CPU SIMD optimizations and GPU usage. Only use if normal mode doesn’t work at all (performance will be very slow).

### 5.2 Performance and Hardware Flags

- **`--threads <N>`** – set the number of CPU threads for inference. By default, KoboldCpp will choose a reasonable value (often #cores or cores minus one). You can experiment for optimal performance. Typically: use number of **physical cores**. For example, on a 6-core/12-thread CPU, `--threads 6` is often best. If you fully offload to GPU, you can drop this to 1 or 2 to handle any residual tasks.
- **`--blasthreads <M>`** – optionally set a different thread count for BLAS (matrix multiplication during prompt processing). If not set, it defaults to the same as `--threads`. In most cases, you can leave this alone. If you notice high CPU usage during prompt loading and want to restrict it, you could set `--blasthreads` lower.
- **`--usecublas`** – use **NVIDIA CUDA/cuBLAS** for GPU acceleration. Requires an NVIDIA GPU. On Windows, works out-of-box. On Linux, ensure you compiled with `LLAMA_CUBLAS=1` or use the CUDA build. This offloads the heavy linear algebra to the GPU.
- **`--useclblast [platform_id] [device_id]`** – use **OpenCL (CLBlast)** for GPU acceleration. Works for NVIDIA, AMD, or Intel GPUs through OpenCL. The optional numeric IDs allow selecting a specific platform/GPU if you have multiple. If you leave them blank, KoboldCpp will list available devices and try the first one. Example: `--useclblast 0 1` might select the second device on the first platform.
- **`--usevulkan`** – use **Vulkan** for GPU acceleration. This is a newer backend that can work on various GPUs (NVIDIA/AMD, possibly Intel). It often gives good performance on AMD GPUs if CLBlast isn’t optimal.
- **`--usemetal`** – (macOS only) use **Metal** for Apple GPU acceleration. This is usually automatic on Mac if available, but you can explicitly set it if needed.
- **`--usecpu`** – force CPU-only mode. This disables any GPU backend auto-selection and ensures all computation is on CPU. By default, if no `--use...` flag is given, KoboldCpp will actually attempt to use the best available backend (it might auto-enable CUDA if on Windows with an NVIDIA card, for example). If you want to make sure to use CPU (perhaps for debugging or comparing performance), use `--usecpu`.
- **`--gpulayers <N>`** – offload *N* transformer layers to GPU (for inference). This works in conjunction with one of the GPU flags above. Offloading layers means those layers’ weights and computations stay on GPU, which can dramatically speed up per-token generation. The optimal number depends on your GPU’s VRAM and the model size. For example, a 6GB GPU can offload around 18 layers of a 13B model at 2048 context. You can specify `--gpulayers -1` to let KoboldCpp auto-choose the maximum it thinks fits, but auto-guess isn’t always perfect, so manual tuning is recommended (start low and increase until you approach VRAM limits).
- **`--tensor_split <r1> <r2> ...`** – for multi-GPU setups (NVIDIA/cuBLAS only). If you have multiple GPUs, by default KoboldCpp will distribute layers across them automatically. The `--tensor_split` flag lets you manually specify the proportion of the model on each GPU. For instance, `--tensor_split 3 1` would split 75%/25% between two GPUs. Provide a ratio for each GPU (the numbers will be normalized). If not set, it tries to split based on VRAM or evenly.
- **`--lowvram`** – modify GPU behavior to use less VRAM at the cost of speed. For cuBLAS, this prevents offloading of certain buffers like the key-value cache to the GPU, which reduces memory usage. Use this if you get out-of-memory errors on GPU. (In newest GGUF models, lowvram is automatically not used unless specified, but as of Jan 2024 it’s functional again when explicitly set).
- **`--nommq`** – disable Quantized MatMul (**MMQ**) optimization. By default, when using cuBLAS, KoboldCpp enables an optimization that uses quantized matrix multiplication for prompt processing (which saves memory and can be faster for certain quant types). If for some reason you suspect this causes issues, you can disable it. Normally, leave it on (or rather, don’t opt it out) because in most cases it improves performance.
- **`--blasbatchsize <N>`** – adjust the batch size for BLAS prompt processing. The default is 512 tokens for LLaMA models. If you have a low-memory device, you could lower this (128, 256) to reduce peak memory during prompt loading at the expense of speed. Typically, only tweak if you see memory errors during the initial prompt.
- **`--flashattention`** – enable FlashAttention (when using CUDA). FlashAttention is a faster GPU attention mechanism that can improve speed and reduce memory usage for large context. Requires a compatible GPU (most modern ones) and you need to have compiled with support. If you have a high-end NVIDIA GPU and are using long contexts, this is beneficial.
- **`--quantkv <level>`** – enable **quantized KV cache**. Level 1 = 8-bit cache, 2 = 4-bit cache (level 0 is default 16-bit). Quantizing the key/value cache saves memory during generation (especially with long context) at a minor quality cost. Note: fully quantizing both key and value requires `--flashattention` on; otherwise only the keys will be quantized. Use if you need to conserve memory for very long chats.
- **`--usemmap`** – use memory-mapped file I/O for the model. This can reduce initial RAM usage because parts of the model are loaded from disk on demand rather than fully read into memory. It’s useful if you have limited RAM or if you want to quickly load a large model without using all memory upfront. Downside: slight latency the first time a new part of the model is accessed (after that it stays in OS cache usually).
- **`--usemlock`** – lock the model in RAM (prevent it from being swapped out). This can improve stability/performance on Linux especially, by ensuring the model data doesn’t get paged to disk. It’s off by default. If you have plenty of RAM, it can be turned on for peace of mind.

### 5.3 UI and Server Behavior Flags

- **`--multiuser [N]`** – control **Multi-User mode**. By default, multiuser support is enabled (equivalent to `--multiuser -1` which means unlimited or some internal default). Multiuser mode allows multiple browsers/clients to connect to the same KoboldCpp instance simultaneously, queuing their requests. If you want to restrict it, you can specify a number N (max concurrent users). `--multiuser 0` disables multi-user, forcing one user at a time (additional connections will be rejected). Usually leave it on; even if you’re the only user, it doesn’t harm.
- **`--password <pass>`** – set a **requirement for an API key** (auth). When this is set, any client (web UI or API call) will need to provide a key matching `<pass>`. In the web UI, it will prompt for an API key on connect. In API usage, you send an `Authorization: Bearer <pass>` header. This is useful if you open up KoboldCpp to a network and want to restrict access. It’s basically a simple shared password for all requests.
- **`--admin`** – enable **Admin Mode**. This allows runtime model/configuration switching via the UI. When admin mode is on, the UI will have an admin panel (usually at a special URL or a toggle) where you can load different `.kcpps` config files or models on the fly. Must be combined with `--admindir <directory>` that contains the config files you want to allow, and optionally `--adminpassword <pass>` to secure the admin panel. Useful for power users hosting a service, but not needed for normal single-model use.
- **`--config <file.kcpps>`** – load settings from a **.kcpps configuration** file. These config files can be saved from the KoboldCpp GUI (they store all launch parameters). Using this flag, you can quickly apply a saved preset. Similarly, `--exportconfig <filename>` will output your current settings to a .kcpps file and exit, and `--exporttemplate <filename>` will save a shareable template (excluding device-specific details).
- **`--showgui`** – show the Launcher GUI even when command-line flags are provided. Normally, if you pass flags, KoboldCpp might just apply them and not show the launcher window. With `--showgui`, the GUI will appear with those flags pre-filled into the fields. This lets you adjust them if needed before finalizing. It’s a way to use a config file or command preset but still verify/edit in the UI.
- **`--quiet`** – **Quiet mode**, which hides detailed logs of prompts and generations in the console. By default, the console will show the text of each prompt and the model’s output as it generates (useful for debugging or if you’re piping output). `--quiet` suppresses this, which can be good for privacy (no visible text in terminal) or just a cleaner log.
- **`--foreground`** – (Windows only) force the console window to the foreground on each new generation. This was added because on Windows, if the console is in the background, the process can get deprioritized (idling). By bringing it front for active generation, it avoids that slowdown. If you run KoboldCpp and notice it getting sluggish when you aren’t focusing the window, try this flag.
- **`--nosplash`** – (if present) skip any splash screen or ASCII art on startup. Not sure if KoboldCpp has one, but some versions print a fancy title.
- **`--unpack`** – if using the PyInstaller one-file binary, this will unpack its contents to a directory. Useful if you want to modify internal files (like the UI HTML/JS or config). After unpacking, you can run `koboldcpp.py` from the unpacked directory for development.
- **`--nomodel`** – launch the UI without loading a model. This brings up Kobold Lite interface where you can connect to an external backend (like if you have KoboldCpp running elsewhere or want to connect to AI Horde, etc.). Usually not needed unless you specifically want an interface without local model.

### 5.4 Generation and API Flags

While most generation parameters (temperature, top-p, etc.) are set in the UI or via API payload, there are some flags affecting generation defaults or API behavior:

- **`--defaultgenamount <N>`** – set a default max tokens to generate if the client doesn’t specify one. Some third-party clients might not send a “max_tokens” or similar. This flag ensures the model doesn’t run on infinitely. For example, `--defaultgenamount 200` would cap generations at 200 tokens unless overridden by the request.
- **`--chatcompletionsadapter <file.json>`** – specify a JSON adapter for formatting OpenAI ChatCompletion API prompts ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=OpenAI%20compatible%20Chat%20Completions%20API,format%2C%20all%20fields%20are%20optional)). This is used to fine-tune how system/user/assistant messages are combined into a single prompt for the model. Unless you are tailoring for a specific model’s conversation format, you usually don’t need this. KoboldCpp has a default adapter logic internally.
- **`--horde*` flags** – There are several flags for running an **AI Horde** worker (community cluster):
  - `--hordekey <key>` – your API key for AI Horde.
  - `--hordeworkername <name>` – name your worker node.
  - `--hordemodelname <name>` – display name of your model on the Horde network.
  - Plus `--hordemaxctx`, `--hordegenlen` to limit what your worker can accept.
  These are advanced; see Section 9. They basically turn KoboldCpp into a Horde worker that others can use via KoboldAI Horde.
- **`--websearch`** – enable the **WebSearch** proxy feature. This allows the model to perform web searches when queries come to a special endpoint. Use if you want the model to have browsing capability (it uses DuckDuckGo under the hood).
- **`--savedatafile <file>`** – specify a database file for server-side story saves. By default, saves are client-side. With this, multiple clients can share and persist stories on the server.
- **`--onready <command>`** – run a shell command when the server is ready (model loaded). Could be used to trigger something else (not sure of exact syntax, but likely for automation).
- **`--logdir <directory>`** – log conversations to a directory (if implemented). Possibly used to keep logs of all interactions.

This isn’t exhaustive, but it covers the most commonly useful flags. For most users, the key ones are selecting GPU backend, threads, and context. Others are for niche scenarios. The GUI launcher covers many via presets: for example, there might be a dropdown for “GPU backend: CPU/CUDA/OpenCL/Vulkan” which internally uses these flags, or a slider for context length, etc.

Feel free to experiment with different combinations to see what yields the best performance or behavior for your use case. Always remember you can refer back to `--help` and the KoboldCpp wiki for clarifications.

## 6. Performance Tuning and Hardware Acceleration

Optimizing KoboldCpp’s performance involves choosing the right model quantization, leveraging your hardware effectively, and tuning parameters like threads and offloading. This section gives tips to maximize speed and throughput on various hardware setups.

### 6.1 CPU-Only Performance

If you don’t have a suitable GPU (or choose not to use one), KoboldCpp will run on the CPU using multiple threads. Here’s how to get the most out of CPU inference:
- **Threads:** Set `--threads` equal to the number of physical cores (or at most, logical cores). If hyperthreading is present, often using all logical cores is fine but diminishing returns beyond physical cores. You can experiment: on a 8-core/16-thread CPU, try 8, 12, 14 threads to see which is fastest. Sometimes reserving a couple threads helps if you want to keep system responsive.
- **High-Performance Mode:** Ensure your CPU is running in a high performance mode (disable any Eco mode or power saver, plug in if laptop, etc.). LLM inference is CPU intensive and benefits from consistent turbo frequencies.
- **Pinning Threads (Advanced):** If you really want to optimize, you can try pinning threads to specific cores using OS tools (`taskset` on Linux, or the Process Lasso utility on Windows). However, this is micro-optimization; often not necessary.
- **BLAS vs Built-in:** On CPU, KoboldCpp uses optimized BLAS routines for prompt processing (like Xbyak or Accelerate). If you notice prompt loading is slow or stalling, you could try `--noblas` to use the fallback method, but generally the optimized path is faster. On Apple Silicon, Accelerate is usually great. On older systems, OpenBLAS (in older Kobold) is deprecated in favor of the built-in methods.
- **Quantization for Speed:** Lower-bit quantizations (Q4, Q5) not only reduce memory but also reduce arithmetic complexity, often increasing speed. For example, a Q4 model can run as much as 1.5-2x faster than an 8-bit model on CPU. So if speed is more important than a bit of quality, choose a smaller quant. 
- **Memory Bandwidth:** LLM inference is memory-bound. Faster RAM (and ensuring dual-channel memory on desktops/laptops) can improve performance. There’s not a flag for this, but be aware if your memory is unusually slow (e.g., running on a VM).
- **Use KV Cache Efficiently:** The model keeps a cache of past tokens. On CPU, this is stored in RAM. There’s an experimental quantized KV which can reduce its size, but if you have enough RAM, default is fine. If you push context length high (e.g., 8k), CPU will have to do larger attention computation, which slows generation. Consider if you really need >2048 context on CPU as it may drastically reduce tokens/s throughput.

In summary, for CPU-only:
- Use an appropriate quant (Q4 or Q5 ideally).
- Use all (or almost all) cores with threads.
- Ensure CPU isn’t thermally throttling (cooling is adequate).
- Accept that higher model sizes scale almost linearly in slowdown; a 13B model might be ~2x slower than a 7B, 30B slower again, etc., on the same CPU. So pick model size according to your acceptable speed.

### 6.2 GPU Acceleration (Single GPU)

Using a GPU can significantly speed up both the initial prompt processing and the per-token generation:
- **Choosing Backend:** If you have an NVIDIA GPU with decent VRAM (4GB+), `--usecublas` (CUDA) is typically the fastest option. AMD GPUs can use `--usevulkan` or `--useclblast`; Vulkan tends to have good performance on AMD in recent versions. Intel GPUs (like Iris Xe or Arc) might work with CLBlast or Vulkan as well.
- **GPU Offloading (`--gpulayers`):** This is crucial for speeding up generation. Offloading even a portion of layers means those layers don’t run on CPU. For example, offloading 20 layers of a 28-layer model will leave only 8 layers to run on CPU, vastly improving throughput. The ideal is to offload as many as possible until the GPU memory is about full. KoboldCpp’s documentation gives some reference points, but a rule of thumb: start with a moderate number and increase. If you overshoot and the model doesn’t fit in VRAM, it may fallback to CPU or crash. For instance, on a GPU with 6 GB VRAM: try `--gpulayers 10` for a 13B model, then 14, then 18, etc., until you either succeed or find the limit.
- **VRAM Usage and Quantization:** The size of model layers that fit in VRAM depends on quantization. A Q4_0 13B model might be ~6.5GB total, so offloading all layers might not fit on a 6GB card (plus overhead). But offloading half might. If you use a more compressed model (Q5_1 or Q4_K), they are slightly larger than Q4_0. So sometimes a slightly more compressed model can mean the difference between fully fitting or not. 
- **Prompt Processing on GPU:** Simply using `--usecublas` or others without `--gpulayers` will offload the **prompt/tokenization** stage to GPU. This means when you input a long prompt or chat history, it can compute that initial pass faster (especially noticeable with long prompts). But it will still do the main generation on CPU per token. So for maximum acceleration, combine `--useX` with `--gpulayers`.
- **Memory Offload Strategies:** If VRAM is not enough for all you want:
  - Use `--lowvram` with cuBLAS to sacrifice some speed and keep usage low.
  - Try quantizing the model more (e.g., use Q4 instead of Q5).
  - If you still run out, consider using half CPU half GPU (that’s what offloading partially is).
- **FlashAttention:** If you have a newer NVIDIA GPU (Ampere or later), enabling `--flashattention` can boost performance for large context sizes by optimizing attention calculation. It reduces memory usage too, possibly enabling slightly more context or faster generation.
- **Overhead Consideration:** For smaller models (7B) and smaller context, sometimes pure CPU can be “fast enough” that the overhead of copying data to GPU isn’t worth it. However, as of late 2023 improvements, even 7B usually sees a speed gain from GPU. But if you have something like a very weak GPU and a strong CPU, test both paths.

**Performance Gains:** Offloading typically has the most impact on **per-token generation speed**. For example, a 13B model might do 2 tokens/sec on a high-end CPU. With a mid-range GPU offloading, it could do 5-10 tokens/sec or more. Prompt processing speed (the time to initially feed the prompt) also improves (on GPU it might be a few seconds vs tens of seconds on CPU for a long prompt).

**Example Setup:**
- GPU: NVIDIA RTX 3060 (12GB). You can likely offload entire 13B Q5_1 model (`--gpulayers 40` or all layers, if supported) and get ~10-12 tok/s in 2048 ctx. A 7B could fully reside easily and get even faster.
- GPU: NVIDIA GTX 1660 (6GB). Offload ~20-22 layers of 13B Q4, or ~all of 7B Q4. Should see ~5 tok/s on 13B, maybe ~10 tok/s on 7B.
- GPU: AMD RX 6800 (16GB). With Vulkan, you might offload a lot. Performance might be slightly lower than NVIDIA but still significant speedup vs CPU.
- iGPU: Intel Iris XE (integrated). You can try CLBlast; offloading might help a bit, but integrated GPUs share system memory, so gains are smaller.

### 6.3 Multi-GPU and High-End Setups

If you have multiple GPUs (NVIDIA only, for now):
- KoboldCpp will by default use all detected NVIDIA GPUs if you just do `--usecublas` without specifying device. It splits the model layers across them. You’ll see each GPU’s memory being used.
- You can adjust how it splits with `--tensor_split`. E.g., if you have an 8GB and a 4GB card, you might do `--tensor_split 2 1` (which would allocate twice as much to the 8GB card as the 4GB card). You may need to experiment.
- **Row vs Layer Split:** KoboldCpp splits by layers by default, meaning whole contiguous layers go to one GPU or the other. There is an alternate row-wise split (splitting each layer’s matrices across GPUs) which can sometimes improve utilization but is not exposed as a simple flag (it was more experimental). For most, layer split is fine and simpler.
- Multi-GPU helps mainly to fit larger models. For example, a 70B model might need two 24GB GPUs to fully offload. Or two 12GB GPUs could handle a 30B fully. The speed doesn’t double, but it can increase if both GPUs are kept busy. 
- If one GPU is much slower, it could bottleneck the entire process, so ideally use identical or similar GPUs. Otherwise, consider only using the faster one (with `--usecublas 0` for example to pick the first GPU if the second is weaker).

### 6.4 Additional Performance Considerations

- **Speculative Decoding:** (Advanced, for tinkerers) KoboldCpp supports using a smaller “draft” model to accelerate a larger model’s generation via speculative decoding. This is quite experimental. If you use `--draftmodel smallmodel.gguf`, the system will use that model to propose tokens ahead and only confirm them with the big model. If the draft is significantly faster, this can improve overall speed. You can also offload the draft model with `--draftgpulayers`. This is mainly beneficial if you have CPU to spare or another GPU to dedicate to draft, and a very large main model. It’s not commonly used by most, but it’s there.
- **Mirostat Sampling:** Not performance per se, but a new sampling method. KoboldCpp had a `--usemirostat` (now deprecated) – you can still enable Mirostat in generation settings if desired (through UI or API). Mirostat can dynamically adjust randomness for more stable output. If you have it on, it might slightly affect generation speed due to extra calculations, but usually negligible.
- **Draft Mode / Intermediate KV Offload:** If running near memory limits, keep an eye on system swap. Using `--usemmap` can help by not loading the entire model to RAM, but once generation starts it will bring needed parts in anyway.
- **Batching multiple requests:** KoboldCpp doesn’t natively do batch inference (like serving two prompts at once) beyond the multiuser queue (which processes sequentially). If you want throughput for many short requests, you might spin up multiple instances (if you have the cores) each with the model loaded. This is heavy on memory but can improve throughput in an API scenario.

**Performance by Hardware Type – Quick Tips:**

- *Low-end CPU (e.g. 4-core, no GPU):* Use 7B or 3B models in Q4 or Q3. Use all threads. Expect a few seconds per sentence. Keep context short (≤1024) to stay responsive.
- *Mid-range CPU (8-core) with no GPU:* 13B Q4 is feasible, or 7B Q5 for better output. Use 6-8 threads. Possibly try 16-bit 7B if you need quality and can handle ~1 tok/sec.
- *CPU with modest GPU (like laptop GTX 1650 4GB or integrated):* Offload what fits (maybe half a 7B). Use GPU for some speed, but also keep threads moderate. A 7B Q5 fully on GPU 4GB might not fit, so do hybrid.
- *Gaming GPU (e.g. RTX 3060/3070):* Run 13B or even 30B models. Offload as much as possible. 13B Q5_1 likely fully fits in 12GB. 30B Q4 might partially offload (12GB can handle maybe 20/32 layers). CPU threads 4-6 to assist.
- *High-end GPU (RTX 3090, 4090, etc.):* You can run 30B or 65B models. A 3090 (24GB) can almost fit a 65B Q4 model fully. Offload all layers if you can. Use `--flashattention` for big contexts. Multi-GPU (2×24GB) can handle 70B+ fully.
- *Apple M1/M2:* Use `--usemetal`. Offload ~all layers it can (the unified memory is large but bandwidth is shared; still, M1 16GB can do a 13B in 4-bit decently). The `--threads` can be set to high performance cores count (e.g., 4 on M1). Apple chips have excellent int8 performance, so maybe try 8-bit models if memory permits for quality.
- *AMD GPU:* Use Vulkan backend. Offload similarly to NVIDIA guidelines by VRAM. AMD tends to not have as refined quant kernels as NVIDIA’s cuBLAS, but it’s improving.

Remember to monitor your system (task manager or `nvidia-smi`/`intel_gpu_top` etc.) to see if the GPU is fully utilized and CPU not overloaded. Aim for a balance where GPU is busy during generation and CPU has just enough threads to feed it.

### 6.5 Example: Improving Throughput

Suppose you find generation slow. Here’s a checklist to methodically improve it:
1. **Check quantization** – Are you using an unnecessarily high precision model? If you loaded F16 or Q8 but have a slow machine, switch to Q4 or Q5.
2. **Enable GPU** – If you have a GPU that’s sitting idle, use it! Add `--usecublas` or `--useclblast` accordingly.
3. **Offload more** – If you used `--gpulayers 10` and see your GPU has free VRAM, increase it to, say, 20 and try again.
4. **Threads** – If CPU usage is low, maybe increase `--threads`. If CPU usage is maxed but GPU isn’t fully loaded (in partial offload scenario), you might reduce threads a bit to let GPU take more load.
5. **Reduce context or split conversation** – If you’re at a 8k context and it’s dragging, consider summarizing old content or splitting chat, or just accept slower speed for long context.
6. **Use a smaller model** – It’s possible a 13B model’s output quality is marginally better but it’s half the speed of a 7B. If you prioritize speed, a well-tuned 7B (like Mistral 7B, which is very good for its size) might suffice and be much faster.
7. **Update KoboldCpp** – Performance improvements frequently come in updates. Ensure you’re using a recent version, as optimizations like MMQ, flashattention, etc., might not be present in older builds.

By applying these, you should reach an optimal state for your specific hardware. 

## 7. Web UI and Usage Modes

One of KoboldCpp’s strengths is its built-in **Kobold Lite** Web UI, which provides a user-friendly interface to interact with your model in various modes (chat, story writing, etc.) without needing to write any code. This section covers how to use the Web UI and explains the different modes of operation.

### 7.1 Accessing the Web UI (Kobold Lite)

After launching KoboldCpp and loading a model (either via GUI or CLI), the program will initialize a local web server. By default, it listens on **http://localhost:5001**. In the console log, you should see something like “*KoboldCpp server running on port 5001*”. To access the UI:
- Open a web browser on the same machine and navigate to **http://localhost:5001**.
- You should see the Kobold Lite interface load. It’s a single-page application that should load quickly.
- If you changed the port with `--port`, use that port instead (e.g., http://localhost:6001).
- If you bound to 0.0.0.0 or an IP, and you want to connect from a different device on your LAN, replace “localhost” with the host’s IP (e.g., `http://192.168.1.100:5001`). Ensure firewall allows it. (See Section 9 on remote access.)

The web UI does not require internet and is served directly by KoboldCpp – all the HTML/JS is embedded. So even if you’re offline, it will work. You may get a prompt about “not secure” (since it’s HTTP on localhost); that’s normal and not an issue when connecting locally.

**Tip:** If the UI doesn’t load or you get connection refused:
- Double-check KoboldCpp is still running (console open, no errors).
- Ensure you’re using the correct port.
- If on Windows, sometimes firewall might block it – check Windows Defender Firewall settings; you might have to allow `koboldcpp.exe` on private networks.
- If you used `--host 127.0.0.1` explicitly, it won’t accept other IPs. Using `--host 0.0.0.0` will accept both localhost and external.

Once loaded, you’ll see the main UI which is by default in **Classic** theme and typically opens in Story mode (or last used mode if it remembers).

### 7.2 UI Layout and Features

The Kobold Lite UI has several components:
- **Text Output Area:** The large central area where the story or chat conversation appears. Here, both your inputs and the AI’s outputs will be displayed in sequence.
- **Input Box:** At the bottom (in most layouts), where you type your next message or story continuation. You can press **Enter** or click a send button to submit (the exact look depends on UI style).
- **Send / Submit Button:** Usually an arrow or “Send” next to the input box.
- **Top Menu/Buttons:** There may be a top bar with buttons like *Save*, *Load*, *New*, *Settings*, etc.
- **Settings Panels:** On one side (often the right side, can be toggled via a button or always visible if window is wide) you have collapsible sections for settings:
  - **Basic** settings: Mode (Format), Max Tokens, Temperature, Top-p, Top-k, etc.
  - **Advanced** settings: fine-grained sampler settings, sequence handling (like stopping sequences).
  - **Memory / Author’s Note / World Info:** In Story/Adventure modes, panels to set persistent memory, author’s note, and world info triggers.
  - **Style & Appearance:** Possibly options to switch UI theme (Classic/Messenger/Aesthetic/Corpo), font size, color scheme, etc.
  - **Characters / Scenarios:** If you have any character JSONs or scenario files loaded, you can choose them here to preload a persona or story setup.
  - **Image Gen / Audio:** If image generation is enabled (by loading an SD model), an *Image* tab or section will appear, where you can input prompts and generate images. Similarly, for audio (Whisper/TTS), controls may appear to record audio or play voice output.

One notable part of Kobold Lite (inherited from KoboldAI’s design) is the **format modes** and related features:
- There’s a **Format dropdown** in Basic settings listing Story, Chat, Instruct, Adventure. This sets how the UI treats your inputs/outputs (detailed in next section).
- **Memory** field: This is a hidden part of the prompt that persists across the entire session. Whatever you write in Memory will be injected into the context for every generation (typically at the top). Use it to remind the AI of key context.
- **Author’s Note:** Another hidden directive, usually inserted at a strategic point (in story mode, maybe after a separator) to influence style or tone of the story. It’s often short term and more stylistic.
- **World Info (Lorebook):** You can create entries like “Dragon: A large fire-breathing reptile...” such that whenever “Dragon” appears in the story, the AI gets the lore about it added to context. There’s a UI to add/edit these entries.
- **Save/Load:** You can save the entire state of a story or chat (including settings optionally) to a file (JSON format). Later you can load it back and continue.
- **Reset/New:** Start a new story or chat (clears context, memory remains if not cleared).

Kobold Lite’s UI is **responsive**, meaning if you narrow the window (say on mobile), it might rearrange elements. On a phone, it might hide the side panel behind a menu button to give more room for chat.

### 7.3 Interaction Modes: Story, Chat, Instruct, Adventure

Kobold Lite supports four primary **modes** of text interaction which determine how your input is formatted to the model and how the model’s output is treated:

- **Story Mode:** For creative writing (novels, fanfiction, etc.), where you and the AI cooperatively write a story. In Story mode:
  - The user’s entries are assumed to be part of the narrative, and the AI will continue the story from there.
  - There are no distinct “turns” like a conversation; it’s a continuous text. Usually, there’s no “you: AI:” separation.
  - Useful for writing in third person or continuing a narrative. The AI will continue in the style of the story.
  - By default, the EOS (end-of-story) token is banned in Story mode so the AI doesn’t stop early.
  - E.g. You write: “John walked into the eerie house, heart pounding.” and the AI might continue: “Moonlight filtered through dusty windows as the floorboards creaked under his feet...”

- **Chat Mode:** For back-and-forth conversation or roleplay, where each user and AI turn is distinct. In Chat mode:
  - The UI may prefix messages with “You:” and the AI with a name (e.g., a character’s name or “AI:”).
  - The conversation is turn-based: you say something, then AI replies, and so on.
  - It simulates a persona or just an assistant answering. Good for Q&A, casual chat, roleplay dialogue, etc.
  - The model prompt is typically formatted with roles if the model was trained on chat (some system prompt possibly plus the alternating dialogue). KoboldCpp handles the formatting behind scenes according to the model’s needs (especially if using a chat template or OpenAI API compatibility).
  - Example: You: “Hello, how are you?” AI: “I’m just a program, but I’m functioning well. How can I help you today?”

- **Instruct Mode:** This is like using ChatGPT in the “system prompt + instruction” style. It treats the user input as an instruction or question, and the AI gives a single, helpful answer (no persona, no multi-turn memory unless you add it manually).
  - Internally, it might format your input as something like “### Instruction:\n<Your prompt>\n### Response:\n” if needed for the model. Many instruction-tuned models expect a format like that.
  - The UI might not label turns as “You/AI” at all, it might just show the prompt and completion.
  - Ideal for tasks: e.g., “Explain the theory of relativity.” The AI will produce a detailed explanation.
  - The AI will usually stop after fulfilling the instruction (especially if EOS token is allowed).
  - In instruct mode, **EOS token unban** is often recommended (so the model can decide to end when done). Kobold Lite likely handles that automatically (rep pen adjustments etc., might also differ).
  - If you ask another question after, note that many instruct models are stateless – they aren’t expecting follow-ups in the same session unless you keep context. If you want multi-turn with memory, Chat mode might be better.

- **Adventure Mode:** Also known as “Text Adventure” or “AIDungeon” style. This mode is similar to story mode but with a second-person perspective where *you*, the player, issue commands or actions, and the AI narrates the results.
  - Commonly, user inputs start with a special token like `> ` to denote a player action, and the AI responds with narrative.
  - Kobold Lite likely automates the `>` prefix and handles formatting.
  - It’s basically Story mode with an implied interactive gameplay loop. Often accompanied by scenario presets (e.g., you choose a fantasy scenario: “You are in a dungeon, a dragon is guarding treasure... What do you do?”).
  - The UI in Adventure mode might also allow *pre-written actions choices* or style akin to AIDungeon.
  - Example: *User:* “> Open the ancient chest.” *AI:* “You kneel before the ancient chest and slowly lift its creaking lid. Inside, you find...” etc.
  - Memory and world info are heavily used in adventure mode to keep track of game state and lore.

The **mode selection** influences not only formatting but also which UI features are visible:
- In Chat/Instruct, you won’t see the Author’s Note because that’s more for storytelling.
- In Story/Adventure, you’ll see those creative aids (Memory, etc.) and not things like “Your name / AI name” (which are in Chat settings).
- The UI will also adjust the prompt templates accordingly.

Under the hood, KoboldCpp might use different token biases or stop sequences:
- Story mode often disables any stop at “\nYou:” (if the model was fine-tuned on multi-turn, you don’t want it to stop at user cues in story).
- Chat mode might use stop sequences to prevent the AI from generating the user’s part (like stop when it produces something that looks like a user prompt).
- Instruct mode might stop on EOS or when a new prompt token would start.

### 7.4 UI Styles (Themes)

Kobold Lite offers multiple **UI themes/layouts** to suit different preferences:
- **Classic:** The default notebook-like interface with plain text. It’s clean and universal for all modes.
- **Messenger:** A chat bubble layout for Chat mode, making it look like a messaging app conversation (user on one side, AI on the other in bubbles). It might even show timestamps and names like typical IM.
- **Aesthetic:** A highly customizable style for Chat/Instruct that allows pretty formatting, including colored text, backgrounds, maybe even profile pictures for user/AI. This is great for role-play with character portraits or just to make it visually engaging. You can often change text colors for AI vs user, font, add CSS, etc.
- **Corpo:** Mimics corporate or professional chat UIs (like ChatGPT’s interface or a Slack chat). Usually simple, with maybe a light gray background chat bubble for AI, etc., aiming for that enterprise feel.

You can switch these in the UI settings (often under a “UI Style” dropdown). Not all styles are available for every mode:
- Messenger is typically for Chat mode only (since story text in bubbles doesn’t make sense).
- Aesthetic might only apply to Chat and Instruct.
- Classic works everywhere.
- Corpo might only apply to Chat (it says specifically for chat/instruct).

Selecting a style should dynamically change the view without reloading the page. It doesn’t affect the model or generation, purely the client-side presentation.

### 7.5 Using the Web UI: Tips & Tricks

**Editing and Regenerating:** In story mode, if the AI’s output isn’t what you wanted, you can often **edit the last AI message** or your prompt and then click a *Regenerate* or *Retry* button. Kobold Lite allows you to tweak the text (fix a name, remove a line) then have the AI continue from the edited point. This is very helpful in story writing to keep continuity.
- There’s usually a *clipboard* or *edit* icon by each message that allows editing it. After editing, the subsequent content is cleared and you can regenerate.

**Multiple Generations & Compare:** Sometimes, you might be able to get the AI to produce multiple endings or choices (some UIs allow branching). Kobold Lite has “multi-gen” support through an extension – not sure if integrated by default. But if you press regen, it might override the previous output unless you copy it elsewhere.

**Memory Management:** Use the **Memory** field wisely. Keep it concise (a few important sentences) because it eats into context every time. It should contain immutable facts or the theme. Example: “John is a 30-year-old detective afraid of heights. The year is 1925 in London.” This way, the AI doesn’t forget these points.

**World Info (Lorebook):** If your story has lots of proper nouns or lore, input them in World Info so the AI has the details when needed, without permanently taking space until those words appear. For instance, add entry: “Kobold: Small creature, mischievous, loves shiny objects.” Then whenever “Kobold” is mentioned, the model will get that detail in context automatically. This helps consistency.

**Author’s Note:** A powerful tool. This is a short directive influencing style. For example: “*Author’s note: The tone of the story is suspenseful and eerie. The following scene should highlight John’s fear of heights.*” This will bias the AI to incorporate that tone and element. You can change the author’s note on the fly depending on the scene, and it only affects subsequent generation until you change/remove it. It’s inserted typically at the end of the context in a special format (maybe wrapped in some tokens to hide it from output). Keep it brief to not waste too many tokens.

**Preventing AI from doing X:** If you find the AI output includes something undesirable (maybe formatting issues or it’s repeating user prompts), look at the **Advanced** settings:
- **Stop Sequences:** You can set certain sequences that when generated will cause the generation to stop. For instance, in chat, a stop sequence might be `"\nYou:"` so that the AI doesn’t start speaking for the user. If the AI is producing “You: ...” then the stop sequence might not be set properly; check the settings or mode template.
- **Banned Tokens/Phrases (Anti-slop):** KoboldCpp has a feature for phrase banning. The UI might expose it as a list of substrings to ban. For example, banning “[” might prevent it from outputting those if your model tends to produce unwanted bracketed text. Use with caution; banning too much can hamper model freedom.
- **Rep Penalty & Order:** If the AI is looping or repeating itself, increase repetition penalty or adjust the repetition penalty range. Check the **sampler settings**:
  - Temperature, Top-p, etc., can be tuned. Lower temperature (towards 0) makes outputs deterministic and repetitive if too low. Higher (~1.0) makes it more creative but sometimes incoherent. Defaults around 0.7 are good.
  - Repetition Penalty around 1.1 or 1.2 is typical to avoid verbatim repeats. The UI lists recommended defaults.
  - Sampler order (the order in which nucleus, top-k, etc., apply) can be left default unless you know what you want. KoboldCpp often defaults to an order that is good. A warning might appear if you choose a suboptimal combination.
- **Mirostat:** If you enable Mirostat in advanced settings, note it replaces top-k/p with its own mechanism. If you don’t understand it, best to stick to familiar samplers.

**Web UI on Mobile:** You can actually open the Kobold Lite UI on a phone or tablet browser (if on same network or using tunnel). The interface should adapt (e.g., toggling side panels via a menu). For chat, that works nicely. For story, editing might be a bit tricky on small screen. Still, it’s a convenient way to use your AI assistant on your phone when away from PC (just keep the server running at home and connect via network).

**Saving Work:** Use the Save button periodically if you’re writing something long. It will prompt to download a JSON (.kobold or .json) file that contains the conversation/story and maybe settings (if opted). You can later Load that file to continue or to have a backup if something crashes. Also the “Persist Autosave Session” option (if enabled) will save progress to browser local storage so if you refresh, it might recover.

**Idle and Multiline Settings:** In Chat mode, there are toggles like:
- *Idle Responses* – whether the AI should generate a response if you as user say nothing for a while (some RP scenarios have AI monologue on idle – not common, can leave off).
- *Multiline Replies* – whether the AI is allowed to generate paragraphs vs one line at a time. Often you want multiline for richer answers, but if you simulate a certain style (like roleplay with short messages), you might limit that.
- *Continue/Auto Mode:* Possibly an option to automatically have AI continue after its reply (like if you want it to keep writing story until you stop). Not sure if Lite has an auto-continue, but original Kobold had “Auto-gen” options.
- *User/AI Name:* In Chat, you can set the name for yourself and the AI character. These will reflect in the chat and prompt formatting. For roleplay, naming the AI as the character name helps maintain personality.

**Image and Speech in UI:** 
- If you loaded an SD model with `--sdmodel`, you’ll see an **Images** tab or a prompt field to input image generation prompt and parameters (steps, etc.). You can generate images which will appear in the UI. You can also send an image to the AI (by uploading) if using a vision model; clicking an uploaded image often gives options like “Describe (Local)” or “Describe (Horde)”.
- If you loaded a Whisper model, there might be a microphone button. Press it to speak; your speech will be transcribed into the text box (with possibly a slight delay as it processes). Then it’s as if you typed it.
- If you loaded TTS, when the AI responds, there might be a play audio button or it might auto-play the spoken version of the text. You can also find settings to select different voice (if the TTS model has multiple speakers, you might specify an ID somewhere).

**Using External UIs:** While Kobold Lite is capable, some users prefer specialized UIs like SillyTavern. That is covered in integration, but just note: if you choose to use SillyTavern or another frontend, you’d generally put KoboldCpp in `--nomodel` mode or just use it headless, and connect via API. In that scenario you might not use Kobold Lite at all.

To conclude, Kobold Lite provides a feature-rich interface to make using local LLMs accessible and fun. Each mode caters to a different use case, and by tweaking settings and using features like memory and world info, you can push the limits of what the model can do in a coherent way. Don’t be afraid to experiment with different modes and settings to see how the behavior changes – you’ll get a feel for what works best for your particular model and story/chat needs.

## 8. Advanced Features

KoboldCpp includes a variety of advanced features to enhance its capabilities beyond basic text chat or story generation. These features can help manage context length, add multimodal inputs/outputs (images, speech), and incorporate the latest techniques in model inference. Below we explain these advanced features and how to use them.

### 8.1 Smart Context and Context Shift

When generating a long conversation or story, eventually you hit the model’s context limit (e.g., 2048 tokens) and older content has to be dropped or reprocessed. KoboldCpp addresses this with **Smart Context** and **Context Shift**:

**Smart Context:** Enabled via `--smartcontext` flag, this feature reserves about half of the context space as a rolling buffer for reuse. In practice:
- Suppose context length is 2048. With Smart Context on, ~1024 tokens are treated as a buffer.
- As you approach the limit (say you have 1800 tokens in context), if certain conditions are met (two consecutive prompts with similarity above a threshold, meaning you’re continuing same conversation/story), KoboldCpp will *trigger* Smart Context.
- It will **truncate the oldest half** of the context (top 1024 tokens) and keep the most recent 1024 tokens.
- It then allows new tokens to be generated in the freed space without having to reprocess the entire history.
- Essentially, it does a big jump every time context is full: drops half, keeps half. This reduces how often it needs to recalc from scratch.
- The analogy given: it’s like a bus that, when full, kicks out half the passengers at once so that the next few stops no one needs to be kicked off, instead of kicking a few each stop (which would be constant overhead).

**Effect:** Smart Context trades off *effective* context length for speed. Only the latter half of your conversation remains in active memory of the model, earlier parts are gone. If your conversation has moved on from those, it might be fine. If not, the model might forget details from >1024 tokens ago.

**Usage:** You would use Smart Context if you want to maintain an ongoing, open-ended chat/story and prefer it not to slow down heavily once past the base context length. It’s particularly useful when running on slower hardware where a full context reprocess is expensive. Remember to turn it on at launch (no GUI toggle, only via flag).

**Context Shift:** A more advanced mechanism that works at the model’s KV cache level, **only for GGUF models**.
- Context Shift automatically removes oldest tokens from the context by “shifting” the hidden state (key-value cache) without re-feeding the remaining tokens through the model.
- It can do this token-by-token (or in small batches) as new tokens are generated, effectively creating a sliding window through the conversation.
- It *does not consume additional context space*, unlike Smart Context which sacrificed half for buffer. So with context shift, you can actually utilize the full length (e.g., 2048) for dynamic context.
- It is **enabled by default** for GGUF models in KoboldCpp. If both smartcontext and shift are on, context shift takes precedence (since it’s better).
- If you ever want to disable it (maybe to compare or if you suspect it causes an issue), use `--noshift`.

**Effect:** With context shift, you can run continuous conversations potentially indefinitely (bounded by memory usage of KV cache which is fixed by contextsize). The model will slowly forget the oldest content as new content comes in, in a seamless way. Unlike Smart Context’s sudden drop of half, context shift is gradual.

**Note:** Not all models may behave perfectly with context shift; theoretically, because the model wasn’t trained with shifting KV caches, there could be slight oddities. But it appears to work well and outputs remain coherent. If you notice output quality degradation over a very long session, it might just be the model’s inherent forgetting or it could be shift – you can test turning it off if needed.

**Fast Forwarding:** Another related feature (and on by default) is **FastForwarding**. This isn’t about dropping context, but about *not reprocessing* static context. If you send a prompt that includes the entire chat history again plus a new line, the model doesn’t recalc the history each time.
- Example: Turn 1, you had context A -> model computes response. Turn 2, your prompt includes A and new user query B. Normally the model would compute embedding for A again. FastForwarding recognizes A was just processed and reuses the cache for A, and only processes B.
- This speeds up interactions especially in chat UIs that send the full conversation every time (some do that to simplify state).
- In Kobold Lite, when you enable streaming it triggers some fast-forward logic as well, but mainly this is automatic and you don’t need to do anything. If you wanted to simulate the model *not* remembering something changed in history, you might turn it off (but generally leave it on).

**Recommendation:** Use context shift (default for GGUF) for long sessions. Smart Context is mostly there for compatibility with older models or if one doesn’t trust shift. You typically would not use Smart Context and Context Shift at the same time (and if you try, shift will override as mentioned). So:
- For GGML models (older .bin) which do not support shift, you could consider `--smartcontext` if you foresee hitting context limits and want a speed boost at the cost of memory.
- For GGUF models, just rely on context shift (no need for `--smartcontext`).
- Either way, if you want to extend usable context beyond what model was trained on, consider using the next feature: RoPE scaling.

### 8.2 Extended Context and RoPE Scaling (NTK)

Some models are fine-tuned or configured to handle longer contexts (e.g., 4K, 8K, 16K tokens). Even if not, there’s a technique using RoPE (Rotary Positional Embedding) scaling to *artificially* extend context length beyond training limits with graceful degradation. KoboldCpp supports this:

- **`--contextsize <N>`** (discussed earlier) sets the buffer sizes for context. You can put, say, 4096 or 8192. KoboldCpp can go up to 32k for GGUF.
- If you set a context size higher than the model’s native limit (usually 2048), KoboldCpp will automatically apply **NTK-Aware RoPE scaling** by default. NTK (No Token Left Behind by Scott et al) is a method where you modify the RoPE base frequency to stretch out the positional encoding.
- The default it uses: if not specified, it will pick a RoPE base such that the model effectively can use the full `--contextsize` without crazy extrapolation. Essentially, NTK scaling tries to keep the model’s perplexity curve smooth up to extended context.
- You can manually control the RoPE settings with `--ropeconfig <scale> <base>`:
  - `<scale>` is a linear scaling factor for frequency. e.g., 0.5 means double context length (2x). This is the older method (Alpha scaling).
  - `<base>` is the new base frequency for NTK scaling. e.g., base 10000 is default (no NTK extension), base 32000 might be for 4k, base 100000 for ~16k context etc. The relationship is not linear, but some known combos: `--ropeconfig 1.0 32000` ~2x, `1.0 100000` ~4x context.
- For most, letting KoboldCpp handle it automatically is fine. If a model’s output starts getting weird past a certain length, you might tweak ropeconfig:
  - Some models like **SuperHOT 8K/16K** have specific scales. The wiki mentions if defaults don't work, try a different scale. For instance, SuperHOT 8K might need linear 0.5 and base 10000 (which is just the linear method).
- Keep in mind, extending context significantly **does degrade quality** somewhat. 2x context (4k) often is okay with slight loss. 4x context (8k) the model might be noticeably less coherent at the tail end. 8x or more (16k+) you often see model struggling with consistency unless it was specifically fine-tuned for long context.
- Also, extended context uses more memory: e.g., 4k context uses ~2x the memory for KV cache compared to 2k. So ensure you have the RAM (or GPU VRAM if offloading KV).

**How to use extended context effectively:**
- Set `--contextsize` to desired maximum, but not arbitrarily high. If you think you might need up to 4k in a session, set 4096. Setting 16384 “just in case” when you’ll never use it just wastes memory and could slow things (through larger matrix sizes).
- Use the UI slider override trick: the UI slider max is 2048, but you can click the number and type a bigger one.
- If the model starts babbling or repeating in the latter half of a long session, it might be at its limit of effective context. You can at that point manually summarize or trim the beginning if needed, or if using Smart Context, it would have trimmed anyway.

**Summary:** KoboldCpp makes using >2048 context fairly easy, but be mindful of the model’s capability. Many LLaMA-based models can stretch to 4096 with NTK and still be mostly okay. Past that, expect some quirks. Always test with your specific model; some of the newer ones (Mistral, etc.) might handle it better or worse.

### 8.3 Streaming Token Output

By default, Kobold Lite shows the AI’s text only after the AI finishes or in chunks every second (polled streaming). If you prefer to see the text as it’s generated, token by token, or if you’re using an API client that supports streaming, KoboldCpp offers streaming modes:
- **Polled Streaming:** This is the default in Kobold Lite. The UI polls the `/api/extra/generate/check` endpoint every ~1 second for new tokens. The effect is you see text appear in segments. It’s simple and works without any special flag.
- **SSE (Server-Sent Events) Streaming:** KoboldCpp has an endpoint `/api/extra/generate/stream` that streams events as they happen (each token or a few tokens at a time) via SSE. Kobold Lite doesn’t use this (for compatibility and simplicity), but third-party clients like SillyTavern or Agnaistic do. In those clients, you’d enable “streaming mode” and they’ll connect to the SSE endpoint, giving a near-instantaneous character-by-character display. SSE requires the client to maintain a connection; not all tools support it, but those that do (like some OpenAI-compatible libraries) can use it if they know KoboldCpp provides it.
- **Deprecated `--stream`:** There was an older pseudo-stream implemented by some adding `&stream=1` or `--stream` flag which basically chunked responses. This is not recommended now. Instead, either rely on the default polling or use SSE via clients that support it.

**In practice:**
- If using the web UI, simply turning on the “stream” toggle in settings (if it exists) or just letting it poll is enough.
- If using an API (like hitting /api/v1/generate from a script), you won’t get streaming by default. But if you use the SSE endpoint or the OpenAI API compatibility with a stream parameter (OpenAI’s API uses `stream=true` in request and then yields chunked responses), KoboldCpp might handle that. The wiki doesn’t explicitly say, but likely the OpenAI /v1 endpoints also support streaming if the client requests it (would need to confirm with their API reference).
- True streaming (SSE) gives a smoother experience but is more complex to implement client-side (need SSE or websockets handling). Polling is easier but updates less frequently.

**Streaming and performance:** There’s a slight overhead in streaming every token (context switches, network overhead). Typically it’s negligible. But if maximizing raw throughput (for e.g., a large generation you don’t need to see until done), turning off streaming can be slightly faster. However, the difference is minor for human-scale outputs.

**Pseudo-Streaming & `streamamount`:** An old method: you could tell the model to generate N tokens at a time, send them, then continue. The wiki mentions appending `&streamamount=x` on Lite URL, but also notes negative performance and that it’s older method. We can ignore that for modern usage.

### 8.4 Multi-GPU Support

We covered multi-GPU in performance, but to reiterate advanced usage:
- KoboldCpp will use multiple GPUs automatically with cuBLAS (Nvidia). It’s one of few local solutions that do this seamlessly.
- If you want to ensure it uses all GPUs, just `--usecublas` (with no device id) and it will find all available GPUs.
- If you want to limit which GPUs: you can set the `CUDA_VISIBLE_DEVICES` environment variable before launching, or possibly use `--usecublas <device>` to specify one (though documentation only shows CLBlast taking device IDs).
- Multi-GPU with CUDA works by splitting weights. There are two split strategies:
  - **Layer-wise (default):** assign whole layers to one GPU or another. This tends to keep each GPU busy in turns (one handles its layers, then passes to next GPU for the next layers).
  - **Tensor (row) splitting:** splitting each layer’s matrices across GPUs (like each does half of a big matrix concurrently). This can in theory be faster if both GPUs work in parallel for the same layer, but it requires synchronization and is only beneficial if the GPUs are identical and the overhead is low. The wiki suggests layer split is usually best, row split might help older cards in some cases. KoboldCpp likely chooses layer split by default.
- There’s a flag in llama.cpp’s API (like `--mulmatmul` or such in some forks) to choose split type, but in KoboldCpp, the mention of “difference between row and layer split” implies maybe a config or environment variable to toggle. Not a common user tweak, unless you’re deep into performance tuning.

**Multi-GPU memory balancing:** Use `--tensor_split` if your GPUs have different memory sizes to allocate proportionally. If they have equal memory, you can usually omit it and it’ll split roughly evenly.

**Hot-swapping GPUs:** Not directly an advanced feature, but note if you started KoboldCpp and one GPU runs out of memory, it might crash; you can try reducing split and relaunch. There’s no dynamic adjustment at runtime for multi-GPU usage in the current state.

### 8.5 LoRA Adapters Support

**LoRA (Low-Rank Adaptation)** is a technique to fine-tune models by training small delta weights. KoboldCpp can apply LoRA adapters to models at load time:
- Use `--lora <lora_file>` to apply a LoRA on top of the base model. The LoRA must be in GGUF or older GGML format and compatible (meaning same base model architecture and tokenization).
- If the LoRA was trained on a higher-precision base, you might need `--lora-base <base_model_file>` to provide the reference for it. For example, you have a 7B Q4 model loaded, and a LoRA that was trained on the 16-bit model. Ideally, you’d apply the LoRA to a 16-bit base then quantize, but if not, KoboldCpp can try to apply it directly on the quantized model. It usually works but might incur some quality loss because the quantized weights + LoRA adjustments might not equal the same as base f16+LoRA then quantize.
- The dev strongly suggests merging LoRAs offline for best results, but runtime LoRA is there for convenience.
- You can also specify multiple `--lora` flags if you want to stack adapters, I believe (some tools allow multiple LoRAs sequentially). Not sure if KoboldCpp supports multiple simultaneous LoRAs via multiple flags or a combined file.

**Example:** You have a base model “Chronos-13B.Q5_1.gguf” and a LoRA “Chronos-13B-Helpdesk-LoRA.gguf” that makes it behave like a customer support agent. You’d run:  
```
koboldcpp ... --model Chronos-13B.Q5_1.gguf --lora Chronos-13B-Helpdesk-LoRA.gguf
``` 
It will load base, then load LoRA and apply it (should show a message that LoRA is applied). Then the model’s responses will reflect the LoRA’s training (in this case, being more polite and helpful, presumably).

Remember, LoRA typically adds a small memory overhead (the LoRA matrices themselves, usually a few MBs) and a tiny compute overhead. But negligible compared to the model.

If you are done with LoRA, you have to restart without the flag; there’s currently no way in KoboldCpp’s web UI to dynamically toggle LoRA on/off (except via Admin mode by loading a config without it).

### 8.6 Vision: Image Input with LLaVA and Image Generation

KoboldCpp extends some models to multimodal vision:
- **LLaVA (Vision-LLM)** and similar multimodal models: These models accept an image + text as input and produce text. Technically, the image is processed by a separate **projection model** (often a small neural network) whose output is fed into the language model as a series of tokens (embedding vectors).
- KoboldCpp allows you to attach such a projector via `--mmproj <projector_file>`.
- The projector file is a GGUF containing the vision adaptation for a specific base model. There are different projectors for different base model sizes/architectures (e.g., one for Vicuna-13B, one for LLaMA2-7B, etc.). The wiki link points to a Hugging Face with various projectors.
- You must choose the correct projector for your model, otherwise dimensions won’t match. E.g. if you have a “Vicuna 13B v1.3” model, use the Vicuna13B projector designed for that.
- Once loaded with `--mmproj`, the Kobold Lite UI will have an option to send images to the model. Typically, you click an image upload button in the chat, the image gets encoded by the projector, and the model “sees” it.
- You then ask something like “What’s in this image?” and the model can describe it. Or in roleplay, you send an image of a character and the model can incorporate what it sees.

**Limitations:** The quality of vision understanding depends on the model. LLaVA and some newer multimodal fine-tunes can be decent at image description, but they are not as good as specialized vision models for subtle details. Also, the feature “only works for GGUF models” and presumably ones that expect image tokens (the model’s vocabulary is extended to include image embedding tokens).

**`--visionmaxres <N>`** – this flag sets the max resolution for images. Higher resolution means more tokens (like splitting image into patches). The default might be 224 or 256 (like original CLIP size). If you set a lower maxres, it will downscale images to that to limit the token count, which can help speed and memory.

**Multi-Modal Workflow:** After sending an image, you likely need to click something like “Analyze” or just ask a question referencing the image. Kobold Lite might show the image inline with a button like “AI Vision” that indicates the image will be processed. Once processed, the model’s next generation will have information from the image.

**Image Generation (Stable Diffusion):** KoboldCpp integrates stable-diffusion.cpp, enabling text-to-image:
- Load an SD model with `--sdmodel <model.safetensors>`. The UI will then show an interface for generating images (like input prompt, negative prompt, steps, etc.).
- This effectively adds an **additional mode** to KoboldCpp – an internal *txt2img* API compatible with the Automatic1111 (A1111) API. You can even use it via API calls to `http://localhost:5001/sdapi/v1/txt2img` if I recall stable-diffusion.cpp’s integration, but more simply, you use the web UI.
- Flags to control SD:
  - `--sdthreads <N>`: If you want to allocate specific threads to image generation (so it doesn’t use all CPU threads and starve the text model, for instance).
  - `--sdquant`: If your SD model is in int8 format or you want to use less memory, this tries to load it quantized.
  - `--sdclamped`: In a multi-user or Horde context, clamping prevents extremely large requests – it sets defaults for max steps or resolution. It can also be configured with a number to set max pixels (like `--sdclamped 1024` to limit max dimension to 1024 or something).
  - `--sdlora`: Provide a Stable Diffusion LoRA file to apply to the image generation model.
  - `--sdloramult`: LoRA strength (like 0.5 for 50% strength).
  - `--sdvae` / `--sdvaeauto`: If you have a custom Variational Autoencoder or want to ensure a VAE is used (SDXL requires a special VAE for final image decoding).
  - `--sdnotile`: By default, stable-diffusion.cpp might tile the VAE for efficiency; this can be disabled if needed.
  - `--sdt5xxl` and `--sdclipl`: These are specifically for **SDXL/Flux** models that require an additional text encoder (T5-XXL) and a CLIP vision model. SDXL uses a two-part conditioning: one from a large T5 and one from a CLIP model. If your file doesn’t have them baked, you load them separately with these flags. (Some SDXL safetensors include them, some not.)

**Using Image Gen in UI:**
- After loading SD model, maybe navigate to an “Image” or “Stable Diffusion” tab. Enter prompt “a beautiful sunset over the ocean, oil painting style” etc. You might have fields for steps (the number of diffusion steps, e.g., 20-50), CFG scale (guidance scale), dimensions (like 512x512), etc.
- Click “Generate”. The image will be created and appear in UI. You can save it from there.
- You can generate multiple images in a row. This runs on the same thread(s) as text generation by default, so if an image is generating, text generation is paused until it finishes and vice versa. They share compute resources. On a multi-core system, you could allocate different threads with `--sdthreads` to separate a bit, but they still likely queue tasks.
- **Integration:** The neat part is, because it’s one app, you could have an AI story and at some point use the image generator to create an illustration, and even drag that image into the story. Or if using Horde, a user could call for an image and your instance can provide it if allowed.

**Horde Note:** There’s a mention of image interrogation via Horde or local CLIP. “Interrogate (AI Horde)” uses stable diffusion’s captioning via the Horde, and “Interrogate (Local)” likely uses a BLIP or CLIP-based captioner if you have one. KoboldCpp doesn’t integrate a local interrogator by default except maybe if you have a suitable model loaded via `--sdmodel` that can do it. Possibly stable-diffusion.cpp supports an `interrogate` if a specific model like BLIP is loaded.

### 8.7 Speech: Whisper (Speech-to-Text) and OuteTTS (Text-to-Speech)

KoboldCpp can turn spoken audio into text and text into spoken audio, making voice conversations possible:

**Whisper (Speech-to-Text):**
- Whisper is OpenAI’s model for speech recognition. KoboldCpp can load a Whisper GGML model with `--whispermodel <whisper.bin>`.
- These models come in sizes tiny, base, small, medium, large, and in English-only or multilingual variants. Use a smaller one for faster transcription, larger for more accuracy.
- After loading, in the web UI’s settings or top bar, you might see a microphone icon. When you click it (and allow browser mic access), it will start recording your voice.
- KoboldCpp runs the Whisper model on the audio input to transcribe it. This happens locally in your browser? Actually, the doc says “everything runs locally within your browser including resampling and wav conversion, and interfaces directly with KoboldCpp transcription endpoint”. So it sounds like the audio is recorded and preprocessed in JS, then sent to KoboldCpp’s `/api/whisper` endpoint for transcription. The heavy lifting (the model inference) is on KoboldCpp side.
- Once transcribed, the recognized text is placed into the input box as if you typed it, possibly with a short delay (depending on model size and speech length).
- You can then press send (or it might auto-send depending on VAD mode).
- **VAD (Voice Activity Detection)** vs PTT (Push-to-talk): The UI likely offers modes:
  - PTT: You hold a key or button, speak, then release, and it transcribes the recorded segment.
  - VAD/Hands-free: You click once to start, and it continuously listens; whenever you talk and pause, it finalizes that chunk of speech into text (using VAD to detect silence).
- **Threading and GPU:** By default, Whisper will run on CPU (maybe multi-threaded, the `--threads` might apply to it as well or it might use a fixed number). If you have a spare GPU or want to accelerate it, unfortunately Whisper GGML typically doesn’t use GPU (there was some experimentation with OpenCL for whisper.cpp, but not sure if integrated here).
- `--whispermodel` can be combined with normal model, no problem. It just loads an extra model in memory (Whisper tiny uses ~150MB RAM, large uses ~1.6GB).
- It's great for a Jarvis-like usage: you talk, it transcribes to text prompt, model answers in text or even voice (via TTS).
- Keep in mind large Whisper (large-v2) is accurate but slow on CPU (maybe 1x or 0.5x realtime on a good CPU). If you need quick, use tiny or base for near realtime at the cost of some accuracy (especially if you have an accent or speak quickly, the small models might slip).

**Text-to-Speech (OuteTTS):**
- **OuteTTS** is a lightweight text-to-speech model (from Oobabooga presumably) that runs via GGUF. KoboldCpp supports loading it with `--ttsmodel <speech.gguf>` and a required `--ttswavtokenizer <wav.gguf>`.
- The *wavtokenizer* is basically a model that decodes the internal representation to actual waveform (like a vocoder). OuteTTS likely generates some intermediate audio representation (like mel-spectrogram or acoustic tokens) which then the wav tokenizer converts to audible sound.
- You can optionally offload these to GPU with `--ttsgpu`, which is recommended if you have a GPU, because generating audio can be heavy (especially if long responses).
- `--ttsthreads` can control CPU threads for TTS if not using GPU.
- `--ttsmaxlen <N>` controls how many audio tokens are generated max, effectively limiting the length of spoken output. If your model tends to ramble and you don’t want a 5-minute speech, keep this moderate (the number is not straightforward seconds, but you might adjust by trial; maybe corresponds to a certain number of seconds).
- After loading, the UI will have an audio icon or setting "Enable TTS" possibly. When the AI generates a text reply, KoboldCpp will automatically pass that text to the TTS model to synthesize audio.
- The audio might then either automatically play in the browser or present a play button. Possibly a small audio player appears below the message.
- Multi-speaker: The OuteTTS model might have multiple voices (some TTS models do). The API mention to check doc for how to change speakers suggests you could send a parameter like `speaker_id` via API. The UI might include a dropdown for voice if the model supports it (for example, “male voice / female voice” etc. if present).
- This essentially gives your AI a voice. Now you can truly have a conversation: you speak -> it transcribes -> model thinks -> model replies text -> TTS speaks it.
- Latency: TTS will add some delay after text generation to produce the sound. If the response is long (several sentences), generating the audio could take a few seconds on CPU, maybe faster on GPU.
- Quality: OuteTTS is not on par with big TTS systems like Google’s or Azure, but it’s pretty good for an offline model. It may sound a bit robotic or low-sample-rate. Using a good wav tokenizer or a refined model helps. Don’t expect human-level natural prosody; expect something like an older gen virtual assistant voice.
- One can also imagine using KoboldCpp’s output with an external TTS if desired (some folks pipe it to ElevenLabs etc.), but OuteTTS gives a fully local pipeline.

### 8.8 Networking and Collaboration Features

We have dedicated Section 9 to network, but there are advanced aspects like:
- **Remote Tunnels:** The `--remotetunnel` flag to auto-run a Cloudflare tunnel for sharing over internet.
- **AI Horde integration:** Running as a Horde worker (with `--hordekey` etc.) effectively allows collaboration in the distributed network, which is advanced usage (helping others generate or using the cluster).
- **Multi-user queue:** Under the hood, multiuser mode will queue requests if two users ask at same time and serve them one by one. If you have a scenario like a group using one KoboldCpp, they won’t get jumbled responses, it will handle it.

### 8.9 Experimental Inference Features

A few more cutting-edge features included:
- **Quantized KV Cache:** We touched on it. If using `--quantkv 2` (full 4-bit KV) and `--flashattention`, you drastically cut memory at little cost. Useful on large context if memory was an issue.
- **Speculative Decoding (Draft Model):** Already explained in performance, but to reiterate: you could load a smaller model as a draft to speed up a larger model’s inference. This is experimental and not widely used yet, but in some future might become more mainstream.
- **Overriding MoE experts:** If you load a Mixture-of-Experts model (like a Switch Transformer with multiple experts), you can set `--moeexperts N` to choose how many experts to use. Perhaps if your hardware can’t handle all experts, or to see how it behaves with fewer. This is niche.
- **Embedding model usage:** `--embeddingsmodel` lets you load a small model specifically for generating text embeddings (like sentence vectors). This would expose an `/api/extra/embeddings` endpoint. You could use that for semantic search or hooking up a vector database (embedding models include ones like text-embedding-ada-002 in GGML format). This is more for devs building retrieval-Augmented generation pipelines with KoboldCpp as backend.

### 8.10 Admin Mode and Runtime Model Switching

**Admin Mode (`--admin`)**:
- When launched with admin, Kobold Lite gets an extra interface (an Admin panel) likely accessible from UI (maybe a shield icon or separate endpoint).
- In admin panel, you can have a list of configured models (the .kcpps files in `--admindir`) and you can instruct KoboldCpp to switch to one of them on the fly.
- KoboldCpp will then *restart itself* with that config. The way it’s described: “KoboldCpp will terminate current instance and relaunch to new config”. So it’s not a hot swap in-process, but a managed restart (somewhat like how certain game servers allow changing map and rebooting seamlessly).
- This is handy if you want to expose multiple models to a user without them needing to manually restart or have multiple instances. E.g., an admin user could switch from a 7B to a 13B model mid-conversation (though conversation context obviously resets unless both models share format and you feed it in again).
- `--adminpassword` adds protection to admin actions, so random users can’t switch your model by hitting the endpoint.
- This feature is somewhat experimental but useful for hosts.

**SSL and Authentication:**
- `--ssl cert.pem key.pem` to serve UI over HTTPS with given cert. This is an advanced configuration often used if you deploy KoboldCpp on a server and want encryption (like behind a domain with Let’s Encrypt cert).
- `--password` already mentioned provides a basic auth key required for any generation requests. Combine this with Cloudflare tunnel or port-forwarding for a reasonably secure remote access.

**Insights into Model Files:**
- `--analyze <model.gguf>` will print out metadata of a GGUF file (and exit). This is for curiosity or debugging – it can show what hyperparameters, training info, etc., are stored in the file.

These advanced features empower KoboldCpp to be more than just a local chat app – it can become a multi-modal assistant, a small-scale API server, or a piece of a larger system. Not every user will need all features, but it’s great to have them available. Always check the official KoboldCpp wiki and discussion forums if you run into issues enabling any advanced feature, as they might have specific quirks or version requirements.

## 9. Network Access and Remote Use

While KoboldCpp is designed primarily for local use, you may want to access it from other devices or allow others to use your instance. There are multiple ways to set up network access, each with considerations for convenience and security.

### 9.1 Local Network (LAN) Access

If you have KoboldCpp running on a computer and want to use it from another device on the same network (e.g., access it on your laptop, phone, or a home server from another PC):
- Launch KoboldCpp with `--host 0.0.0.0` (or specify your machine’s LAN IP). By default, KoboldCpp might bind to localhost only. Binding to 0.0.0.0 makes it listen on all network interfaces.
- Ensure the `--port` is open (default 5001). On Windows, it might trigger a firewall prompt – allow it on private network.
- Find the local IP of the host machine. On Windows, run `ipconfig` (look for IPv4 address). On Linux/Mac, `ifconfig` or `ip addr`.
- From the client device’s browser, navigate to `http://<HOST_IP>:5001`. For example: `http://192.168.1.50:5001`. Kobold Lite should load.
- If it doesn’t load, troubleshoot:
  - Ping the host from the client to verify network connectivity.
  - Make sure no firewall is blocking (on Windows, manually add rule if needed; on Linux, check ufw or similar).
  - Double-check the host is indeed bound. In KoboldCpp console, it should say listening on 0.0.0.0:5001 or similar. If not, it might still be on localhost – ensure the flag took effect.
- Once connected, you effectively can use KoboldCpp on one machine while it’s actually running on another. All processing still happens on the host machine, the client is just a remote control via browser.

**Use cases:** Running KoboldCpp on a powerful desktop, and accessing it from a lightweight laptop or a tablet in another room. Or hosting it on a home server (maybe headless) and connecting from any PC at home.

**Multiuser scenario:** If two people on LAN connect to the same instance (multiuser mode on by default), they can have separate sessions concurrently. KoboldCpp will queue their generation requests so if both hit “send” at same time, one will be processed then the other. Each will see only their own conversation in UI. This is a way to share a single model among family or colleagues on the same network.

### 9.2 Remote Access Over the Internet

Exposing KoboldCpp over the internet requires caution (you are essentially running an AI service that anyone could try to use if they find it). There are a few methods to do it safely:

**Method 1: Cloudflare Tunnel (Argo Tunnel)** – Easiest secure method:
- KoboldCpp includes a helper for Cloudflare’s tunneling service. On Windows, there’s `Remote-Link.cmd` script which you can run after launching KoboldCpp. On other OSes, you can install the `cloudflared` CLI.
- Newer KoboldCpp even has `--remotetunnel` flag to automatically start a Cloudflare tunnel when you launch with that flag.
- What it does: It will create a temporary public URL (like `https://some-random-name.trycloudflare.com`) that securely tunnels to your local KoboldCpp port.
- You’ll see the URL in the console or script output. It might look like: `https://blue-river-12345.trycloudflare.com`.
- Visit that URL from anywhere (your phone on 4G, or a friend across country). It will load Kobold Lite UI as if it were hosted at that domain.
- The connection is encrypted and proxied by Cloudflare, and you didn’t have to open any ports on your router.
- **Security:** The URL is obscure, but technically if someone guesses or discovers it, they could access your instance. It’s advised to use `--password` flag in combination: then the person will need the correct password (API key) to actually use the model. Without it, they’d see an empty UI that doesn’t respond to their queries (since no key).
- Also note the trycloudflare URLs are temporary and not private if someone monitors them, but practically they’re hard to stumble upon. For extended use, Cloudflare offers the ability to use your own domain or a named tunnel (beyond scope here).
- The `Remote-Link.cmd` is a convenience for Windows users; it basically runs `cloudflared tunnel` for you. On Linux/macOS, just run the equivalent cloudflared command manually (after installing cloudflared from Cloudflare’s site).

**Method 2: Port Forwarding / Direct Exposure**:
- You can forward the KoboldCpp port on your router to your machine. E.g., forward external port 5001 to internal 5001 on your PC’s IP.
- Then find out your public IP (or set up a dynamic DNS).
- Then you (and anyone with address) can access `http://<YourPublicIP>:5001` (or use a domain name if you have). 
- **Important:** This is risky unless you secure it:
  - Absolutely set `--password` so that even if someone finds it, they need the key to use it.
  - Better, proxy it behind an HTTPS server (like run an Nginx with basic auth and SSL in front). Or use `--ssl` to serve via HTTPS directly (with a certificate you provide).
  - You might restrict it to specific IPs via firewall if possible.
- Port forwarding is less ideal because it exposes an open port; bots could find it and attempt to misuse. Also, if not using SSL, anything you do is plaintext over the internet (the Cloudflare tunnel approach inherently adds SSL).
- If you do this method, definitely test from an outside network, ensure password works, etc. Realize that if someone maliciously spams your KoboldCpp, they could overload your CPU/GPU or potentially cause it to generate undesirable content (though they’d need the API password if set).
- Use this method only if you know what you’re doing and perhaps only for short-term access.

**Method 3: VPN**:
- If you have a VPN to home (like WireGuard or similar), you can just connect through that and use method 9.1 (LAN access) through the VPN.
- This is secure (because only you on VPN have access) and doesn’t expose anything publicly. But it requires setting up a VPN server on your network.

**Method 4: AI Horde (for public sharing)**:
- Slightly different approach: Instead of giving direct access to your UI, you could contribute your KoboldCpp as a **worker** to the AI Horde network. Then others (or you remotely through a Horde client) can use your model via Horde’s queue.
- To do this, you launch KoboldCpp with `--hordekey` (you get an API key by signing up on Horde) and a worker name and model name.
- Your KoboldCpp will connect to Horde and wait for tasks. It’s essentially volunteering your GPU/CPU to the community. You can restrict how many threads it uses for horde, etc.
- This isn’t exactly remote controlling your specific instance (it’s more like making it a server for all), but you could also use it yourself by going to KoboldAI Horde web UI and specifically requesting your worker by name (if you set it to public).
- If you do that, consider the impact: Horde requests might come frequently if your worker is available. You can stop the worker by quitting KoboldCpp or launching without the flag.

**Method 5: Reverse Proxies**:
- If you run something like a Caddy or Nginx reverse proxy (maybe for other services), you can integrate KoboldCpp. For instance, set up a subdomain `ai.mydomain.com` that proxies to `localhost:5001`. Use basic auth or restrict by IP for security.
- Then you can use `https://ai.mydomain.com` to access it. With proper config, you can even get Cloudflare or LetsEncrypt certificates for encryption.
- This route is similar to port forwarding but behind an existing web server that can handle security layers.

**OpenAI Proxy usage**:
- Another idea: Use KoboldCpp’s OpenAI API mode in conjunction with something like an OpenAI reverse proxy such as `oai-reverse-proxy`. This is if you want to trick an application that expects the OpenAI API into using your KoboldCpp by DNS or environment variable hacking. It’s quite advanced and for specialized scenarios (the user mention in wiki suggests it, but it's not needed for normal remote use if you can just point to the direct /v1 endpoint of KoboldCpp).

### 9.3 API and Remote Client Usage

Accessing KoboldCpp remotely isn’t only via web UI. You can use the API endpoints from anywhere (if you’ve allowed network## 9. Network Access and Remote Use (continued)

### 9.3 API Access and Remote Clients

Beyond the web interface, you might want to use KoboldCpp’s API from remote applications or custom scripts. The network setup from above applies (you must allow access via LAN, tunnel, or port forward), and then you can hit the endpoints directly:

- **KoboldAI API endpoints:** For example, `http://<host>:5001/api/v1/generate` can be called with a JSON payload (containing fields like `prompt`, `max_length`, `temperature`, etc.) to get a generated text response ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=How%20can%20I%20use%20the,from%20the%20KoboldAI%20United%20API)). If you wrote a custom frontend or chatbot, you could POST to this from anywhere (given network access). The response is JSON with the generated text and some metadata. You can find the full API spec on the KoboldCpp wiki or by visiting `http://<host>:5001/api` (which might show a reference).
- **OpenAI-compatible endpoints:** If you are integrating with an application that supports pointing to a custom OpenAI API (like many chat UIs, or coding tools):
  - Use the base URL `http://<host>:5001/v1` as the OpenAI endpoint.
  - Provide an API key if required by that app; if you set `--password <pass>`, then use that as the “API key” (the app will send it as Bearer token).
  - For example, with OpenAI’s Python library:
    ```python
    import openai
    openai.api_base = "http://<host>:5001/v1"
    openai.api_key = "test"  # or your password if set
    response = openai.ChatCompletion.create(model="your-model-name", messages=[{"role": "user", "content": "Hello"}])
    print(response.choices[0].message.content)
    ```
    KoboldCpp will treat the request as if it’s an OpenAI Chat completion (or Completion if you use that endpoint). This allows using local models in tools built for OpenAI.
  - Many UIs like **SillyTavern**, **Pygmalion web UI**, or **mobile apps (e.g., AI Chat)** allow a custom OpenAI endpoint and key – just plug in your KoboldCpp address.
- **Ollama API:** If an application expects an Ollama interface, you can similarly point it to KoboldCpp’s port (there’s likely an `/api/ollama/generate` or something that KoboldCpp handles). This is less common.
- **SSE Streaming via API:** If your remote client supports SSE (e.g., some Web apps or terminal clients for OpenAI API can handle streaming chunks), KoboldCpp will stream tokens on the `/v1/chat/completions` or via the Kobold extra endpoint when requested. Make sure your client sets the appropriate parameter (`stream=True` for OpenAI calls).
- **Multiple Users:** If you have multiple remote clients connecting, KoboldCpp’s multiuser mode will isolate their sessions. Each API call can include a `user` identifier (some API support that), but KoboldCpp mostly handles separation by connection in the UI. For pure API use, if you want to separate sessions, you might instantiate separate sessions in your calling code or run multiple instances of KoboldCpp for each user if needed.

**Example Scenarios:**
- You run KoboldCpp on a home server with `--host 0.0.0.0 --password mypass`. You expose it via Cloudflare tunnel. You then configure SillyTavern on your laptop to use “Kobold AI” API at `https://<tunnel-url>` with API key “mypass”. Now SillyTavern’s rich UI is driving your KoboldCpp instance remotely. You get SillyTavern’s interface and features, but all generations happen on your server.
- You use KoboldCpp as a pseudo-OpenAI API to connect with a chatbot mobile app. You input the custom API base and key, and now your phone app chats with your local model when you’re on the go (provided your server is accessible via internet).
- You integrate KoboldCpp into a home automation voice assistant: the assistant records speech, sends it to KoboldCpp’s Whisper endpoint for transcription, then sends the text to KoboldCpp’s generate endpoint, and finally gets audio from TTS endpoint to speak out. This can all happen over your LAN using the API calls.

### 9.4 Security Considerations (Privacy and Safety)

When enabling network access or third-party integrations, be mindful of **privacy and security**:

- **Local Privacy:** By default, if you run KoboldCpp offline (no network), your conversations are not sent anywhere. KoboldCpp does not phone home or use any external service, and all data remains on your machine. The web UI stores chat content in your browser’s memory (and optionally local storage for autosave), and the backend only logs to console. So, purely local use is private.
- **Open Source Transparency:** KoboldCpp and Kobold Lite are open source (AGPLv3), meaning anyone can inspect the code. This gives assurance that no hidden data collection or telemetry is present. You can even compile from source to be sure of what you’re running.
- **Third-Party Services:** If you use features like Web Search proxy (`--websearch`), then obviously queries go out to DuckDuckGo via the local proxy. If you use AI Horde, your generations for others and others’ prompts go through the Horde network. These potentially expose content to external servers (DuckDuckGo or Horde servers). Use such features only if you are comfortable with that. By default, they are off.
- **Horde Privacy:** When you volunteer your model to Horde, other people’s prompts will be processed by your instance. Those prompts and the model’s outputs will be visible in your console (and to Horde logging). Ensure nothing sensitive is being inadvertently shared. Also, Horde assigns an ID to your worker but not your personal info (unless you put something identifying in the worker name).
- **Authentication and Access Control:** Always use `--password` when exposing KoboldCpp beyond your trusted LAN. This ensures only people with the key can use it. It prevents misuse and also gives you control—if you share it with a friend, you can later change it (by restarting with a new password) to revoke access.
- **SSL Encryption:** If you’re accessing KoboldCpp over an unencrypted channel (HTTP) across networks you don’t trust, consider enabling SSL. You can generate a self-signed cert or use a domain with Let’s Encrypt to get a cert, then provide `--ssl cert.pem key.pem`. Alternatively, use a Cloudflare tunnel or a VPN which inherently encrypt traffic.
- **Rate Limiting:** KoboldCpp doesn’t have built-in rate limit, so if you expose it publicly, someone could spam it with requests and hog your resources. Running behind a web server like Nginx allows adding rate limiting. If it's just you or a small group using it, it’s likely fine.
- **Data at Rest:** KoboldCpp can save chats to disk (via save files or `--savedatafile` DB). These are stored locally. If you worry about disk confidentiality, treat those files as you would any sensitive document. They’re not encrypted by the program.
- **Console Logs:** Remember that prompts and outputs are echoed in the terminal by default (unless `--quiet` is used). If you walk away from your machine, someone could see recent prompts in the console window. Use `--quiet` or close the console if that’s a concern.
- **Sharing via URL:** Kobold Lite has a “Share” button that creates a URL embedding the story (like in a GET param or fragment). The wiki notes this doesn’t upload data to a server, it just encodes it in the URL itself. Still, if you share that URL with someone, they can decode the story from it. Keep in mind the URL can become extremely long. Treat those share URLs as sensitive if the content is private, since anyone with the URL can see the story embedded.
- **Personal Data:** Running local means by default no personal data leaves your machine. If you integrate with something like an online TTS service or translation API (not in KoboldCpp by default, but if you did), then data would leave. Always be aware which features involve external calls.
- **Model Content Filtering:** KoboldCpp does not censor or filter the model’s outputs by default. The content is as good or bad as the model. If privacy includes avoiding uploading content to third-party for moderation, note that here there is none – which is the point for many (no external filter).
- **Malicious Inputs:** If you expose an API publicly, someone might try to use it to generate disallowed or problematic content in your name. To be safe, keep it private or to trusted users. The models themselves might have mitigations, but not necessarily. 

In summary, **KoboldCpp can be run fully offline and privately**. If you extend it to network use, take steps (like authentication and encryption) to maintain that privacy and control who can access it. The fact that it’s on your hardware means you retain ownership of the data – unlike cloud services that might log your interactions. For ultimate privacy, keep it offline; if convenience dictates remote use, do so in a secured manner.

## 10. API Integration (KoboldAI, OpenAI, Ollama, etc.)

*(This was covered in Section 9.3 in terms of usage. We’ve interwoven API integration details throughout sections 7 and 9. To avoid redundancy, we consider Section 10 merged with those discussions.)*

**Summary of API Integration:** KoboldCpp’s compatibility with multiple APIs means you can plug it into various ecosystems:
- It works with **KoboldAI clients** (like SillyTavern, Agnaistic, Pygmalion’s webUI) by selecting KoboldAI API and pointing to your KoboldCpp server ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20SillyTavern%3F%20What%20is,How%20do%20I%20use%20them)).
- It works with tools expecting **OpenAI** or **Ollama** APIs with minimal configuration changes.
- This flexibility allows using KoboldCpp as a backend for chatbots, coding assistants, voice assistants, and more, benefiting from local model running but interfacing through well-known protocols.

*(Proceed to next section, since much of Section 10 is addressed.)*

## 11. Troubleshooting and Common Errors

Despite careful setup, you might encounter issues. Here are common problems and their solutions:

- **KoboldCpp window closes immediately (on Windows):** This usually means an error occurred on startup (e.g., missing DLL, model not found, illegal instruction). To see the error, run it via Command Prompt so the window stays. Once you see the message:
  - If it says **AVX2** not supported or illegal instruction, use the `--noavx2` flag or Old CPU preset.
  - If it says **WinError** about missing DLL, ensure you have Visual C++ Redistributables installed. The packaged .exe should include what’s needed, but if compiled yourself, you might need to ship `cublas64_xx.dll` etc. (Place them next to the exe).
  - If it says **file not found**, double-check the model path.
  - If it flashes too fast even in terminal, add a dummy `pause` at end or redirect output to a file to catch it.
- **Model fails to load (crashes or errors during load):**
  - **Memory allocation error:** If you see “not enough space in scratch buffer” or similar allocation failures, reduce `--contextsize` (if you set it high) or reduce `--blasbatchsize` to lower memory footprint for prompt ingestion. Also ensure your system has sufficient free RAM (close other apps).
  - **GGML version mismatch:** “Model detected as wrong type/version” – This might happen if you have an unusual or very new GGML model. Try using `--forceversion` with the correct version code. For example, if you suspect it’s GGUF but it didn’t detect, `--forceversion 6` (since GGUF v1/v2 is 6). The list of version codes is shown in the error message. Only do this if you are confident; forcing wrong type can cause nonsense output or crash.
  - **Corrupt model file:** If download was incomplete, the model might not load or output gibberish. Re-download from a reliable source.
  - **No such file or directory:** The path might have spaces or special characters. Enclose it in quotes in command line or use the GUI to select it.
- **CPU old instruction set (no AVX2):** As above, use `--noavx2`. If that still doesn’t work, try `--failsafe` which disables even more optimizations (SSE). Note failsafe also disables GPU usage, so it’s purely for checking if it can run at all. Windows 7 especially requires noavx2 or failsafe.
- **Performance is extremely slow on modern CPU:** Make sure you didn’t accidentally trigger noavx2 or failsafe. In GUI, selecting an “Old CPU” preset when you actually have AVX2 will cripple performance – stick to default if AVX2 is supported. On Windows, if for some reason the AVX2 code path isn’t being used (shouldn’t happen unless CPU is very old), check if the correct binary is running (the PyInstaller build should have AVX2 support by default).
- **The AI output is weird or low-quality:**
  - Could be a **bad quantization** issue. Some community quant files (especially K-quants from early conversions) were buggy. Try a different quant or source. If model outputs garbled text (not just low coherence, but actual random characters), that’s a sign. Re-download from the model’s HuggingFace (theBloke’s conversions are generally good).
  - If using a **rope scaling** beyond the model’s capability, output might degrade (model starts to ramble or repeat). Try using a smaller context or different `--ropeconfig` values.
  - Ensure you’re not using a model architecture that KoboldCpp doesn’t fully support. For instance, some models require special handling (like certain 20B models or multi-layernorms) – check release notes or try a standard model to compare.
  - Check sampler settings; extreme settings can make output odd. Reset to defaults (Temp ~0.7, Top-p ~0.9, no extreme repetition penalty) and see if it improves.
- **AI keeps talking endlessly (doesn’t stop):** Usually because the End-Of-Stream token is being prevented:
  - Go to Kobold Lite settings, find **EOS token ban** and set it to “Unban” or “Auto” instead of always banned. In Story mode, you might ban it; but in Chat/Instruct, allowing EOS lets the model naturally finish.
  - In API usage, ensure you’re not overriding `stop` sequences incorrectly. If using the OpenAI endpoint via some clients, some of them automatically stop on newline or etc., which might conflict. But in general, unbanning EOS often fixes the model not stopping.
  - If the model still doesn’t stop, you can limit `max_tokens` (Max New Tokens in UI) as a fallback to force it to cut off after a certain length.
- **AI stops too early or mid-sentence:** Opposite issue, possibly **EOS token** is mistakenly appearing. Some fine-tuned models might output the EOS special token prematurely. If you have “EOS token ban” set to Auto, try “Ban” in those cases so it treats EOS as a bad token and continues. This scenario is more rare; usually happens if model learned to emit EOS token in certain contexts (like some instruct models might output `<|end_of_turn|>` or similar).
- **Model replies as user or repeats prompt:** If in Chat mode the model starts responding with both sides (i.e., role confusion):
  - Make sure you have the correct mode selected (Chat vs Story). If you accidentally left it in Story mode while doing a conversation, the model might just continue everything including your lines.
  - Ensure your prompt format is correct. Kobold Lite handles this, but if using API you may need to include the role tags or use the right endpoint (e.g., use the chat completions endpoint rather than the raw generate).
  - In Advanced settings, check stop sequences. For example, if the model is supposed to stop when it sees `\nUser:` but your user role is “You:”, the stop sequence might not match and it might generate the user’s part. Adjust them to match the conversation format used.
- **Web UI not reachable on network:** If you did `--host 0.0.0.0` and still can’t connect from another device:
  - Possibly a firewall. On Windows, manually add a firewall rule for port 5001 (TCP) allowing inbound on private network.
  - Ensure you’re using the correct IP and that the host machine isn’t on a different subnet (for instance, if using a guest WiFi network separate from main).
  - Try `--host 0.0.0.0 --port 5002` (some port other than 5001) in case some environment has 5001 blocked or reserved.
- **UI issues:**
  - If the UI seems unresponsive or you get a white page, try hard-refreshing (Ctrl+F5) or clearing cache. The UI is static, but a partial load might cause issues.
  - If you updated KoboldCpp by replacing the binary, maybe clear the browser cache to ensure it loads the new UI files (though if version changed, usually cache-busting is handled).
  - Check the browser console (F12) for errors – if it complains about not being able to connect to certain endpoints, verify you’re using the correct protocol (if you enabled SSL, you must use https:// in URL).
- **Incompatibility with certain models/features:** If a model uses very new GGUF features that KoboldCpp version doesn’t support yet (like a new quant type), you might need to update KoboldCpp. Check for updates on the GitHub. The community is active, and by 2025, new quantization or architectures might require a newer build.
- **High VRAM usage / GPU OOM:** If running with `--usecublas` and it crashes due to CUDA OOM:
  - Lower `--gpulayers` so less of the model is on GPU.
  - Use `--lowvram` to offload some buffers to RAM.
  - If using flashattention, note it might allocate some buffer equal to context length; rarely, flashattention can increase memory slightly for certain sizes (though usually it saves memory). You could test without it.
  - Ensure no other GPU-heavy processes are running (close that game or mining software, etc.).
- **Sluggish generation after running a while:** If after many prompts the generation slows down unexpectedly:
  - Could be fragmentation of memory or just lots of data in context. If on the edge of RAM and swapping, the system might slow. Check memory usage; if high, consider using `--usemmap` so OS can manage memory better.
  - Could be thermal throttling (for long sessions, CPU/GPU might heat up and slow). Monitor temperatures. If CPU, maybe reduce threads by 1 to give it a little breathing room. If GPU, ensure it’s not overheating.
  - A rare edge: if multiuser queue built up, it might appear slow as it’s handling others – but if just you, not applicable.
- **No effect from GPU flags on Linux:** If you compiled yourself and `--usecublas` doesn’t speed up anything, you might not have actually built with CUDA support. Ensure you ran `make LLAMA_CUBLAS=1` and that it succeeded (look for “Using CUDA” in build logs). If using prebuilt, ensure you downloaded the “cuda” version.
- **Horde worker issues:**
  - If `--hordekey` is set but it says SSL errors connecting to Horde, try adding `--nocertify` to bypass certificate verification (some environments have trouble verifying the CA).
  - If your worker isn’t appearing on Horde: ensure port is open (not needed, it’s outbound to Horde servers), ensure no firewall blocking outgoing connection, check that you didn’t set `--noprior` or something limiting (there are some Horde flags to only do certain requests).
  - Ensure your `--hordemodelname` exactly matches one of the model names on Horde if you want to share publicly. Otherwise, others might not specifically request it.

If none of the above helps, consult the KoboldCpp GitHub issues or Discord community (KoboldAI Discord). Often, someone might have encountered a similar issue.

## 12. Integrations with Other Interfaces (SillyTavern, Agnaistic, Horde)

KoboldCpp can serve as the engine behind various frontends and systems:

- **SillyTavern:** A popular web-based chat frontend tailored for character RP and rich formatting. To use with KoboldCpp:
  - Launch KoboldCpp (with network access as needed).
  - In SillyTavern settings, select **API** = Kobold (or KoboldAI) and enter the address (e.g., `http://localhost:5001` or your remote URL) ([Home · LostRuins/koboldcpp Wiki · GitHub](https://github.com/LostRuins/koboldcpp/wiki#:~:text=What%20is%20SillyTavern%3F%20What%20is,How%20do%20I%20use%20them)).
  - If you set a password, input it as the API key in SillyTavern.
  - Once connected (SillyTavern might show a green connected indicator), you can chat with characters through SillyTavern, and KoboldCpp will generate the responses.
  - SillyTavern supports streaming tokens via Kobold’s SSE, so you’ll see messages appear character by character if you enable that.
  - This integration is great for multi-character RP or using SillyTavern’s extensive features like character galleries, formatting options, etc., while still running the model locally on KoboldCpp.
- **Agnaistic:** Another frontend for chatting with AI, also supporting KoboldAI API. Setup is similar to SillyTavern. Provide the base URL of KoboldCpp in its config. Agnaistic focuses on a single chat but with some unique UI choices, use whichever you prefer.
- **Pygmalion (UI) and Others:** Some communities (like Pygmalion for RP or TavernAI) have their UIs. Many are KoboldAI-compatible. Essentially, any UI that can talk to a KoboldAI server can talk to KoboldCpp. You just have to provide the URL/IP and possibly the model name (some UIs might try to fetch available models via `/api/v1/model`).
- **Text Generation WebUI (oobabooga’s UI):** That’s typically for running models itself, but if you wanted, you could possibly connect it to an endpoint. However, it’s easier to just run the model in KoboldCpp directly or in that UI; mixing them is uncommon.
- **AI Horde Integration:** Two ways:
  - As mentioned, KoboldCpp can **serve as a Horde worker** (provider). This lets you contribute your model’s power to the community. When active, you’ll see logs in KoboldCpp whenever a request comes from Horde, and the web UI will indicate it’s in Horde mode (maybe).
  - Using Horde as a user: Kobold Lite itself can connect to Horde to generate images or text. For text, Kobold Lite’s Online Services might allow connecting to Horde (like a front-end for it). But since you have a local model, you probably don't need to use Horde as a client, except for comparison or accessing models you don’t have locally.
  - If you connect Kobold Lite to Horde (via the AI Horde option in UI), none of your local models are used – you’d essentially be using remote models from volunteers. So that’s a separate mode when you have no local model. It’s either/or at any given time in the UI (either running local or connected to Horde).
- **Agnostic to Platform:** Because KoboldCpp provides these network APIs, integrations are platform-neutral. You could have an automation on a Raspberry Pi sending requests to a KoboldCpp on a PC, or a web app in Node hitting it, etc.
- **LangChain or Custom Code:** If you’re building an app (in Python, JavaScript, etc.), you can integrate KoboldCpp as a conversational AI service by calling its API, similar to how you’d call OpenAI. For example, using LangChain’s `OpenAI` wrapper but pointing the environment to KoboldCpp’s OpenAI emulation endpoint. Or using their `KoboldAI` wrapper (if one exists) with the Kobold API.
- **Voxta / Home Assistant:** There are projects to integrate AI chat with voice assistants. KoboldCpp’s local speech and text generation can be integrated. For instance, Home Assistant could trigger a script that records speech (or uses its conversation integration) and sends to KoboldCpp, then plays the TTS response. This gets into advanced homebrew integrations but it’s doable since KoboldCpp exposes needed pieces (Whisper API, generate API, TTS API).
- **Horde Web UI:** There’s a site lite.koboldai.net (Horde front end) that can connect to a specified URL. If you go there and choose “Connect to custom endpoint”, you can enter your KoboldCpp address. This is basically using the Kobold Lite interface delivered from their site but controlling your model. Usually, you’d use your own local UI though.

**Troubleshooting Integrations:**
- Ensure the versions of the client and KoboldCpp are compatible in terms of API. KoboldCpp tries to maintain compatibility, but if an integration isn’t working, double-check if any config (like a toggle for streaming or certain parameter names) needs adjustment.
- Use the developer console or logs on the client side to see if any requests are failing (maybe a 404 if wrong path or 401 if auth needed).
- One thing to note: The “model” name field in some UIs – since KoboldCpp isn’t multi-model at a time (aside from admin mode), some UIs will show a dropdown of model names after connecting (populated via `/api/v1/model` list). KoboldCpp might return only the current model as available. If the UI expects to pick a model, just select that one (should usually auto-select if only one). If none appears, perhaps the UI expected a slightly different format. In such case, try using a different integration path (OpenAI mode vs Kobold).
- If streaming doesn’t work on a third-party UI, it might require enabling SSE or using a specific connection mode. Check that UI’s docs for “streaming with Kobold”.

By leveraging integrations, you can use KoboldCpp as a drop-in replacement for cloud AI in many applications, giving you more control and no usage fees. The versatility of supporting multiple protocols is a big advantage.

## 13. Privacy and Security Features

Finally, let’s highlight how KoboldCpp addresses privacy and security, and what features exist to help you keep your data safe:

- **Local-First, Offline Operation:** KoboldCpp is designed to run entirely offline. All model inference happens on your hardware, and by default it does not require an internet connection. **No data is sent to any external server** during normal operation. This means your prompts and the model’s outputs remain local, providing a high degree of privacy out of the box.

- **No Telemetry:** There is no telemetry or usage reporting built into KoboldCpp. The developers confirm that it does not send any of your content or statistics elsewhere. (If you’re technically inclined, you can verify this by reviewing the open source code or monitoring network activity – you’ll find none unless you enable certain features like web search.)

- **Open Source and Auditable:** The entire codebase (including the UI) is open source (AGPLv3). This means anyone can inspect the code for potential vulnerabilities or unwanted behaviors. You are free to compile it yourself. This transparency is a cornerstone of trust in privacy-sensitive applications. Security researchers or hobbyists can audit how it handles data.

- **Local Data Storage:** Any chat histories or story content you create in the UI are stored locally:
  - In the browser, your content can persist in local storage for autosaves (if that setting is enabled). Otherwise, it's in memory until you save it. When you click “Save”, a file is saved to your disk (or whatever location you choose). KoboldCpp doesn’t automatically send it anywhere else.
  - If you use `--savedatafile`, that database (likely a SQLite DB) resides on the machine running KoboldCpp. It’s not transmitted externally.
  - The “Share” feature encodes data in a URL but does not send it to a cloud service. However, anyone with that URL can decode the content, so treat shared links carefully (the data is base64 or similar encoded in the link itself).
  - No cloud storage integration (unless you manually copy files to cloud).

- **Password Protection (`--password`):** If you open up your KoboldCpp to a network (LAN or beyond), you can enforce an **API key/password** requirement on all usage. This is essentially authentication to prevent unauthorized access. The UI and API both honor it:
  - When set, the browser UI will ask for a key on connection (a popup or a field). Without the correct key, the UI won’t allow generations.
  - API calls must include an `Authorization: Bearer <password>` header. If not, they’ll be rejected (HTTP 401).
  - This is important for security when running on a server. It’s a simple single-key auth (not per-user accounts), but often sufficient for personal use.
  - If collaborating, you can share the key with a friend. If needed, change it by restarting with a new `--password`.
  
- **SSL/TLS Support (`--ssl`):** KoboldCpp can serve content over HTTPS if you provide a certificate and key. This ensures that if you access it remotely (or even on LAN, to prevent local network snooping), the traffic is encrypted. While for purely local use this isn't needed, it’s recommended for any internet exposure. You would generate or obtain a certificate (e.g., via Let’s Encrypt if you have a domain, or a self-signed one for personal use).
  - Example: `koboldcpp --ssl fullchain.pem privkey.pem --password MySecretKey`.
  - After that, you’d access it via `https://` and the data (prompts, responses) is encrypted in transit.
  - If using a self-signed cert, your browser will warn that it’s not from a trusted authority – you can bypass that for yourself (since you created it). For a more seamless setup, a domain with a valid cert is best.

- **Multi-User Isolation:** KoboldCpp’s multiuser mode segregates sessions by client, meaning Person A’s chat history isn’t visible to Person B connecting from another device (except on the server console). Each client’s prompts and generations are queued and routed back only to that client. This is a kind of basic privacy between users on the same instance. However, be aware that the **server console will log all prompts** from all users unless `--quiet` is used.
  - If you are running a shared instance and want to avoid seeing others’ content (or having it in logs), run with `--quiet`. Then the server won’t print the text of prompts/outputs to the terminal (just some status logs).

- **Memory of Content:** Once KoboldCpp is closed, the model does not retain memory of your chats (the model’s state is not saved unless you explicitly save a story or snapshot). There is no risk of a cloud model storing your conversation because it’s all local. If you want to be extra sure, you can avoid saving any logs or simply delete the save files when done.

- **External Features Opt-In:** Features that involve external services (like AI Horde, web search, Cloudflare tunneling) are opt-in and explicitly triggered by flags. By default, none of those are active. So you won’t accidentally send your data out unless you meant to. For example:
  - *Web Search* requires `--websearch` and a query in a prompt that triggers it.
  - *Horde* requires `--hordekey` etc., which you’d know if you set.
  - *Image generation via stable diffusion* runs locally (downloads model from Hugging Face, but that’s just a one-time download).
  - *Whisper and TTS* are local. Your microphone audio is processed in the browser and by local KoboldCpp – no cloud speech service.
  
- **Sensitive Data Handling:** If you’re using KoboldCpp for sensitive or personal information:
  - Prefer offline usage (no tunnels open).
  - If you do remote in, ensure encryption and auth as described.
  - Realize that if someone gains access to your machine, they might find chat histories in the save files or console output. So apply normal computer security: use OS account passwords, disk encryption if appropriate, etc.
  
- **AGPL Consideration:** AGPL license means if you modify KoboldCpp and provide it as a service to others over network, you are obliged to share your modifications. Using it privately or even sharing with friends without code changes doesn’t trigger any requirement. Just something to note if you ever decided to integrate it into a web service and alter the code.

In conclusion, KoboldCpp is inherently privacy-friendly by virtue of being local and open source. The main things for a user to do are:
- Use `--password` for any network access.
- Use `--ssl` or a secure tunnel for encryption.
- Keep your machine secure as you would normally.
- Understand what features send data out (and in default usage, essentially none do).

By following these practices, you can enjoy the capabilities of KoboldCpp with confidence that your data remains your own and protected.

---
