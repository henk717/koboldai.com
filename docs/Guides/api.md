# KoboldCpp API
For swagger.json, click here or use online version.

## Version: 2025.06.03

### /api/v1/config/max_context_length

#### GET
##### Summary:

Retrieve the current max context length setting value that public backends see

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/v1/config/max_length

#### GET
##### Summary:

Retrieve the current max length (amount to generate) setting value

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/v1/generate

#### POST
##### Summary:

Generate text with a specified prompt

##### Description:

Generates text given a prompt and generation settings.

Unspecified values are set to defaults.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |
| 503 | Server is busy |

### /api/v1/info/version

#### GET
##### Summary:

Current KoboldAI United API version

##### Description:

Returns the matching *KoboldAI* (United) version of the API that you are currently using. This is not the same as the KoboldCpp API version - this is used to feature match against KoboldAI United.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/v1/model

#### GET
##### Summary:

Retrieve the current model string.

##### Description:

Gets the current model display name.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/true_max_context_length

#### GET
##### Summary:

Retrieve the actual max context length setting value set from the launcher

##### Description:

Retrieve the actual max context length setting value set from the launcher

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/version

#### GET
##### Summary:

Retrieve the KoboldCpp backend version

##### Description:

Retrieve the KoboldCpp backend version

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/preloadstory

#### GET
##### Summary:

Retrieves the KoboldCpp preloaded story

##### Description:

Retrieves the KoboldCpp preloaded story, --preloadstory configures a prepared story json save file to be hosted on the server, which frontends (such as KoboldAI Lite) can access over the API.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/perf

#### GET
##### Summary:

Retrieve the KoboldCpp recent performance information

##### Description:

Retrieve the KoboldCpp recent performance information

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/multiplayer/status

#### POST
##### Summary:

Fetches the current multiplayer turn information.

##### Description:

Fetches the current multiplayer turn information. Only useful for Multiplayer sessions in KoboldAI Lite.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/multiplayer/getstory

#### POST
##### Summary:

Fetches the current multiplayer story data, LZMA compressed encoded base64

##### Description:

Fetches the current multiplayer story data, LZMA compressed encoded base64. Data is usually in the same format is KAI Lite compressed savefiles.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/multiplayer/setstory

#### POST
##### Summary:

Sets the current multiplayer story and increments the turn.

##### Description:

Sets the current multiplayer story and increments the turn.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/generate/stream

#### POST
##### Summary:

Generate text with a specified prompt. SSE streamed results.

##### Description:

Generates text given a prompt and generation settings, with SSE streaming.

Unspecified values are set to defaults.

SSE streaming establishes a persistent connection, returning ongoing process in the form of message events.

``` 
event: message
data: {data}

```

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |
| 503 | Server is busy |

### /api/extra/generate/check

#### GET
##### Summary:

Poll the incomplete results of the currently ongoing text generation.

##### Description:

Poll the incomplete results of the currently ongoing text generation. Will not work when multiple requests are in queue.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

#### POST
##### Summary:

Poll the incomplete results of the currently ongoing text generation. Supports multiuser mode.

##### Description:

Poll the incomplete results of the currently ongoing text generation. A unique genkey previously submitted allows polling even in multiuser mode.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/last_logprobs

#### POST
##### Summary:

Obtains the token logprobs of the most recent request.

##### Description:

Obtains the token logprobs of the most recent request. A unique genkey previously submitted is required in multiuser mode.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/tokencount

#### POST
##### Summary:

Counts the number of tokens in a string.

##### Description:

Counts the number of tokens in a string, and returns their token IDs. Also aliased to /api/extra/tokenize

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/detokenize

#### POST
##### Summary:

Converts an array of token IDs into a string.

##### Description:

Converts an array of token IDs into a string.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/abort

#### POST
##### Summary:

Aborts the currently ongoing text generation.

##### Description:

Aborts the currently ongoing text generation. Does not work when multiple requests are in queue.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/transcribe

#### POST
##### Summary:

Uses Whisper to perform a Speech-To-Text transcription.

##### Description:

Uses Whisper to perform a Speech-To-Text transcription.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/websearch

#### POST
##### Summary:

Searches the web using DuckDuckGo and returns the top 3 results.

##### Description:

Searches the web using DuckDuckGo and returns the top 3 results.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/tts

#### POST
##### Summary:

Creates text-to-speech audio from input text.

##### Description:

Creates text-to-speech audio from input text.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/embeddings

#### POST
##### Summary:

Creates an embedding vector representing the input text. Please refer to OpenAI documentation

##### Description:

Creates an embedding vector representing the input text.

