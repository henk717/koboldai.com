# Hardware options compatible with Kobold includes the following:
yes, you can run Kobold on your android phone, you macbook, your windows desktop, your server, basically everything.
it has yet to be tested on those fancy japanese toilets, if you would, please DO NOT! send us a picture of it working on those. But do send it working on your android smart fridge if you get it working.

Known working hardware options:
 - CPU:
  - Intel: Core series newer than 7th Gen should be fully compatible
  - AMD: AM4 and AM5 CPUs Should all work
  - Qualcomm: Snapdragon 888 and newer mobile CPUs should work
  - If not working, try "old CPU", this will work on older options, including some "retro PC" builds
 - GPU:
  - NVidia:
   - Minimum recommended is "Pascal": the GTX 10 series over the 1050. 1030 is not recommended.
   - Workstation cards such as the quadro p4000 are fully compatible
   - Server cards such as the tesla p40 are fully compatible
   - Absolute bare minimum "it works, but" is kepler, the GTX 700 series (NOT The GTX 600 series version of kepler)
  - AMD:
   - RDNA2 and newer work. This is the "Navi 21" series such as the RX 6800. 
   - CDNA1 and newer work as well. This is the Instinct MI100 and similar.
   - Vega 10 and newer work but are no longer recommended. This is the MI25, MI50, MI60. 
   - Absolute bare minimum "it works, but" is GCN cards such as the FirePro. These are not recommended as the amount of time to get them working you could have made enough to get the next card up by selling lemonade at a stand.
  - Intel:
   - Intel Arc Alchemist and newer cards work using Vulkan.
   - Intel Iris Xe is not recommended and is untested with Kobold. If the card supports Vulkan, then it may work with Kobold, but little to no testing has been done since other than the onboard graphics options, all Xe cards are datacenter. 

## Hardware Options in Launcher:
 - #### Presets
  - Use CPU: This will ignore your GPU if you have one. rarely the best option.
  - use CuBLAS: This will use Cuda on an NVidia GPU. if you have no NVidia GPU, then do not use (unless you know what you are doing)
  - Use Vulkan: This will use Vulkan on any GPU or APU. This is recommended for Intel GPUs as the default option, and for AMD GPUs when ROCM isnt an option.
  - Use CLBlast: This is a graphics card agnostic option like Vulkan, but generally slower.
  - (old cpu) options: like the above, however they should be used only as a fallback when the above has a crash related to hardware incompatibility.
 - #### GPU ID:
  - This will show up to 4 GPUs on your system, you can use this to select which GPU to utilize, or use the "ALL" option to utilize all available GPUs.
 - #### GPU Layers:
  - Layers are parts of a model, different models will have different numbers of layers, such as Gemma 3 having 63 layers. There will not be a comprehensive list provided as every model has a different count. Other than the first layer (input) and last layer (output) Layers are generally the same size. typically this means you can divide the file size by the layer count to determine the size of each layer.
  - This setting tells kobold how much of the model to put on the GPU and how much should remain on the system ram, more layers on GPU will generally be faster.
  - -1: This will attempt to find the amount of vram available and set the model to offload a layer less than the expected max capacity. This generally will work, but manual editing can get it more precise.
  - Any other number: if the number is greater than the number of layers in the model, it will load all layers on the GPU. 
 - #### Threads:
  - Threads is max number of threads Kobold will run simultaneously, this defaults to 1 less than the number of cpu cores available. increasing will mean that if Kobold hangs, the system may hang, but may provide performance benefits. decreasing will provide little/no benefits and just cause slowdowns instead.
  - This setting pertains to the generation side of the AI, but will be used as the default for the other "threads" options as well.
 - #### Blas Threads:
  - Same as "Threads", but this specifically relates to Prompt Processing and nothing else.
 - #### Blas Batch Size:
  - Batch size controls how much of the prompt is processed at once. on higher end GPUs, increasing may improve speed, but will increase VRAM usage. generally 512 is your best option, though GLM4 based models require a much smaller batch size
 - #### Launch Browser:
  - This will use your system default HTTP/HTTPS protocol handler to open the browser. (This will open your default browser to kobold automatically once the model is loaded)
 - #### High Priority:
  - On windows, this sets the Task priority from "normal" to "high". This sometimes improves speed at the cost of slowing down other processes on the system. on Linux this does nothing
 - #### Use MMAP: 
  - Use Memory Map, this generally speeds up loading of the model as it "holds" the memory when it starts loading rather than waiting for it to need more memory to load the next chunk of the model. On some hardware this causes the model to not "unload" properly, but generally should be safe to enable
 - #### Use mlock:
  - This "locks" the model in memory until the program is closed. This prevents it from slowing down when chrome decides it needs to use another 8gb ram.
 - #### Debug Mode:
  - Provides extra info in terminal and UI. Generally not needed.
 - #### Keep Foreground:
  - Brings Kobold to the foreground each generation, helps prevent computer from going to sleep
 - #### CLI Terminal Only:
  - does not launch the Kobold lite interface, and ignores all networked requests, only launches in commandline mode.
 - #### Force Version:
  - For use with pre-GGUF models (ie: GGML), generally not needed anymore. 0 automatically detects which version to use.
 - #### Run Benchmark: 
  - Tests your hardware
