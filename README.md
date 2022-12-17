# WeatherStation SWS12500
   *Connecting Sencor SWS 12500 weather station to Home Assistant*

 ### Easy way to connect this meteorological station to Home Assistant using Node-Red and plotting graphs in Gafana
   link to station: [SWS12500](https://www.sencor.cz/profesionalni-meteorologicka-stanice/sws-12500)
 ***  
   
   ## **Configure station in AP mode**

  First of all you need to configure your station in AP mode to send data to your local HA installation.
* hold WiFi button on the back of station for 6 seconds until AP will flash on display.
* select your station from available AP on your computer
* connect to stations setup page: `http://192.168.1.1` from your browser
* in the third URL section fill in address to your local HA installation

![APScreen](/README/weatherstationAP.png?raw=true)
  
   > **URL:**         `http://yourlocal.homeassistant.local:1880/endpoint/weatherstation`  
   > **Station ID:**  not needed. Might be filled in for security reasons  
   > **Station Key:** not needed. Might be filled in for security reasons

*you can change `/weatherstation` to whatever you want, but you have to change NR configuration appropriately.*  

---
## **Node-Red flow**  
To receive data from your station, there need to be flow with `http in` function.  

> **Method:**  get  
> **URL:** weatherstation (or whatever you named endpoint in station's URL)
    
* Link this function with `http response` with `Status code: 200`  
* Link `http in` with convert funcion.
* Link `convert` function with `MQTT out` node  *(download `NR-flow.json` and import flow to NR)*

![MQTTout](/README/NRflow.png?raw=true)

**MQTT Out node**  
> **Server:** your HA MQTT server  
> **Topic:** weather (or whatever you want to use for topic in MQTT messages)  


---
## **Configure Home Assistant**
In HA we have to create sensors for recieving data from NR  
* add `mqtt:` section to `configuration.yaml` of your Home Assistant *(download `mqtt.yaml` from repository)*
* **restart HA and enjoy collecting data from your weather station :)**  
  
      
---
## **Send meteo data to windy.com**
First of all you need to create station on windy.com: [Windy stations](https://stations.windy.com)

After registering your station to windy.com you will get API key which will be used in NR flow. 
* download `Windy-function.json` and add this function with `http request` to NR
* make sure to update `lon`, `lat`, `elevation`, `tempheight`, `windheight` in `windy function` to match your data
> **lat:** your latitude  
> **lon:** your longitude  
> **elevation:** your elevation  
> **tempheight:** sensor elevation above ground level  
> **windheight:** sensor elevation above ground level   

* connect `Windy function` to `Data reciever` 
![WindyFlow](/README/windy.png) 

* configure `http request` with your API key from Windy Stations
![APIKEY](/README/windyAPIKey.png)

**And you are done. Windy.com will now show you data.**
