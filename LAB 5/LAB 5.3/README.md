# LAB 5.3: BONUS TRACK

For those who managed to finish 5.1 in time, here is a challenge.
The OPC-UA Server you are currently monitoring is a bad one, it does not send you the OPC:URI. That means that your identity mapping will not work, and there is nothing in the payload that will identify the source OPC UA Server.  
You have given a task to connect a second PLC at:  

```
opc.tcp://lucaopcuaserver.northeurope.cloudapp.azure.com:50000  

with the following node Ids:
    "Id": "nsu=http://opcfoundation.org/UA/;i=2258"
    "Id": "nsu=http://microsoft.com/Opc/OpcPlc/;s=SpikeData"
    "Id": "nsu=http://microsoft.com/Opc/OpcPlc/;s=DipData"
    "Id": "nsu=http://microsoft.com/Opc/OpcPlc/;s=AlternatingBoolean"
    "Id": "nsu=http://microsoft.com/Opc/OpcPlc/;s=RandomUnsignedInt32"
    "Id": "nsu=http://microsoft.com/Opc/OpcPlc/;s=PositiveTrendData"
    "Id": "nsu=http://microsoft.com/Opc/OpcPlc/;s=NegativeTrendData"
```
## Add an Identity tagging Module

The new Server is properly compliant to OPC UA, and will send the URI. As a result, if you connect to it, the OPC URI is sent to the IoT Hub.

```
{
  "NodeId": "nsu=http://microsoft.com/Opc/OpcPlc/;s=RandomUnsignedInt32",
  "ApplicationUri": "urn:OpcPlc:4093458deeba",
  "DisplayName": "ns=2;s=RandomUnsignedInt32",
  "Value": {
    "Value": 750522697,
    "SourceTimestamp": "2019-11-20T20:40:22.4333226Z"
  }
}
```

Your mission, should you accept it, is to add a "ApplicationUri": "urn:OpcPlc:ekskogNet",
to the JSON coming from the first PLC you connected.

