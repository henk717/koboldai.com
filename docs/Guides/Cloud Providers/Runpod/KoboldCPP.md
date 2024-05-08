# KoboldCpp Runpod
### What is Runpod?
Runpod is a cloud hosting provider with a focus on GPU rentals that you can pay per minute. They offer a wide range of GPU's at competitive prices. The best part is that they allow their community to make templates so that you can get up and running without having to do any difficult setup. In this guide we will focus on setting up the KoboldCpp template.

Strengths of Runpod:

- Easiest to use of all the cloud providers.

- Secure Cloud is consistent, community cloud is cheaper.

- Large variety of GPU's.

- CUDA support is up to date.

- Docker based, so you run our official runtime with maximum support.

- Very fast downloads in a lot of the locations (We measured as high as 600mb/s of actual download speed in Sweden)!

- Cheaper than many providers.

Weaknesses of Runpod:

- More expensive than VastAI / TensorDock which are both harder to use.

- The GPU's you want may not always be available.

### Using the KoboldCpp template with the default settings and default model
1. Visit [https://koboldai.org/runpodcpp](https://koboldai.org/runpodcpp) to be taken to Runpod with the correct template loaded.
2. Select a GPU with 16GB of VRAM such as the A4000.
3. Click Deploy On-Demand
4. Your template is now launching, KoboldCpp has a fast startup time and in most server regions will be available in 2 minutes. You can monitor the (down)loading progress by clicking on the log button (At the top of the logs is a play button for automatic refreshing, also make sure you are on the container logs tab).
5. Once KoboldCpp has finished loading you can connect to its UI by clicking on the Connect button and then Connect to HTTP service (Sometimes it may tell you that its not available despite already being available, its better to just click and refresh the page if you get an error). Need an API link instead? Check the log's container tab for all the API links.


### Finding your own GGUF models on Huggingface
1. You can find GGUF versions of the models on [Huggingface](https://huggingface.co/models?sort=trending&search=gguf). Once you are on this website use the searchbar by typing in the name of the model you wish to use followed by GGUF.
2. Once you have found your model click on the "Files and Versions" tab.
3. You now see a list of models, next to the quant size you want (We recommend Q4_K_S if you are unsure) is a small download icon. Right click and copy the link. This is the link you will use for KCPP_MODEL.
4. If there are multiple parts you can seperate them by a comma so then it would become "Link1,Link2,Link3" etc.

### Using your own GGUF models with the KoboldCpp template

1. Visit [https://koboldai.org/runpodcpp](https://koboldai.org/runpodcpp) to be taken to Runpod with the correct template loaded.
2. Select a GPU big enough to fit your model.
3. Click on Edit Template.
4. Click on Expand Variables.
5. Paste the link to the GGUF you wish to use in the KCPP_MODELS field seperate the multiple parts in the correct order with a comma if there are multiple parts. (You can do the same for the image gen model)
6. Click Set Overrides.
7. Click Deploy On-Demand
8. Your template is now launching, KoboldCpp has a fast startup time and in most server regions will be available in 2 minutes. You can monitor the (down)loading progress by clicking on the log button (At the top of the logs is a play button for automatic refreshing, also make sure you are on the container logs tab).
9. Once KoboldCpp has finished loading you can connect to its UI by clicking on the Connect button and then Connect to HTTP service (Sometimes it may tell you that its not available despite already being available, its better to just click and refresh the page if you get an error). Need an API link instead? Check the log's container tab for all the API links.