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
