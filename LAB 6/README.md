# LAB 6 -  Formatting the IoT Hub Payload  

As You notice, the PLC is sending data in 1000 ms interval, and we are sending data to IoT Hub every 5 seconds (as per our pn.json file). That means that we are sending an array of values to IoT Hub. 
```
[
    {
        "NodeId": "ns=1;s=020",
        "DisplayName": "Carico Mobile Testa Linea",
        "Value": {
            "Value": 1.8922288759663677,
            "SourceTimestamp": "2019-05-11T06:39:14.4652272Z"
        }
    },
    {
        "NodeId": "ns=1;s=050",
        "DisplayName": "Avvitatura Supporti Ammortizzatori",
        "Value": {
            "Value": 3.0535855908443765,
            "SourceTimestamp": "2019-05-11T06:39:14.4659883Z"
        }
    },
    {
        "NodeId": "ns=1;s=020",
        "DisplayName": "Carico Mobile Testa Linea",
        "Value": {
            "Value": 1.97165655681878,
            "SourceTimestamp": "2019-05-11T06:39:19.4708524Z"
        }
    },
    {
        "NodeId": "ns=1;s=050",
        "DisplayName": "Avvitatura Supporti Ammortizzatori",
        "Value": {
            "Value": 3.477945840245054,
            "SourceTimestamp": "2019-05-11T06:39:19.4715840Z"
        }
    }
]
```

Your task now is to create a module that looks into the sampling array above and send to IoT Hub the following payload:

```
[
  {
    "NodeId": "ns=1;s=020",
    "Value": <THE AVERAGE OF ALL READINGS FOR THIS TAG IN THIS ARRAY>
  },
  {
    "NodeId": "ns=1;s=050",
    "Value": <THE AVERAGE OF ALL READINGS FOR THIS TAG IN THIS ARRAY>
  }
]
```
This LAB will give you instructions to create a node.js module. You can of course choose any other SDK you are more familiar with. You can find instructions for different languages [here](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-develop-for-linux). Just select your favorite language from bullet point 2.    

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

Use npm to create a Node.js solution template that you can build on top of.

1. In Visual Studio Code, select View > Integrated Terminal to open the VS Code integrated terminal.
2. In the integrated terminal, enter the following command to install yeoman and the generator for Node.js Azure IoT Edge module:
```
> npm install -g yo generator-azure-iot-edge-module
```

4. Select View > Command Palette to open the VS Code command palette.
5. In the command palette, type and run the command _Azure: Sign in_ and follow the instructions to sign in your Azure account. If you've already signed in, you can skip this step.
6. In the command palette, type and run the command _Azure IoT Edge: New IoT Edge solution_ Follow the prompts in the command palette to create your solution
   
## Add your registry credentials

The environment file stores the credentials for your container repository and shares those with the IoT Edge runtime. The runtime needs these credentials to pull your private images onto the IoT Edge device.

1. In the VS Code explorer, open the .env file.
2. Update the fields with the username and password values that you copied from your Azure container registry.
3. Save this file.

## Select your target architecture

Currently, Visual Studio Code can develop Node.js modules for Linux AMD64 and Linux ARM32v7 devices. You need to select which architecture you're targeting with each solution, because the container is built and run differently for each architecture type. 

1. Open the command palette and search for Azure IoT Edge: Set Default Target Platform for Edge Solution, or select the shortcut icon in the side bar at the bottom of the window.
2. In the command palette, select the target architecture from the list of options. For this tutorial, we're using a raspberry pi as the IoT Edge device, so will use ARM32v7 as our target platform.

## Update the module with custom code

Each template comes with sample code included, which takes simulated sensor data from the tempSensor module and routes it to IoT Hub. In this section, add code to have NodeModule analyze the messages before sending them.

1. In the VS Code explorer, open modules > NodeModule > app.js.
2. Replace the entire _PipeMessage_ function with a _FormatMessage_ function.
3. Below is a skeleton for the code You will need to create:
```
// This function filters out messages that report temperatures below the temperature threshold.
// It also adds the MessageType property to the message with the value set to Alert.
function filterMessage(client, inputName, msg) {
    client.complete(msg, printResultFor('Receiving message'));
    if (inputName === 'input1') {
        // the array containing the array with PLC readings is your msg variable
        // let's parse so we can work with it
        var message = msg.getBytes().toString('utf8');
        var messageBody = JSON.parse(message);

        // below is the code you need to write, change the pseudo code with your design
        // start pseudo code
        // FOR EACH VALUE IN THE ARRAY ACCUMULATE THE VALUE ACCORDING TO THE NODE ID
        // AFTER RUNNING THROUGH THE ENTIRE ARRAY, DIVIDE EACH ACCUMULATOR
        // BY THE NUMBER OF VALUES EACH NODE ID HAD
        // CREATE A NEW JSON WITH THE NODE ID AND THE AVERAGES AS SHOWN EARLIER IN THIS LAB
        // CONVERT THE NEW JSON INTO A STRING, CALLED THIS STRING 'formattedMsg' AND YOU ARE DONE
        // end pseudo code

            var outputMsg = new Message(formattedMsg);
            outputMsg.properties.add('MessageType', 'Alert');
            client.sendOutputEvent('output1', outputMsg, printResultFor('Sending received message'));
        }
    }
}
```
4. Replace the function name pipeMessage with formatMessage in client.on() function.
5. Save the app.js file

## Build and push your module  
Now you need to build the solution as a container image and push it to your container registry.

1. Open the VS Code integrated terminal by selecting View > Terminal.
2. Sign in to Docker by entering the following command in the terminal. Sign in with the username, password, and login server from your Azure container registry. You can retrieve these values from the Access keys section of your registry in the Azure portal.
> docker login -u <ACR username> -p <ACR password> <ACR login server>
4. Right click on the Docker.arm32v7 file on the left pane on VS. 
5. Choose build
6. Name your module as <MYACR>.azurecr.io/formatter:0.0.1
7. Push Your module to your registry. The easiest way is to use the Docker extension for visual code. Just open the extension, expand your image, choose the right version, right click and select 'Push'

## Create a new manifest

You can now go back to the portal and add the new module to the deployment manifest.
Remember that modules are not **PUSHED** to the edge device. The Edge device **PULLS** it from the registry, and therefore you must add the registry information to the manifest, as shown in the image below:

![](images/config-registry.png )

