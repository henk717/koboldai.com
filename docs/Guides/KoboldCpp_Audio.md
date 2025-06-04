# Audio
KoboldCPP supports both Text to Speech and Speech to Text.

## Audio Settings
- #### Whisper Model
    - OpenAI when they still were "Open" under the early leadership of Musk and Altman before Musk left released a model to convert Audio to text in several different sizes.
    - https://huggingface.co/koboldcpp/whisper contains usable versions of the smaller models in a single location
    - This expects a GGML .bin file rather than a GGUF file.
- #### OuteTTS Model (Text-to-Speech)
    - Converts audio tokens to speech, can sometimes work with singing and similar as well, but not nearly as good as a model focused on music
    - https://huggingface.co/koboldcpp/tts contains prequanted OuteTTS 1b and 500m
- #### OuteTTS Threads
    - See threads in Hardware section
- #### OuteTTS Max Tokens
    - How many tokens should be generated and convert to speech
- #### WavTokenizer Model (Text-To-Speech)
    - Converts text to audio tokens usable by OuteTTS
- #### TTS Use GPU
    - Use the GPU for this process instead of CPU. Useful for faster audio responses, but takes up space that could be used for a larger text model
