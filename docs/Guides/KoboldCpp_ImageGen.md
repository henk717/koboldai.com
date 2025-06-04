# Image Generation
KoboldCPP supports generation of Images using Stable diffusion models such as SD 1.5, SDXL, SD 2.0, Flux, SD 3.5, and so on. 
For full details, please see https://github.com/leejet/stable-diffusion.cpp

## Image Gen Settings
- #### Image Gen Model
    - A Safetensors or GGUF GGML image diffusion file. Due to GGUF being a container file, ComfyUI GGUF files are not compatible, only SD.CPP GGUF Files should be used.
- #### Clamped Mode
    - Primarily for multiuser, will reduce the size requested to this limit to prevent long gens from reducing usability
- #### Image Threads
    - See [Threads](KoboldCpp_Hardware.md) in Hardware section.
- #### Compress Weights
    - Quants the model to Q4_0 to allow it to run faster at the cost of quality. On larger models (Flux) this is more beneficial with lower cost than on smaller models (sd1.5).
- #### Image LoRA
    - A LoRA adapter to add information to the Image Gen model. Will be applied to everything. See another guide for detailed information on this.
- #### Image Lora Multiplier
    - Weight of the above option.
- #### T5-XXL File
    - T5-XXL is used by the image gen model to convert from text to input tokens. This should only be set on Flux and SD3.0/SD3.5. Not used for SDXL and older models. This is used for longer descriptions to be converted
- #### Clip-L File
    - Like T5-XXL, This is used primarily for converting "tags" to tokens. 
- #### Clip-G File
    - Like T5-XXL, this is used primarily for converting short descriptions to tokens.
- #### Image VAE
    - Converts image output tokens to something visible
- #### Use TAE SD (AutoFix Borken VAE)
    - Used for SDXL when loading a model without an updated VAE. Older SDXL models require this if they generate monochrome images (all black, all green, all blue) or gibberish images (check Clip skip first, this second)
- #### No VAE Tiling
    - VAE Tiling reduces memory usage of converting from Image output tokens to visible image, at cost of quality. This prevents it from being used at all.

## Concedo model:
LostRuins/Concedo has the following available: https://huggingface.co/koboldcpp/imgmodel where older is 1.5 and the other 2 are sdxl.

## Clip/T5:
Clip and T5 are used simultaneously for all requests, the mentioned distinction is just for those using in highly advanced settings (ComfyUI) and not necessarily used in Kobold in this manner.