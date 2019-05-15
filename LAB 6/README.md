# LAB 5 - Visualizing it
So far we have used very rudimentary tools to look at our data. Let's fix that.

## Time Series Insights
Let's create a Time Series Insights , which is a tool that is very much appreciated by Operations teams in manufacturing.

1. On the Portal, search for Time Series Insights
2. Choose an environment name, your subscription, the resource group we are using and the same location
3. Press "Next: Event Source"
4. Give it a name, leave IoT Hub as source type, select your IoT Hub, and use the iothubowner policy name
5. Choose the consumer group timeseries that we created in LAB 1.

![](images/tsi.png )

6. Press "Event Source". Choose the values to add the IoT Hub as input.

![](images/tsi1.png )


7. Once the provisioning is ready, select it and choose "Go to Environment" (top of Portal page)
8. As a default, TSI will display a graph for the event as a counter (not super useful)
9. Let's choose a relevant, input. Choose from the dropdown.
10. Let's add more inputs. 
11. Choose from the dropdown
12. Our payload to IoT Hub is at the moment not formatted, and we can only see one graph on Time Series. We can however add predicates to Time Series to allow us for quickly separate the two values 

![](images/tsi3.png )

