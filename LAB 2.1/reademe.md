# LAB 2.1: Lambda Architecture Batch Layer  

## Create Blob storage

1. Select your Resource group and then click the "+ Add" button to add a Storage Account
2. Search for Storage Account and then click “Create”
3. Select the subscription, Resource Group and Region
4. Give it a unique name. A Storage Account is fully addressable from the public internet!!!
6. Leave everything else with default settings
7. Press “Review + create“
8. Verify the settings and press “Create“
9. When the resource is created, go to it

![](images/storage-complete.png)

10. On the landing blade, Select Blobs
11. Create a Container by Pressing the appropriate Button. Give it a name and select "Container" as Public Access level

![](images/create-container.png)

### Create a route to storage  

IoT Hub works by declarative routing, and so far we have used the default route. We will now create a new route to send telemetry from sensors to the cold storage.

1. Go back to your IoT Hub
2. Select "Message Routing". Choose the "Custom endpoints" tab.
3. Create a new Custom endpoint. Press "+ Add" and choose Blob Storage from the drop down menu. Give the end point a name and pick the container we have created. Select "Create"
4. Enter the Routes tab and press "+ Add". Give your route a name, select your blob as endpoint. On the query, let's route only if a device is of type simulated. This info is sent by the device simulator as a header (in the sample code). Write:  

```  
deviceType = 'test'  
```

on the Routing query field. Press "Save".
5.  Start the telemetry again by setting the status to true on the twin document
6. Once the route is activated, you may notice that the telemetry is no longer showing. Can You think of the reason for that?
7. Wait a little and verify that the blob has now telemetry stored.

One drawback from using message routing in IoT Hub is that once a rule is matched, the telemetry is removed from the queue and can't be consumed by other applications. There are ways around this but for now let's just disable the route on the Portal.  
