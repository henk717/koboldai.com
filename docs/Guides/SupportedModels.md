### **Supported LLM models**
Kobold.cpp supports all models that LLAMA.cpp supports, as well as no longer supported models.
a few examples include:
- Llama, Llama 2, Llama 3, Llama 4
- Mistral, Mixtral, devstral, mistral small, mistral large
- Qwen, Qwen2, Qwen2VL
- Gemma, Gemma 2, Gemma 3
- GPT 2, GPT NeoX, GPT4All, GPT-J
- Phi-2, Phi-3
- RWKV4
- variants and finetunes of the above formats, including Miqu, Yi, Cerebras, Pythia, Falcon, Starcoder, Deepseek, etc.

### **Samplers**
Kobold.cpp supports various samplers:
- Top-K: This setting limits the number of possible words to choose from to the top K most likely options, removing everything else. Can be used with Top-P. Set value to 0 to disable its effect.
- Top-A: Alternative to Top-P. Remove all tokens that have softmax probability less than top_a*m^2 where m is the maximum softmax probability. Set value to 0 to disable its effect.
- Top-P: Discards unlikely text in the sampling process. Only considers words with the highest cumulative probabilities summing up to P. Low values make the text predictable, as uncommon tokens are removed. Set value to 1 to disable its effect.
- TFS: Alternative to Top-P, this setting removes the least probable words from consideration during text generation, considering second order derivatives. Can improve the quality and coherence of the generated text.
- Typical: Selects words randomly from the list of possible words, with each word having an equal chance of being selected. This method can produce text that is more diverse but may also be less coherent. Set value to 1 to disable its effect.
- Temperature: Controls how 'Random' the output is by scaling probabilities without removing options. Lower value are more logical, but less creative.
- Repetition Penalty: Applies a penalty to reduce usage of words that have already been used recently, making the output of the AI less repetitive.
- Mirostat: Alternative sampling method that overrides other samplers. See mirostat section.
- Min-P: An experimental alternative to Top-P that removes tokens under a certain probability. Set value to 0 to disable its effect.
- DynaTemp: Dynamic Temperature Sampling is a variant of normal Temperature sampling where the temperature is allowed to automatically vary between two preset limits. Temperature is allowed to be automatically adjusted dynamically between DynaTemp ± DynaTempRange. Set DynaTemp Range to 0, or set min and max to the same value, to disable it.
- DRY: DRY is a dynamic N-gram anti-repetition sampler, used as an alternative or together with Repetition Penalty. Only recommended for advanced users.
- XTC:  XTC (Exclude Top Choices) sampler is creative writing sampler designed by the same author of DRY. It removes common tokens with high probability when there are many choices, allowing for more creative and flavorful text. To use it, increase xtc_probability above 0 (recommended values to try: xtc_threshold=0.15, xtc_probability=0.5)

### **Quants**
Kobold.cpp supports all quant variants supported by LLama.cpp.
For all cards:
- Q#
    - Q2, Q3, Q4, Q5, Q6, Q7, Q8, F16
- Q#_K
	- Q2_K, Q2_K_XXS, Q2_K_XS, Q2_K_S, Q2_K_M, Q2_K_L, etcetera
- Q#_0
	- Q4_0, Q5_0, Q8_0
- Q#_1
	- Q4_1, Q5_1

Not for Vulkan but works with CUBLAS/HIPBLAS:
- IQ#
	- IQ1_xxs
	- IQ1_s
	- IQ3_m
	- etcetera

Only benefits on mobile, but works on other systems:
- Q4_0_4_4
- Q4_0_n_m
- Q4_0_4_2

- In general the quality (from worst to best) and filesize (from smallest to biggest) follows this order: 
- Q2K, Q3_K_S, Q3_K_M, Q3_K_L, Q4_0, Q4_K_S, IQ3_L, Q4_1, Q4_K_M, IQ4_L, Q5_0, Q5_1, Q5_K_S, Q5_K_M, IQ5_L, Q6_K, Q8_0, F16

Kobold.cpp does NOT support pytorch .bin files or safetensor files natively, Please see the section on model conversion for aid. It does however support GGML .bin files.

### **Where can I find or download GGUF and GGML models for KoboldCpp?**  
- GGML models can be found uploaded on [Huggingface](https://huggingface.co/models), simply by searching for `GGML` or `GGUF`. They should be a file in `.bin` or `.gguf` format
- Since TheBloke left, [Barowski](https://huggingface.co/bartowski) and [Mradermarcher](https://huggingface.co/mradermacher) have taken over as the Best sources for GGUF quants.
- A large selection of older high quality models can also be found on [TheBloke's Huggingface Repo](https://huggingface.co/TheBloke), look for GGUF/GGML. 
- Lastly, you can also convert the models yourself, using the [appropriate quantization and conversion tools](https://kcpptools.concedo.workers.dev). 

### **What's the difference between GGUF and GGML formats**  
GGUF is a newer format designed to (hopefully) be more future proof. As of Oct 2023, it is the latest and recommended format for LLAMA and LLAMA2 models. For models released before 2024, GGML is preferable. For models released since the start of 2024, GGUF is preferable. KoboldCpp remains compatible with any version of both formats, but older GGML variants will not see the same speed improvements that newer GGUF files get.

### **What are the differences between the different files for each model? Do I need them all? Which Quantization? F16? Q4_0? Q5_1?**  
No, you don't need all the files, just a single one. GGML Model files are typically a single .bin or .gguf file, with files over 40GB typically being 0000x-of-0000y.gguf, which you need every part of the same file (ie: 1-4 if its 00004.gguf). 
The multiple files represent different compression levels of each model. [Read more here](https://www.reddit.com/r/LocalLLaMA/comments/13l0j7m/a_comparative_look_at_ggml_quantization_and/).

### **Supported Image Generation Models**
It also supports all model formats that Stable-Diffusion.cpp supports, including:
- Flux1.Dev, Flux1.Schnell
- Stable Diffusion 3, 3.5
- Stable Diffusion 2
- Stable Diffusion XL
- Stable Diffusion 1.5
- PhotoMaker

In addition, it supports various subversions of each, such as:
- SD-Turbo
- SDXL-Turbo
- LCM

For image generation, Safetensors are supported, unlike with text generation. However, they are supported by way of automatically converting on launch, it is preferable to manually convert if you wish to reuse the same model often.

### **Image Recognition**
For Image recognition, Kobold.cpp supports Llava vision, Qwen2VL, and Gemma 3 using [MMproj files](https://huggingface.co/koboldcpp/mmproj/tree/main). It will typically support every Vision mmproj that Llama.cpp supports, and if it doesnt, wait a few hours.



### **Speech**
For speech recognition, Kobold.cpp supports [Whisper models](https://huggingface.co/koboldcpp/whisper/tree/main).

For speech generation, Kobold.cpp supports [OuteTTS](https://huggingface.co/koboldcpp/tts)


### **Embeddings**
Kobold.cpp supports all variants of Embeddings that are supported by Llama.cpp. This includes models like [snowflake](https://huggingface.co/mradermacher/snowflake-arctic-embed-l-GGUF)
