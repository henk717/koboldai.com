# KoboldCpp NotivaAI
### What is NotivaAI?
NotivaAI is a cloud hosting provider with a focus on GPU rentals that you can pay per minute. They offer various GPU's at competitive prices. They work with our project to ensure their service is easy to use for KoboldCpp users. In this guide we will focus on setting up the KoboldCpp template.

Strengths of NotivaAI:

- One of the easiest to use

- Similar or lower GPU prices than other easy to use providers

- Works directly with us to ensure proper compatibility

- CUDA support is up to date.


Weaknesses of NotivaAI:

- Smaller variety of available GPU's than other providers (The popular ones such as the 3090, A6000 and A100 80GB are available)

- The GPU's you want may not always be available.

### Using KoboldCpp on NotivaAI with the default settings and default model
1. Visit [https://koboldai.org/novitacpp](https://koboldai.org/novitacpp) to be taken to NotivaAI with the correct settings loaded.
2. All GPU's can run on the default settings, pick the cheapest GPU that is available and click its Deploy button.
3. Click Next and then click on Deploy.
4. Your instance is now launching. You can monitor the (down)loading progress by clicking on the log button and then switching to Instance Logs.
5. Once KoboldCpp has finished loading you can connect to its UI by clicking on the Connect button and then Connect to HTTP service (Sometimes it may tell you that its not available despite already being available, its better to just click and refresh the page if you get an error). Need an API link instead? Check the log's container tab for all the API links.


### Finding your own GGUF models on Huggingface
1. You can find GGUF versions of the models on [Huggingface](https://huggingface.co/models?sort=trending&search=gguf). Once you are on this website use the searchbar by typing in the name of the model you wish to use followed by GGUF.
2. Once you have found your model click on the "Files and Versions" tab.
3. You now see a list of models, next to the quant size you want (We recommend Q4_K_S if you are unsure) is a small download icon. Right click and copy the link. This is the link you will use for KCPP_MODEL.
4. If there are multiple parts you can seperate them by a comma so then it would become "Link1,Link2,Link3" etc.

### Using your own GGUF models with the KoboldCpp on NovitaAI

1. Visit [https://koboldai.org/novitacpp](https://koboldai.org/novitacpp) to be taken to NotivaAI with the correct settings loaded.
2. Select a GPU big enough to fit your model and click its Deploy button.
3. Paste the link to the GGUF you wish to use in the KCPP_MODELS field seperate the multiple parts in the correct order with a comma if there are multiple parts. (You can do the same for the image gen model)
4. Click Next and then click on Deploy.
5. Your instance is now launching. You can monitor the (down)loading progress by clicking on the log button and then switching to Instance Logs.
6. Once KoboldCpp has finished loading you can connect to its UI by clicking on the Connect button and then Connect to HTTP service (Sometimes it may tell you that its not available despite already being available, its better to just click and refresh the page if you get an error). Need an API link instead? Check the log's container tab for all the API links.