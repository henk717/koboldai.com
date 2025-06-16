# Kobold Lite Main interface:
Kobold AI and Kobold CPP both host the same Main interface called "Kobold Lite". Due to lack of updates, Kobold AI version of Lite is somewhat outdated from the KoboldCPP version or the HTML page itself.  
The main page can be found here: https://github.com/LostRuins/lite.koboldai.net and can be opened indepently from either KoboldAI or KoboldCPP simply by opening the index.html in your browser.  
Alternatively, you can access the hosted version at https://lite.koboldai.net/.

The primary interface has a few simple buttons:
- ##### AI
  This controls access to remote AI providers. For a comparison of options, please see [Remote Support](API_Providers.md)
- ##### New Session
  New session will allow you to reset most settings to defaults, as well as remove all previously posted content  
  Keep AI selected when unchecked will require you to go to the AI tab after resetting to select a new option.  
  Keep Memory will retain anything set in Context while only resetting the current conversation and similar
- ##### Scenarios
  A few premade scenarios with some settings defined to get you started. These are meant mostly as samples rather than replacing a dedicated character or story site such as CharacterHub.
- ##### Save / Load
  Allows saving to your local browser (client) or if enabled on the server, allows saving to your server.
- ##### Settings
  Please see the full settings breakdown for specifics
- ##### Context
  Context holds all settings about the information provided to the api outside of settings. Things like backstory, info about the world, character details, etc are all in context.
- ##### Quick buttons
  - Back acts like a back button in your browser or phone, undoing the last change (either removing last message, or undoing a retry)
  - Retry will take the last message from the AI and attempt to run it again
  - Redo will take the last message and if from the AI will retry it, if from the user will move it to the user input field
