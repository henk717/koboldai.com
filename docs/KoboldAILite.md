# KoboldAI Lite

## What is KoboldAI Lite? How do I use it? 
KoboldAI Lite is a lightweight, standalone Web UI for KoboldCpp, KoboldAI Client, and AI Horde, which requires no dependencies, installation or setup. It comes pre-bundled with all distributions of KoboldCpp and is ready to use out of the box. 

**After starting KoboldCpp (default port is 5001), navigate your local browser such as Chrome, Firefox or Safari to http://localhost:5001 and KoboldAI Lite will be launched.**

How KoboldAI Lite welcomes you in Chrome with our classic UI:
![KoboldAI Lite](../img/KoboldAILite.png "KoboldAI Lite")

We also provide KoboldAI Lite as KoboldAI.net read our [definition of KoboldAI.net](Koboldai.net.md) if you don't want to setup KoboldCpp locally.

### **Basic Modes of KoboldAI Lite**  
KoboldAI Lite has 4 different modes, which you can toggle using the 'Format' Dropdown inside the "Basic Settings" panel. 
- Story Mode: For creative fiction and novel writing, the AI continues your story based on your input.
- Chat Mode: Simulates a character persona with an interactive AI chatbot. Ask the AI anything, or chit-chat with it in turn based conversation.
- Instruct Mode: ChatGPT styled instruction-response. Give the AI a task, and it will try to fulfill the instruction.
- Adventure Mode: AIDungeon styled interactive fiction, choose-your-own-adventure, describe an action and the AI narrates the result.
The best way to get started after launching KoboldAI Lite is to jump into a pre-crafted **Scenario**, which you can select from the "Scenarios" button.

### **UI Style Select**  
In newer KoboldAI Lite versions, you can pick from 3 different UIs (not all are available in all modes).
- Classic: This is the default Kobold notepad look and feel, simple, clean, efficient, and available for all modes.
- Messenger: This is an alternative UI for chat mode, which shows up as a messenger style chat between you and the AI.
- Aesthetic: This is an alternative UI for chat and instruct mode, which allows great customization of text styles, colors, padding and inclusion of image portraits.

### **What are samplers? How do I change or disable them? What are the best samplers?**  
Samplers are basically how the AI determines the next token to choose, from the list all possible tokens. There are many different samplers with different properties, though you will generally only need a few. Good defaults to use are Top-P=0.92, RepPen=1.1, Temperature=0.7 and a sampler order of [6,0,1,3,4,2,5], leaving everything else disabled (default).  
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

### **What is sampler order? What is the best sampler order? I got a warning for bad suboptimal sampler orders.**  
Sampler order controls the order in which the above samplers are applied in sequence, to the list of token candidates when choosing the next token. It is **STRONGLY** not advised to change this from the default of [6,0,1,3,4,2,5] as that can lead to very poor outputs.

### **What are custom presets?**  
Presets are pre-configured sampler settings that have been contributed or collected over time to emulate specific writing styles or platforms. Some of them have sub-optimal configurations or sampler orders, but they should be considered artistic rather than practical - you will likely still get optimal results from the default preset.

### **Max Ctx. Tokens (Context Size)**  
Context size determines the maximum number of tokens (context window) that will be sent to the AI, in other words, it controls how far back in, and how much of the text the AI gets to access, remember and use. Most models are limited to 2048 tokens of context, but some have been trained with larger context sizes. Bigger contexts take more memory and are slower to process and generate with. To extend context, refer to the sections on "longer context above 2048" and "RoPE scaling". This field can be manually overridden past the slider limit by editing the text input field.

### **Amount to Generate**  
Maximum number of tokens the AI can generate for it's response to each submitted request. Each token is roughly about four letters long.

### **Token Streaming**  
Enable this option to allow tokens from an incomplete AI response to be gradually streamed into the UI instead of only responding when the generation is complete. Not applicable for AI Horde users.

### **Trim Whitespace**  
This option combines multiple consecutive newlines into one single newline. It also removes trailing whitespace at the end of the submitted prompt.

### **Trim Sentences**  
This option trims the AI's response down to the last complete sentence, if possible.

### **EOS Token Ban**  
This option controls the AI's usage of the End-Of-Stream Token, a special token that lets the AI stop responding early when it thinks the response is complete. It replaces the old `--unbantokens` launcher flag.  
- Auto: Automatically determine whether to use EOS tokens or not.
- Unban: Always allow the EOS token to be used.
- Ban: Prevent the EOS token from being generated.

