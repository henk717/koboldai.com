# KoboldCpp Colab
### What is Google Colab?
Google Colab is a platform for AI researchers / programmers to get free compute for their AI experiments.

Strengths of Colab:

- Free for a multiple hours per day if GPU's are available.

- Easy to use with the KoboldCpp colab notebook.

- Runs up to 20B models on the free tier.

- UI optimized for python coding experiments.

Weaknesses of Colab:

- Not every AI model is within their terms of service, some actions with the AI are prohibited by their terms of service.

- Colab Pro is expensive and still restrictive.

- Pay as you go tier does not give you anything extra without also having Pro.

- Meant only for AI research and development purposes.

- Terminal access only for Pro users.

- May not be used for AI Horde hosting.

- Sudden disconnects.

- Tab needs to remain open.

- Paid GPU's not as good as the cheaper GPU's from other cloud providers.

### How to use Colab with the KoboldCpp Notebook
1. Open the [KoboldCpp Notebook](https://koboldai.org/colabcpp)
2. On the form that just opened select the model you wish to use or paste your own GGUF download link.
3. Click the play button next to the form.
4. Colab will now install a computer for you to use with KoboldCpp, once its done you will receive all the relevant links both to the KoboldAI Lite UI you can use directly in your browser for model testing, as well as API links you can use to test your development.
5. Keep the colab page open and check for occational captcha's, otherwise they will shut down your machine.

### FAQ

##### Can I use the KoboldCpp notebook with websites requiring the KoboldAI API?

It is possible to use the KoboldCpp Notebook to test websites using the KoboldAI API for free. But keep in mind Google intends this service to be used for AI research, development and testing purposes so we do ask you to not abuse the system and only use it for AI testing / research.

##### Will my data be kept when I close the KoboldCpp notebook?

No, Colab deletes the machine when you are done using the notebook and because the links are randomized you can not easily get your data back from KoboldAI Lite once the page is closed. We recommend you to download your story or use a UI that has local saving.

##### You say Google may randomly shut it down, what happens to my session if that happens?
Your session is stored inside of your browser so you will have time to download your session even if they shut you down. But do not close or refresh the page before doing so as this will make it unavailable.

##### This is a Google service, is it private?
KoboldCpp's Notebook by default does not log any of the prompts you are doing. Colab can realistically see the model you are using and how much you are using it (For prompts they have to scrape memory). We do not expect them to monitor what you submit to the AI.

##### I need an OpenAI API for something I am working on, can I use this notebook?
Yes, one of the links that you will receive once it is fully up and running is an OpenAI API link. This makes it great for your OpenAI based applications.