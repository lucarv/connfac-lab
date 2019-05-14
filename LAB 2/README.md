# LAB 2 - Lambda Architecture
For this lab we will attach a stream processor that will subscribe to the telemetry from IoT Hub. It will observe the event flow.

![](images/lambda.png)

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
10. Click the “Add Stream Input” button, select IoT Hub and setup it up to receive messages from your IoT Hub. Give it a name. select the **asa** consumer group and leave the other default values.
11. Choose "Edit query", and change the input to the alias name you created for the IoT Hub. Enter the following query:
```sql
SELECT 
    *
FROM 
    [<YOUR IOTHUB ALIAS>]
```    
12. Let's observe the stream before we create a query. Right click on the input name and choose "Sample data from input". Accept all parameters, an press "ok".

![](images/observe.png )

13. After about 3 minutes when the stream analytics engine samples the data on the IoT Hub, a little icon appears next to the input name. Press 'Test'. Look at the results.
14. In the previous lab, we had a route from IoT Hub to Blob that we disabled. If you go back to the Portal and re-enable it, You will notice that no messages reach Stream Analytics. One way of still keeping the pattern is to have Stream Analytics do the routing instead So let's do that.
15. Click the “Add Stream Output" button, select Blob Storage and setup it so telemetry is stored in the container we created in Lab 1. 
16. Edit the Query as to select telemetry INTO the container
```sql
SELECT *
INTO
    [<YOUR BLOB ALIAS>]
FROM 
    [<YOUR IOTHUB ALIAS>]
```    
17. Save the work and start the job from trhe overview pane on the Stream Analytics Job page. 
18. Verify that telemetry is again stored in the container.  


### Create an Azure Function App

We will create a Service App that receives telemetry and act upon a condition. We will need to use the Service API to get it

1. GO TO YOUR ioT hub and copy the service connection string
   
   ![](images/servicecs.png )

2. Go back to your Resource group and Create a Function App.
3. Give it a unique name, it is fully addressable from the public internet!!!
4. Select the Subscription, Resource Group and Region. Leave all other settings to default values.
5. Select your newly created app. Expand the menu (Press the "+" next to the menu item "Functions"), select Quickstart and create an In-portal function. Select "more templates" then choose "HTTP Trigger". Give it a name.
6. In the Function App run.csx file replace the default code with the code found [here](https://github.com/lucarv/connfac-lab/blob/master/LAB%202/function/run.csx)
7. Edit the code to enter the service connection string we saved in point 1
8. Under your Function go to View Files, add a file called function.proj
9. Copy the content below to the function.proj file

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

7. Press "Save". The function will download all necessary nuget packages you have declared in the function. It will then compile. 
8. Observe the log at the bottom. When you see the line below you are ready
```
   2019-05-13T20:22:58.681 [Information] Compilation succeeded.
```

### Update your Stream Analytics job

1. First thing we must do is to stop the ongoing job. 
2. Add an Output to your Stream Analytics job to stream data into your Function, the input type should be _Azure function_
3. Give it an alias, select your function accordingly
4. Update your Stream Analytics query so add the statemet below to the existing query
```sql
SELECT 
    [iothub].IoTHub.ConnectionDeviceId as DeviceId
INTO
    [function]
FROM 
    [iothub]
WHERE
    Temperature > 70
```
4. Save your query.
5. Use the direct method we created in Lab 1 to reset the device.
6. Start the Stream Analytics job (wait with next step until it is started)
7. Run the Device Simulator
8. Verify that when “Temperature” reaches 7 0+ degrees, the reset function is invoked on the device and then telemetry restarts at 25 degrees

## Bonus Task

If You and your team has managed to conclude this lab with time to spare, You can try modify the query in stream analytics to add an output to Power BI and create a live dashboard for your solution.


[LAB 3](https://github.com/lucarv/connfac-lab/tree/master/LAB%203)
