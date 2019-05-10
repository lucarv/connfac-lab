# LAB 3 - Azure Industrial IoT
For this lab we will implement move into opc-ua territory. We will add an OPC-UA publisher to the EDGE device, subscribe to a few nodes and publish the node values to the IoT Hub.

![](images/arch.png )

We will need to create an new deployment manifest that includes the opc-ua publisher. This module is stored on the microsoft reporitory at mcr.microsoft.com/iotedge/opc-publisher:linux-arm32v7
This module needs to read a config file that contains information about the OPC-UA servers and nodes that it should connect too. We will need to store this configuration file on the edge device and subsequently mount it so it is available for the module. Let's do that first.

1. SSH into the raspberry pi
2. Create a folder called iotedge with a file called pn.json. Run the following commands:
```
> mkdir iotedge  
> cd iotedge  
> nano pn.json
```
3. Insert the following lines in the pn.json file
```
[
  {
    "EndpointUrl": "opc.tcp://ekskog.net:4334/UA/HBVUattic",
    "UseSecurity": false,
    "OpcNodes": [
      {
        "Id": "ns=1;s=020",
        "OpcSamplingInterval": 2000,
        "OpcPublishingInterval": 5000,
        "DisplayName": "Current time"
      },
      {
        "Id": "ns=1;s=050",
        "OpcSamplingInterval": 2000,
        "OpcPublishingInterval": 5000,
        "DisplayName": "Current time"
      }
    ]
  }
]
```