### **Placeholder Tags**  
This option allows the placeholder tags `{{user}}` `{{char}}` `{{[INPUT]}}` and `{{[OUTPUT]}}` to be used by character card or scenario authors, which will be dynamically replaced with the correct value on runtime. For example, `{{char}}` will get replaced with the chatbot's selected nickname.

### **Persist Autosave Session**  
This option autosaves your story and settings, which will be restored the next time you start KoboldCpp again. However, to avoid data loss you are still recommended to manually export your saved story .json files from time to time.

### **Save File Includes Settings**  
This option allows your KoboldAI Lite UI, generation and sampler settings to be saved directly into the story json file itself, and loaded again in future.

### **Show Rename Save File**  
This option triggers a popup when you save your story, allowing you to rename the target save file name.

### **Autoscroll Text**  
This option scrolls down the text window to the bottom every time a new AI response is received.

### **Invert Colors**  
This option inverts all the colors for the UI, useful for e-ink displays or people who prefer a light theme.

### **Chat/Story - Idle Responses**  
Enabling this option allows the AI to automatically send new responses after the player has been idle for a few seconds, useful to simulate a real-time chat conversation.

### **Chat - Multiline Replies**  
This allows the AI to respond to your chat messages with more than a single-line response. This may result in more verbose and lengthy chat responses, but the output can also become wildly incoherent and unpredictable, or the AI might even start talking as someone else. **Not recommended for beginners**.

### **Chat - Continue Bot Replies**  
This option allows the AI to stop speaking halfway (incomplete reply), and then resume speaking within the same message, when you press the submit button again. If disabled, each response from the AI will instead start on a new line with the AI name prefix added (IRC style). Enabling 'Continue Bot Replies' may result in the AI refusing to speak if it does not know what to say. **Not recommended for beginners**.

### **Chat - Your Name / AI Name**  
You can set your displayed name and the AI name for the current chat session, useful for roleplaying specific characters.

### **Instruct - Start and End Sequence**  
Set this to the Instruct start and end instruct sequences that the model was trained on for best quality. For Alpaca, this is `### Instruction: ` and `### Response: `, which should generally work well for most instruct models. You can add newlines with `\n` if desired.

### **Instruct - Enable Markdown**  
This allows instruct mode to generate formatted markdown, such as item lists, tables and code blocks. Useful for coding tasks.

### **Adventure - Adventure Prompt**  
This option injects a pre-prompt to the AI to make it take adventure mode more seriously, useful especially if your prompt is short. **Highly recommended to keep enabled for beginners, unless using a custom scenario.**

### **How do I make the AI remember things?**  
As contexts gets very long, eventually the earlier parts of your story will exceed the maximum context length and get trimmed away. There are some features in the 'Memory' panel to preserve the overall aspets of your story in such scenarios.  
- Memory - This is a sequence of text that will always be injected into the start of each prompt sent to the AI. It is useful for things the AI should always remember even over very long stories, such as main theme(s) of your story, the broad strokes of the setting, central conflict(s), and protagonist. As it uses up context space, try to keep Memory short, at most a paragraph or two.
- Author's Note - This is similar to memory, but is injected *near* the *end* of the prompt rather than at the start. It's used to describe recent situations, or guide the AI to behave in a certin way for the current scene. A/N Strength affects how far back this text is injected.
- World Info - This is text that is only situationally injected into the prompt. When the World Info "Key" is matched, the corresponding "Content" text gets injected into the start of the prompt. Useful for reminding the AI of facts, character names, ages, personalities, places as well as plot points, like a dictionary or encyclopedia.

### **What are stop sequences (stopping tokens)?**  
Stop Sequences are a set of specially designated tokens or phrases that should make the model stop generating early. For example, if you wanted the output to end after a new paragraph, you could use `\n\n` as a stopping sequence. Chat mode, Instruct mode and Adventure mode all come with preconfigured stop sequences.

### **What are the buttons above the user text input box?**  
- Back - This functions like an Undo button, reversing the most recent action or AI response.
- Redo - This is a Redo button, which reverses the 'Back' button and restores deleted text from history.
- Retry - This button retries your most recent action or message, useful if you don't like the AI response and want something different.
- Edit - This is not a button but a checkbox toggle. When enabled, you'll be able to retroactively modify any part of your existing story, or the response from the AI.

