## Weather Dashboard with BME280 readings and MING Stack.

This is a project working with ESP32-C3 Xiao and a BME280 sensor to get temperature, humidity and pressure data. 

Once we get the readings, we will use a MQTT (MQTTX) broker to publish the data and Node-Red will be a suscriber that will receive the sensor values.

After Node-Red receive the values, data will be stored in InfluxDB because its a real-time data base and work better if we have time-based readings.

Finally if all its okay, we could connect our InfluxDB with Grafana and show our data in real time.
![image](https://github.com/Manux9512/BME280-Weather-Dashboard/assets/105811018/5142feec-feae-468b-974b-78e5c82872f6)


### ESP32 and BME280 code. 

The code was written in C using VScode with IDF plugin framework from espressif. 

-*main* folder contain the **main.c** file, that have all the code to connect BME280 sensor, wifi red, and how to publish data to the topics in a MQTT server .

-*components* folder contains the code for the BME280 sensor. This code is provided by Bosch and you will have 3 files **bme280.h**, **bme280.c** and **bme280_support.c**.
 
