# LAB 4 - Visualizing it
So far we have used very rudimentary tools to look at our data. Let's fix that.

## Time Series Insights
Let's create a Time Series Insights , which is a tool that is very much appreciated by Operations teams in manufacturing.

1. On the Portal, search for Time Series Insights
2. Choose an environment name, your subscription, the resource group we are using and the same location
3. Press "Next: Event Source"
4. Give it a name, leave IoT Hub as source type, select your IoT Hub, and use the iothubowner policy name
5. Choose the consumer group timeseries that we created in LAB 1.

![](images/tsi.png )




1. Once the provisioning is ready, select it and choose "Go to Environment" (top of Portal page)
2. As a default, TSI will display a graph for the event as a counter (not super useful)
3. Let's choose a relevant, input. Choose from the dropdown.
4.  Let's add more inputs. 
5.  Choose from the dropdown
6.  Observe the Time Series graphs and heat maps