### **My chat mode is malfunctioning. How do I stop the AI from replying as myself?**  
This can happen when the model is poorly prompted, especially with 'Multiline Replies' enabled. Often, the solution is just to retry the most recent request. However, here are some tips to avoid this:  
- Disable 'Multiline Replies'
- Use a good model, preferably finetuned on chat conversations
- Make sure the initial prompt or character card is well formatted. Names should be consistent, well-formatted layout wise, and not misspelled. A few good examples in memory goes a long way, if the chat history is bad, the chat future will be bad too.
- In extreme cases, set your chat username as a custom stopping token. This will have unintended side effects.

### **My AI response is very short / the AI response in the console is longer, some words got trimmed from the terminal to the UI.**  
This is the opposite problem to the above, sometimes the AI has many interesting things to say, but they get trimmed away because it responded across multiple lines or even multiple paragraphs. Enabling 'Multiline Replies' allow such responses to be used. Remember - the AI learns from examples. A boring prompt or dull messages from the user can lead to dull AI replies.

### **What is AI Vision?**  
AI Vision is an attempt to provide multimodality by allow the model to recognize and interpret uploaded or generated images. This uses AI Horde or a local A1111 endpoint to perform image interrogation, similar to llava, although not as precise. Click on any image and you can enable it within Lite. This functionality is not provided by KCPP itself.

### **What file formats does KoboldAI Lite support?**  
KoboldAI Lite supports many file formats, automatically determined when the file is loaded. These include:
- KoboldAI Classic .json saves (Default)
- KoboldAI United .json saves (V2 format)
- KoboldAI KAISTORY files
- TavernAI and SillyTavern Character Cards (JSON format, WebP and PNG all supported)
- Oobabooga charaacter and story exports
- Agnai and Tavern world info formats
- Raw text files