This is an OpenAI compatibility endpoint.

 Please refer to OpenAI documentation at [https://platform.openai.com/docs/api-reference/embeddings/create](https://platform.openai.com/docs/api-reference/embeddings/create)

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/json_to_grammar

#### POST
##### Summary:

Converts a provided JSON schema into GBNF grammar.

##### Description:

Converts a provided JSON schema into GBNF grammar. Example schema at [https://platform.openai.com/docs/guides/structured-outputs](https://platform.openai.com/docs/guides/structured-outputs)

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/data/save

#### POST
##### Summary:

Saves data to a slot in a database file in the KoboldCpp server.

##### Description:

Saves data to a slot in a database file in the KoboldCpp server.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/data/load

#### POST
##### Summary:

Loads data from a save slot in the database file in the KoboldCpp server.

##### Description:

Loads data from a save slot in the database file in the KoboldCpp server.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/data/list

#### POST
##### Summary:

List available saved slots from the KoboldCpp server.

##### Description:

List available saved slots from the KoboldCpp server, returns an array of strings containing their titles.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/admin/list_options

#### GET
##### Summary:

List available .kcpps files to load.

##### Description:

List available .kcpps files to load.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/admin/reload_config

#### POST
##### Summary:

Switches the currently loaded .kcpps config, and reloads any changed files or models.

##### Description:

Switches the loaded config, along with any settings and model file changes.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/admin/check_state

#### POST
##### Summary:

Gets the number of bytes taken for existing save state, and predicts the bytes required for a new save state.

##### Description:

Gets the number of bytes taken for existing save state, and predicts the bytes required for a new save state.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/admin/save_state

#### POST
##### Summary:

Creates a new KV cache save state in memory. Overwrites any existing saved state.

##### Description:

Creates a new KV cache save state in memory. Overwrites any existing saved state.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/admin/load_state

#### POST
##### Summary:

Reloads a previous KV cache save state into context.

##### Description:

Reloads a previous KV cache save state into context.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/admin/clear_state

#### POST
##### Summary:

Frees all previous KV cache save state.

##### Description:

Frees all previous KV cache save state.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /api/extra/shutdown

#### POST
##### Summary:

Shuts down the current KoboldCpp server.

##### Description:

Shuts down the server and exits koboldcpp. Only usable from localhost! Both old and new KoboldCpp Server must have been launched with the --singleinstance flag for this to work.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /props

#### GET
##### Summary:

Returns the Jinja template stored in the GGUF model, if found.

##### Description:

Returns the Jinja template stored in the GGUF model, if found.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /.well-known/serviceinfo

#### GET
##### Summary:

Retrieve the common API identity provider

##### Description:

Retrieve the common API identity provider

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /sdapi/v1/sd-models

#### GET
##### Summary:

Gets a list of image generation models

##### Description:

Gets a list of image generation models. For koboldcpp, only one model will be returned. If no model is loaded, the list is empty.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /sdapi/v1/options

#### GET
##### Summary:

Gets configuration info for image generation

##### Description:

Gets configuration info for image generation, used to get loaded model name in A1111.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /sdapi/v1/samplers

#### GET
##### Summary:

Gets a list of supported samplers

##### Description:

Gets a list of supported samplers.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /sdapi/v1/txt2img

#### POST
##### Summary:

Generates an image from a text prompt

##### Description:

Generates an image from a text prompt, and returns a base64 encoded png.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /sdapi/v1/img2img

#### POST
##### Summary:

Transforms an existing image into a new image

##### Description:

Transforms an existing image into a new image, guided by a text prompt, and returns a base64 encoded png.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /sdapi/v1/interrogate

#### POST
##### Summary:

Generates a short text caption describing an image

##### Description:

Generates a short text caption describing an image.

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Successful request |

### /v1/completions

#### POST
##### Summary:

Generates text continuations given a prompt. Please refer to OpenAI documentation

##### Description:

Generates text continuations given a prompt.

This is an OpenAI compatibility endpoint.

 Please refer to OpenAI documentation at [https://platform.openai.com/docs/api-reference/completions](https://platform.openai.com/docs/api-reference/completions). All KoboldCpp samplers are supported, please refer to /api/v1/generate for more details.

##### Responses

| Code | Description |
| ---- | ----------- |
| default |  |

### /v1/chat/completions

#### POST
##### Summary:

Generates a response from a list of messages. Please refer to OpenAI documentation

##### Description:

Given a list of messages comprising a conversation, the model will return a response.

 This is an OpenAI compatibility endpoint.

 Please refer to OpenAI documentation at [https://platform.openai.com/docs/api-reference/chat](https://platform.openai.com/docs/api-reference/chat). All KoboldCpp samplers are supported, please refer to /api/v1/generate for more details.

##### Responses

| Code | Description |
| ---- | ----------- |
| default |  |

### /v1/models

#### GET
##### Summary:

List and describe the various models available in the API. Please refer to OpenAI documentation

##### Description:

List and describe the various models available in the API.

 This is an OpenAI compatibility endpoint.

 Please refer to OpenAI documentation at [https://platform.openai.com/docs/api-reference/models](https://platform.openai.com/docs/api-reference/models)

##### Responses

| Code | Description |
| ---- | ----------- |
| default |  |

### /v1/audio/transcriptions

#### POST
##### Summary:

Transcribes a wav file with speech to text using loaded Whisper model. Please refer to OpenAI documentation

##### Description:

Transcribes a wav file with speech to text using loaded Whisper model.

 This is an OpenAI compatibility endpoint.

 Please refer to OpenAI documentation at [https://platform.openai.com/docs/api-reference/audio/createTranscription](https://platform.openai.com/docs/api-reference/audio/createTranscription)

##### Responses

| Code | Description |
| ---- | ----------- |
| default |  |

### /v1/audio/speech

#### POST
##### Summary:

Generates Text-To-Speech audio from input text. Please refer to OpenAI documentation

##### Description:

Generates Text-To-Speech audio from input text.

 This is an OpenAI compatibility endpoint.

 Please refer to OpenAI documentation at [https://platform.openai.com/docs/api-reference/audio/createSpeech](https://platform.openai.com/docs/api-reference/audio/createSpeech)

##### Responses

| Code | Description |
| ---- | ----------- |
| default |  |

### /v1/embeddings

#### POST
##### Summary:

Creates an embedding vector representing the input text. Please refer to OpenAI documentation

##### Description:

Creates an embedding vector representing the input text.

This is an OpenAI compatibility endpoint.

 Please refer to OpenAI documentation at [https://platform.openai.com/docs/api-reference/embeddings/create](https://platform.openai.com/docs/api-reference/embeddings/create)

##### Responses

| Code | Description |
| ---- | ----------- |
| default |  |
