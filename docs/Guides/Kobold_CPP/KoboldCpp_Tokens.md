## Tokens
Tokens are words, phrases, or simple characters as a single numerical value. These are utilized because trying to run an AI with every single character would take vastly more ram and vram, as well as being significantly slower.  
The following will show you how an AI sees your text: https://huggingface.co/spaces/Xenova/the-tokenizer-playground

## Token options in launcher:
- #### Use ContextShift:
    - Reduces reprocessing when continuing a conversation with the LLM.
    - This will take remove the oldest tokens and leave the newest tokens behind, this allows you to send longer prompts without losing the newest (and probably most important) messages
    - When a brand new model architecture appears, sometimes has some issues for a short period of time, but generally will be resolved rather quickly.
- #### Use FastForwarding:
    - Reduces reprocessing when continuing a conversation with the LLM. Similar effect to ContextShift, but different concept.
    - This will just continue to use the existing prompt and add to it rather than replace it.
- #### Use Sliding Window Attention (SWA):
    - Similar to Context Shift, but more noticeable improvements in Memory usage on the models that directly support it (Gemma 3) and little/none on models that dont (llama 4)
- #### Context Size:
    - How much information the model can see at once.
- #### Default Gen Amt:
    - Controls default response length for API requests. Typically will be overriden by a manual setting in your endpoint (E.G.: Kobold Lite, Talemate, etc)
- #### Custom RoPE Config:
    - 
    - ##### RoPE Scale:
        - default of 1.0
    - ##### RoPE Base:
        - Default of 10000
- #### Use FlashAttention: 
    - This is disabled by default due to being hardware and model dependent.
    - On NVidia and some AMD GPUs this will significantly improve prompt processing.
    - On CPU and some AMD GPUs this will significantly degrade prompt processing
    - If you do not have full offload of the model (set GPU layers to max) then this may cause slowdowns
- #### Quantize KV Cache:
    - Reduces the size of your prompt by Quanting the resultant context. Will reduce the size in memory, but may cause the model to act dumber.
- #### No BOS Token:
    - Drops the BOS token that is automatically added to prompts in the background. This is needed for some models, but not all. Check the model HF page for more info.
- #### Enable Guidance
    - Allows use of a "negative prompt", which may slow down interaction but can prevent unwanted content.
- #### MoE Experts:
    - MoE models have multiple "experts" operating on the context simulataneously, with this you can set how many are active at any given time, potentially reducing memory usage and improving speed at the cost of intelligence.
- #### Override KV:
    - Allows manually setting device for specific components of your context
- #### Override Tensors: 
    - Allows manually setting the device for specific components of your model.
    - This is required if you can not fully offload GLM4, and will provide speed benefits to some other model architectures if you can not offload fully. If you can fully offload will have less of an impact.