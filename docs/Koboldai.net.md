# [KoboldAI.net](https://koboldai.net)

KoboldAI.net is the online hosted version of KoboldAI Lite that does not require KoboldAI or [Koboldcpp](Koboldcpp.md) to be installed.
Want to use KoboldAI Lite with the AI Horde, OpenAI API or simply want the latest version of Kobold AI Lite? You can use the AI menu to load up your favorite AI provider.

This UI is known to be compatible with the following AI providers (And many more):

- AI Horde

- KoboldAI API / KoboldCpp API

- OpenAI API providers

- Google Gemini API

- OpenRouter API

- Cohere API

- Claudee by Antropic AI

For more information about KoboldAI Lite check the KoboldAI Lite webpage on our site.

### [Click here to go to KoboldAI.net and see it for yourself!](https://koboldai.net)
---
### FAQ
##### Can KoboldAI.net be used as an API?
No, KoboldAI.net runs entirely in your browser and does provide an API or model hosting, if you are asked for a Kobold AI API we recommend you to use [Koboldcpp](Koboldcpp.md).
The default service on KoboldAI.net is AI Horde which does offer its own API available at https://aihorde.net/api but this API is not compatible with programs requiring the KoboldAI API.
For the API's of the other providers please refer to their documentation.

##### Your website requires me to bring my own API key for the provider I wish to use, why can I trust you?
KoboldAI.net runs entirely in your browser and communicates directly to most of the API providers that we provide support for, that means the API key you provide will never be submitted to us.
In some cases a service did not configure their API correctly and we have to use a CORS proxy to provide support, if this is the case we give you a clear warning before you fill out your API key. The CORS proxy server is operated by Lostruins and hosted on Cloudflare, we do not monitor its logs.

##### Where are my stories saved when I use KoboldAI.net?
When you use KoboldAI Lite your stories are saved in your own browser, no data is sent to us. This does mean that you can loose them if you delete your cookies, so always make sure to use the save button to download a json file of your story to keep it safe.

##### How private is my session?
What you send to the AI is as private as the AI Service you send it to. For AI Horde this means that your story will be sent to a random volunteer who may or may not be able to see your story. Because of this we recommend you to avoid any identifiable information in your stories when using this provider. If you need more privacy host your own [KoboldCpp](Koboldcpp.md) instance or use a different AI provider from the list which privacy policy you agree with.

##### I want to use a program, provider or proxy for OpenAI that isn't operated by OpenAI, can I bring my own OpenAI API link?
Yes! You can specify your own link to any OpenAI compatible provider or software. We support both the regular Completions (Recommended) and Chat Completions. You can even use this feature to test KoboldCpp's OpenAI compatibility.

##### I love that all your programs are open source, is KoboldAI.net open source?
Lite.KoboldAI.net is the official domain where we host KoboldAI Lite this is almost the same file as the one we ship with our local products (In the local version we change 1 value that disables the AI menu and prevents the UI from connection to internet API's by default).

You can find the exact source code on https://github.com/lostruins/lite.koboldai.net it is licensed AGPLv3 .

##### What is the difference between KoboldAI.net and KoboldAI Lite?
KoboldAI Lite is the name of the interface that ships across all our products (Including some third party ones), KoboldAI.net is the online version meant to be used with self hosted or third party API links. If you use KoboldAI Lite this does not automatically mean you are using KoboldAI.net, while if you are using lite.koboldai.net you are using KoboldAI Lite.

##### I love a new feature on KoboldAI.net and its not in the KoboldAI Lite that ships with KoboldCpp yet. Can I use KoboldAI.net to connect to my own KoboldCpp for the time being?
Yes! But browsers can be picky when the API link you are connecting to isn't HTTPS. Because of that the easiest way to get it running is by generating a remote link from KoboldCpp even if you connect to your own PC.

##### Can I get banned for using KoboldAI.net?
When you connect to your favorite API provider using KoboldAI.net the same limitations apply as their official website. You will not get banned for using KoboldAI.net or KoboldAI Lite with your favorite provider, but you could be banned when your generations violate their terms of service. Because of this we advice against using the Jailbreak feature with online API's, that feature is designed for local models.

##### Can I save my stories on a Cloud Provider such as Google Drive?
We don't have direct support for this so we advice installing the mobile app or desktop app of your cloud provider and then saving your json in the synchronized folder or app.

#### What is the difference between KoboldAI.net and the AI Horde?
KoboldAI.net is a place where you can use the KoboldAI interface to interact with a variety of AI providers including the AI Horde. This gives you a user friendly way to interact with the various models the volunteers provide.

[AI Horde](https://aihorde.net) itself is the platform that provides access to the models that the volunteers are hosting. It is operated by [Haidra](https://haidra.net) and a continuation of the former KoboldAI Horde platform by the same creator ([db0](https://dbzer0.com)).

[KoboldAI.net](https://koboldai.net) provides one of the interfaces you can use to access this platform, and KoboldCpp is one of the possible backends you can use to volunteer for the platform.