# LAB 2.2: Lambda Architecture Speed and Serving Layer  

For this lab we will attach a stream processor that will subscribe to the telemetry from IoT Hub. It will observe the event flow, we will then push the data into a serving layer

![](images/speed-and-serve.png)

## Create Cosmos DB

1. Select your Resource group and then click the "+ Add" button to add to the serving layer
2. Search for Cosmos DB and then click “Create”
3. Enter a job name (unique within the resource group)
4. Select the Subscription, Resource Group and Region
5. Give an account name
6. Leave everything else with default settings
7. Press “Create“

## Create Stream Analytics Job

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
12. Let's observe the stream before we create a query. 

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
17. Save the work and start the job from the overview pane on the Stream Analytics Job page. 
18. Verify that telemetry is again stored in the container.  


### Create a streaming dataset

Modify the query in stream analytics to add an output to Power BI and create a live dashboard for your solution.
We will do this together, your proctor will guide you through that

[NEXT LAB](https://github.com/lucarv/connfac-lab/tree/master/LAB%203)
