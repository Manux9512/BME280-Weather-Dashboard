## Weather Dashboard with BME280 readings and MING Stack.

This is a project working with ESP32-C3 Xiao and a BME280 sensor to get temperature, humidity and pressure data. 

Once we get the readings, we will use a MQTT (MQTTX) broker to publish the data and Node-RED will be a suscriber that will receive the sensor values.

After Node-RED receive the values, data will be stored in InfluxDB because its a real-time data base and work better if we have time-based readings.

Finally if all its okay, we could connect our InfluxDB with Grafana and show our data in real time.
![pipeline](https://github.com/Manux9512/BME280-Weather-Dashboard/assets/105811018/5142feec-feae-468b-974b-78e5c82872f6)


### ESP32 and BME280 code. 

The code was written in C using VScode with IDF plugin framework from espressif. 

-*main* folder contain the **main.c** file, that have all the code to connect BME280 sensor, wifi red, and how to publish data to the topics in a MQTT server .

-*components* folder contains the code for the BME280 sensor. This code is provided by Bosch and you will have 3 files **bme280.h**, **bme280.c** and **bme280_support.c**.
 
### MQTT broker 

The broker used in this proyect was MQTTX. After you download and intalling in your computer you need to setup the connections to obtain the broker address and make the connections in the **main.c** file
![alt text](mqttconfig.png)

With this parameters you can create the connections with the broker with an address in this format:

    ws://broker.emqx.io:1083/mqttgit

The next step is to add the topics that we want to subscribe, this topics should be the same as the setup topics in the main.c file.

Once you setup the connections and flash your device with the code, you should see something like this in mqttx:
![alt text](mqttx.png) 

This are the topics that we created and published data from the BME280.

### Node-Red 
Now that we have running our MQTT broker, we can setup Node-RED as a suscriber and get all the readings from our sensor. Furthermore we will connect to an installed and configurated InfluxDB.
Node-Red works as a workflow tool mainly used for IoT. You will work with modules that are called "nodes" the equivalent of "processores" in Nifi.

![alt text](nodered.png)

The nodes that were used in Node-RED were "mqtt in", "influxdb out" and "debug". The "influxdb out" node is not installed by default you need to install it.

For "mqtt in" you need to setup the following parameters:
![alt text](mqttin.png)

*server*: broker direction

*topic*: topic that you want to suscribe

And for "influxdb out":
![alt text](influxout.png)

*organization*: organization configurated in you influxdb

*bucket*: name of your bucket

*measurement*: a name for the measurement, in this case I used the same as the topics.

*server*: in server you will have some parameter to setup:
![alt text](influxoutconfg.png) 

*name*: a name for you server configuration
*version*: the version of your influxdb

*url*: the direction of you db

*token*: the key that you got once you setup influxdb

If you setup correctly you will see in de debug area how your sensor reading begin to appear.
