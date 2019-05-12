# LAB 1 - Azure IoT Platform
For this lab we will work a lot with IoT Hub. We will explore most of its functions using a device simulator.

## Step-by-step guide
Sign in to <http://portal.azure.com>

### Create/Select Resource group 
On the left pane choose Resource Groups.
1. Click the "+ Add" button to create a new Resource group, or in case a Resource Group has been assigned, choose it.
2. Give the Resource group a name and choose North Europe as region

### Create IoT Hub
(You might need to the refresh icon in Azure to see your new Resource group)
1. Select your Resource group and then click the "+ Add" button to add an IoT hub
2. Search for IoT Hub and then click “Create”
3. Select the subscription, Resource Group and Region
4. Give it a unique name. An IoT Hub is fully addressable from the public internet!!!
5. Click “Next: Size and scale“
6. Select the S1 pricing tier, 1 unit is enough
7. Press “Review + create“
8. Verify the settings and press “Create“
 
Go back to your Resource group and verify that the IoT Hub there

Now we will add a few consumer groups, which we will need later. On the IoT Hub blade, select Built-in endpoints and create two consumer groups, call one asa and another timeseries.  

![](images/consumergrps.png)


### Create Blob storage
1. Select your Resource group and then click the "+ Add" button to add a Storage Account
2. Search for Storage Account and then click “Create”
3. Select the subscription, Resource Group and Region
4. Give it a unique name. A Storage Account is fully addressable from the public internet!!!
5. Choose the asa consumer group we created on the Iot Hub.
6. Leave everything else with default settings
7. Press “Review + create“
8. Verify the settings and press “Create“
   
Go back to your Resource group and vselect the Storage account
1. On the landing blade, Select Blobs
2. Create a Container by Pressing the appropriate Button. Give it a name and select "Container" as Public Access level

### Create Stream Analytics Job

1. Select your Resource group and then click the "+ Add" button to add a Stream Analytics Job
2. Search for Stream Analytics Job and then click “Create”
3. Enter a job name (unique within the resource group)
4. Select the Subscription, Resource Group and Region
5. Choose Cloud as Hosting environment
6. Leave everything else with default settings
7. Press “Create“
8. Go back to your Resource group and select the Stream Analytics job
9. Click Input to create a new input for the Stream Analytics job
10. Click the “Add Stream Input” button, select IoT Hub and setup it up to receive messages from your IoT Hub. You can keep the default settings.
11. Give the Stream Analytics job an output and select Blob storage as the output type. Give it a name and make sure you select the storage account and container you have previously created. Leave the other settings as default.
12. Edit the Stream Analytics Query by change the input the alias name you created for the IoT Hub and the output to your Blob Storage alias
13. Press Save.
14. Start the Stream Analytics job from the Stream Analytics Overview page

### Provision and start sending telemetry from a simulated device
1. Go to IoT Hub and click IoT Devices on the left panel (blade)
2. On the IoT Devices page click **"+ Add"** and give the IoT device a name e.g. **Simulator**
3. After you created a new device click refresh on the IoT Device page until the new device appears.
4. Select the device and copy the Connection string (primary key) you will need this to run the **Device Simulator** app. Follow the [Device simulator guide](https://github.com/lucarv/connfacc-hol/tree/master/Device_Simulator) up to bullet 5.
5. Verify the invocation of a direct method. Select your device in the portal. Verify that the device resets and the temperature starts from 25C again.
![](images/InvokeReset.png)

### Run solution and verify data
Start the solution and verify that the simulator is sending messages to
1. IoT Hub: Look on the IoT Hub Overview page and see “Usage” (Hit refresh and verify that the messages count is increased)
2. Stream Analytics Job: Look on the Stream Analytics Overview page and see “Monitoring” there should be events coming in
3. Under your storage account go to Blobs, select your container, drill down to the lowest level of the folder structure and verify that files were created
4. Stop the Device Simulator (Ctrl+C on terminal)
5. Stop the Stream Analytics job

### Create an Azure Function App

Go back to your Resource group and Create a Function App.
 
1. Give it a unique name, it is fully addressable from the public internet!!!
2. Select the Subscription, Resource Group and Region. Leave all other settings to default values.
3. Select your newly created app. Expand the menu, select Quickstart and create an In-portal function. Select "more templates" then choose "HTTP Trigger". Give it a name.
4. In the Function App run.csx file copy/paste the following code instead of the default code
5. On the run.csx window, replace all code with [this](https://github.com/lucarv/connfacc-hol/tree/master/Device_Simulator)
6. Under your Function go to View Files, add a file called function.proj
7. Copy the content below to the function.proj file

```json
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
    </PropertyGroup>
   
    <ItemGroup>
        <PackageReference Include="Microsoft.Azure.Devices" Version="1.17.3" />
        <PackageReference Include="Microsoft.Azure.Amqp" Version="2.4.2" />
    </ItemGroup>
</Project>
```

7. Restart the Function

### Update your Stream Analytics job

1. Add an Output to your Stream Analytics job to stream data into your Function
2. Give it an alias, select your fucntion accordingly
3. Update your Stream Analytics query so it is like below (define inputs and outputs with aliases you provided)

```sql
SELECT
*
INTO
    [<YOUT BLOB ALIAS>]
FROM
    [<YOUR IOTHUB ALIAS>]

SELECT 
    IoTHub.ConnectionDeviceId DeviceId
INTO
    [functionOutput]
FROM 
    [<YOUR IOTHUB ALIAS>]
WHERE Sensor.Temperature > 30.0
```


Start the Stream Analytics job (wait with next step until it is started)
Run the Device Simulator
Verify that the “Temperature” reaches 30+ degrees, then stops for 10 seconds and then restarts at 25 degrees
