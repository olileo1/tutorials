# How to Setup WSL2 for Python Development
Another convenient way to develop Python Code on your Windows Computer is to install the *Windows Subsystem Linux 2* (short **wsl2**) and install anaconda there. Again using Visual Studio Code we can then easily connect to the wsl2 and develop our Python Code.

There are already plenty of tutorials out there on how to install everything so I will just give you the highlevel steps you need to take:

1. install *wsl2* on your computer: To install the *wsl2* on your windows system just follow the instructions from the [Microsoft Documentation](https://learn.microsoft.com/en-gb/windows/wsl/install). On newer computers you basically just need to run the following command from a Powershell or CDM session (for beginners I would recommend to just go with the default, which will currently install you the latest Ubuntu Version):
    ```console
    wsl --install
    ```
2. Setup your Ubuntu System on your *wsl2*. If you have successfully installed the *wsl2* on your computer, you can easily start a terminal session either directly through the [Windows Terminal](https://learn.microsoft.com/en-us/windows/terminal/install)(which is even recommended to install by Microsoft) or through the Powershell using the following command:
    ```console
    wsl
    ```
    If you start the Ubuntu System for the first time you will be guided through the process of creating your first user account. That's basically it

3. Install *Anaconda* on your Ubuntu System. To install *Anaconda*, we basically need to follow the [Offical Anaconda Installation Guide](https://docs.anaconda.com/free/anaconda/install/linux/). The only thing that was difficult for me as a beginner was how downloading the Anaconda installer and successfully executing the installation command. So I will explain that in more details:
  1. In the [installation guide](https://docs.anaconda.com/free/anaconda/install/linux/), they simply state that you need to "download the Anaconda Installer for Linux". This won't work that easily if you are working from your Windows Computer with the *wsl2*. Instead of clicking the Download Link you can rightclick and just *copy the download* link. Then you can execute the following command on your Ubuntu System:
      ```console
      wget https://repo.anaconda.com/archive/Anaconda3-2023.07-2-Linux-x86_64.sh
      ```
      Please note, that the Link can change in the future. The above command will download you the Anaconda installer into your current working directory.
      Now you can execute the *bash* command as stated in the Installation Guide:
      ```console
      bash Anaconda3-2023.07-2-Linux-x86_64.sh
      ```
      Please note, that here I changed the path as well. First our installer is not lying in the directory *~/Downloads* and second the installer Version also changed. From here on you can simply follow the Anaconda Installation Guide
4. Now that you should have your *wsl2* properly setup and equiped with the newest Anaconda Version you can continue by installing the [wsl extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) for VS Code.

Now you should be able to start Visual Studio Code, connect to the wsl2 and start Python Coding.