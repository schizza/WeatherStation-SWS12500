# WeatherStation SWS12500
   *Connecting Sencor SWS 12500 weather station to Home Assistant*

 ### Easy way to connect this meteorological station to Home Assistant using Node-Red and plotting graphs in Gafana
   link to station: [SWS12500](https://www.sencor.cz/profesionalni-meteorologicka-stanice/sws-12500)
 ***  
   
   ## **Configure station in AP mode**

  First of all you need to configure your station in AP mode to send data to your local HA installation.
* hold WiFi button at the back of station for 6 seconds until AP will flash on display.
* select your station from available AP on you computer
* connect to stations setup page: `http://192.168.1.1` from your browser
* in the third URL section fill in address to your local HA installation

![APScreen](/schizza/timer/README/weatherstationAP.png?raw=true
  
   > URL:         `http://yourlocal.homeassistant.local:1880/endpoint/weatherstation`  
   > Station ID:  not needed. Might be filled in for security reasons  
   > Station Key: not needed. Might be filled in for security reasons

*you can change `/weatherstation` to whatever you want, but you have to change NR configuration appropriately.*  

## **Node-Red flow**  
To receive data from your station, there need to be flow with `http in` function.  

> Method:  get  
> URL: weatherstation (or whatever you named endpoint in station's URL)
    