### **Where can I find the source code for KoboldAI Lite? What about the online version?**  
The source code for KoboldAI Lite is under AGPLv3, and [can be found here](https://github.com/LostRuins/lite.koboldai.net).
The web version powered by Horde can be accessed at https://lite.koboldai.net

### **Can I run a UI without Javascript, (e.g. from a very old browser) or over the command line (e.g. SSH?)**  
You can use KoboldCpp NoScript WebUI, which does not require Javascript to work. It should be W3C HTML compliant and should run on every browser in the last 20 years, even text-based ones like Lynx (e.g. in the terminal over SSH). It is accessible by default at `/noscript` e.g. `http://localhost:5001/noscript` . This can be helpful when running KoboldCpp from systems which do not support a modern browser with Javascript.

## KoboldCpp Integrations  
### **What is KoboldAI United? How to use KoboldAI Client / Kobold United?**  
[KoboldAI United](https://github.com/henk717/KoboldAI) is the current actively developed version of KoboldAI, while [KoboldAI Client](https://github.com/KoboldAI/KoboldAI-Client) is the classic/legacy (Stable) version of KoboldAI that is no longer actively developed.  
KoboldCpp maintains compatibility with both UIs, that can be accessed via the `AI/Load Model > Online Services > KoboldAI API` menu, and providing the URL generated after launching KoboldCpp.  

### **What is Horde? How do I use it? How do I share my model with Horde?**  
The AI Horde is a crowdsourced distributed cluster of Image generation workers and Text generation workers, where people can share their own processing power to generate images and text for other users. KoboldCpp now comes included with an embedded *lightweight Horde Worker* which allows anyone to share their ggml models with the AI Horde without downloading additional dependences apart from KoboldCpp.
- To use Horde as an end-user, you can go to https://lite.koboldai.net
- To share your own models and compute power over Horde using Koboldcpp:
  - Register for an [AI Horde API key](https://horde.koboldai.net/register).
  - Enable the Horde config from the GUI and fill in all details, or launch with `--hordeconfig` with parameters for `[hordemodelname] [hordegenlength] [hordemaxctx] [hordeapikey] [hordeworkername]`, filling up all 5 will start a Horde worker for you that serves horde requests automatically in the background. 
  - Exclude the last 2 parameters to continue using your own standalone Horde worker (e.g. Haidra Scribe / KAI Horde Bridge).

### **I'm encountering SSL errors with my horde worker**  
You can try `--nocertify` mode which allows you to disable SSL certificate checking on your embedded Horde worker. This can help bypass some SSL certificate errors.

### **How can I use the Kobold API? Is there an API reference? How does the KoboldCpp API differ from the KoboldAI United API?**  
The KoboldAI web API is the interface which downstream applications can communicate with KoboldCpp. The full KoboldCpp API reference can be found at https://lite.koboldai.net/koboldcpp_api as well as within the program by visiting `http://localhost:5001/api`. 
- In general, the most important endpoint is `/api/v1/generate`, which is the endpoint used to send prompts and receive responses from the AI. 
- Other useful endpoints are `/api/v1/model`, `/api/v1/config/max_length` and `/api/v1/config/max_context_length`
- In addition, KoboldCpp also implements a few additional endpoints not found in the original KoboldAI API, these include 
  - `/api/extra/generate/stream` for SSE streaming
  - `/api/extra/version` for version information 
  - `/api/extra/perf` for performance and timing information
  - `/api/extra/abort` to abort an in-progress generation
  - `/api/extra/generate/check` to get the partially completed text for an in-progress generation
  - `/api/extra/tokencount` to tokenize and accurately measure how many tokens any string has.
  - `/api/extra/true_max_context_length` to get the actual ctx length loaded from the launcher.

### **Is there an OpenAI API?**  
Yes, as of v1.45.2, there is now a simple OpenAI compatible completions API, which you can access at `/v1/completions`. You're still recommended to use the Kobold API as it has many more settings.

### **Are my chats private? What is with the Share button?**  
KoboldCpp is capable of running fully locally offline without internet, and does not send your inputs to anywhere else. Generated content using the API is displayed in the terminal console, which is cleared when the application is closed. Likewise, KoboldAI Lite UI will store your content only locally within the browser, it is not sent to any other external server. KoboldCpp and KoboldAI Lite are fully open source with AGPLv3, and you can compile from source or review it on github.  

If you use KoboldCpp with third party integrations or clients, they may have their own privacy considerations. When using Horde, your responses are sent between the volunteer and the user over the horde network and potentially can be read from either end, so do not send privacy sensitive information with Horde.

The "Share" button in KoboldAI Lite does not actually upload any data anywhere, rather it compresses your entire story into a very long URL (which encoded the data within it), that can be reloaded on a different device using the web version of KoboldAI Lite.

---

### Useful Links and References  
[Latest KoboldCpp Release for Windows](https://github.com/LostRuins/koboldcpp/releases/latest)  
[KoboldCpp repo and Readme](https://github.com/LostRuins/koboldcpp)  
[Github Discussion Forum](https://github.com/LostRuins/koboldcpp/discussions) and [Github Issues list](https://github.com/LostRuins/koboldcpp/issues?q=)  

### Other established resources  
[Local LLM guide from /lmg/, with good beginner models](https://rentry.org/local_LLM_guide)  
[SillyTavern documentation regarding KoboldAI](https://docs.sillytavern.app/usage/api-connections/koboldai/)  
[PygmalionAI documentation regarding KoboldAI](https://docs.pygmalion.chat/local-installation-(cpu)/pygcpp/#android)  
[KoboldAI Discord Server](https://koboldai.org/discord)  
Also check out /lmg/, r/KoboldAI and r/LocalLLaMA/  

### Misc. Guides  
[Installing KoboldCpp on Android via Termux](https://www.reddit.com/r/KoboldAI/comments/14uxmsn/guide_how_install_koboldcpp_in_android_via_termux/)  
[Installing KoboldCpp on Linux with GPU](https://www.reddit.com/r/LocalLLaMA/comments/13q6u9e/koboldcpp_linux_with_gpu_guide/)  
[Building KoboldCpp CUDA on Linux](https://www.reddit.com/r/LocalLLaMA/comments/14faz1d/building_koboldcpp_cuda_on_linux/)  
[Simple Windows Guide to getting started with KoboldCpp](https://www.reddit.com/r/singularity/comments/144th3k/incredibly_simple_guide_to_run_language_models/)  
[Simplified LLAMA Guide](https://rentry.org/TESFT-LLaMa#koboldcpp-windows)  
[Compiling on Windows, A quick guide](https://github.com/LostRuins/koboldcpp/issues/664)

### Notable Forks (They may have special features)  
https://github.com/henk717/koboldcpp  
https://github.com/ycros/koboldcpp  
https://github.com/YellowRoseCx/koboldcpp-rocm  
https://github.com/0cc4m/koboldcpp  
https://github.com/SammCheese/koboldcpp  
