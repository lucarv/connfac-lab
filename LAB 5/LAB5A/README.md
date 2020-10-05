# LAB 5.1: DEVELOP YOUR OWN MODULE

We are currently using the OPC Publisher in supervised mode. That means that if you want to manage the OPC UA Endpoints and tags, you need to manually edit the pn.json file. This is not very friendly and prone to errors.  
Within OPC Foundation, a number of architecture services exist to handle Secure Tag Subscrition. One of these services is called Global Discovery Service (GDS). Microsoft has implement GDS as part of our Industrial IoT Platform. We will talk more about that later, but in the mean time, make sure you start the deployment of the IIot Platform while we do other things (as it is time consuming).  
Follow the instructions found [here](https://azure.github.io/Industrial-IoT/deploy/howto-deploy-all-in-one.html)

We will now create a simple module to manage our published nodes.  

## Create a Private Registry

As we start authoring modules, we need to store the images to a Registry. You can use any docker compatible registry, so let's create a private one on Azure.  
On the Portal, search for Container Registry and add one to your Resource Group. As usual, select a (global) unique name, subscription and location. Leave everything else as is.

## Set up VS Code and tools

Use the IoT extensions for Visual Studio Code to develop IoT Edge modules. These extensions provide project templates, automate the creation of the deployment manifest, and allow you to monitor and manage IoT Edge devices. In this section, you install Visual Studio Code and the IoT extension, then set up your Azure account to manage IoT Hub resources from within Visual Studio Code.

1. Install Visual Studio Code on your development machine.
2. Once the installation is finished, select View > Extensions.
3. Search for Azure IoT Tools, which is actually a collection of extensions that help you interact with IoT Hub and IoT devices, as well as developing IoT Edge modules.
4. Select Install. Each included extension installs individually.
5. When the extensions are done installing, open the command palette by selecting View > Command Palette.
6. In the command palette, search for and select Azure: Sign in. Follow the prompts to sign in to your Azure account.
7. In the command palette again, search for and select Azure IoT Hub: Select IoT Hub. Follow the prompts to select your Azure subscription and IoT hub.
8. Open the explorer section of Visual Studio Code by either selecting the icon in the activity bar on the left, or by selecting View > Explorer.
9. At the bottom of the explorer section, expand the collapsed Azure IoT Hub Devices menu. You should see the devices and IoT Edge devices associated with the IoT hub that you selected through the command palette.

![](images/view-iot-hub-devices.png )

## Create a new project

1. In Visual Studio Code, select View > Integrated Terminal to open the VS Code integrated terminal.
2. In the integrated terminal, enter the following command to install yeoman and the generator for Node.js Azure IoT Edge module:
```
> npm install -g yo generator-azure-iot-edge-module
```
4. Select View > Command Palette to open the VS Code command palette.
5. In the command palette, type and run the command _Azure: Sign in_ and follow the instructions to sign in your Azure account. If you've already signed in, you can skip this step.
6. In the command palette, type and run the command _Azure IoT Edge: New IoT Edge solution_ Follow the prompts in the command palette to create your solution

## Select your target architecture

Currently, Visual Studio Code can develop Node.js modules for Linux AMD64 and Linux ARM32v7 devices. You need to select which architecture you're targeting with each solution, because the container is built and run differently for each architecture type. 

1. Open the command palette and search for _Azure IoT Edge: Set Default Target Platform for Edge Solution_, or select the shortcut icon in the side bar at the bottom of the window.
2. In the command palette, select the target architecture from the list of options. For this tutorial, we're using a raspberry pi as the IoT Edge device, so will use ARM32v7 as our target platform.

## Update the module with custom code

Each template comes with sample code included, which takes simulated sensor data from the tempSensor module and routes it to IoT Hub. In this section, add code to have NodeModule analyze the messages before sending them.

1. In the VS Code explorer, open modules > NodeModule > app.js.
2. Write an app that allows a user to manipulate the pn.json file.
3. You can see a sample of this code at [github](https://github.com/lucarv/pn_manager). 