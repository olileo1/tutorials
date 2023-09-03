# Setup Azure ML
Setting up Azure ML in the Microsoft Azure Cloud is really easy. For me it went so quickly and smoothless I even forgot to take notes. So, I am sorry that I can not provide you with a detailed how to, but I will tell you the main steps you have to do:

1. Create a Microsoft Azure account. For that simply go to the [Azure Homepage](https://azure.microsoft.com/en-us/) and get started. I think Microsoft even gives you a couple hundred dollars of free credits to use on their services. If you are only planning on using Azure for these tutorials, the monthly costs should be far below 10€/month.
2. Once you have created a Microsoft Azure Account you can go to the [Azure Portal](portal.azure.com) and select the *Azure Machine Learning*-Service
3. Create an *Azure Machine Learning* - Workspace. Using Azure Machine Learning, as a private person, you only need to pay for the compute ressources you use. For these tutorials it is more than sufficient to select the cheapest Compute Instance, which costs you around 0.07€/hour.
4. Launch the *Azure Machine Learning Studio* and start by creating your first *Compute* - Instance.

## First steps with your Azure ML Compute Instance
If you just click through the wizard without changing the default parameters during the process of creating your Compute Instance, you will end up with Compute Instance, with the following properties:

* Linux Ubuntu OS
* Python installed
* Anaconda installed, with a few default conda environments
    - the environment provided in this repository mimicks the python sdkv2 environment

You can start your compute instance directly via the web interface. Once your compute instance has started, you can open a new terminal session and clone this repository. Once the repository is clone you can go through the Notebooks provided in the following ways:
* via the Azure ML *Notebooks* - Web Interface (does not require a local VS Code Installation)
* via the VS Code Web-Interface provided by Azure ML (does not require a local VS Code Installation)
* via the VS Code Desktop-Interface. For that you need VS Code installed on your devices togehter with the [Azure Extensions](https://code.visualstudio.com/docs/azure/extensions)