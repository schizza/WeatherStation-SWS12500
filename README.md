# WeatherStation SWS12500
   *Connecting Sencor SWS 12500 weather station to Home Assistant*

 ### Easy way to connect this meteorological station to Home Assistant using Node-Red and plotting graphs in Gafana
   link to station: [SWS12500](https://www.sencor.cz/profesionalni-meteorologicka-stanice/sws-12500)
 ***  
## **Setting up http proxy**
As the stations FW cannot handle url with /path we need a little help with http proxy. You can use your already set proxy server or setup a new one.  
If you are setting new proxy there are two approaches - configuring a new server on your machine or use Home Assistants add-ons.  
  
<br>

## *Setup a new http proxy server using nginx*
Follow installation guide for your system or build version for your system - [nginx download page](https://nginx.org/en/download.html)

```
  sudo apt-get install nginx  
  
  sudo wget -P /etc/nginx/sites-available https://github.com/schizza/WeatherStation-SWS12500/blob/f7be558bbb5945ad6eba2f54a6fd18767f5cc342/weatherstation.nginx.conf 

  sudo ln -s /etc/nginx/sites-available/weatherstation.nginx.conf /ect/nginx/sites-enabled/weatherstation

  sudo systemctl restart nginx
```
<br>
<br>

## *Setup **Nginx Proxy Manager** with Home Assistant's addon*  

1. In Home Assistant go to Addons and install `Nginx Proxy Manager`  
*you need MariaDB installed (in HA addons) to run Proxy Manager correctly*  

2. In `configuration` tab set appropriate ports for you configuration  
![settingProxyManager](/README/settingProxyManager.png?raw=true)
> **HTTP/SSL Entrance port:** you need to set this value form 443 to something else because port 443 will be in use  
> **HTTP Entrance port:** this is your port for communicating with station throught your proxy server  
> **NGinx Proxy Manager Admin web interface:** port of administration web page 

Now you can start your Proxy Manager. First start will take a while - be patient
![ProxyManagerStart](/README/settingProxyManager1.png?raw=true)  

3. While `Nginx Proxy Manager` is running, go to administraion page.
> **First login**  
> *user:* `admin@example.com`  
> *password:* `changeme`
4. Create a new proxy host
![CreateProxyHost](/README/proxyHostAdd.png?raw=true)  
![CreateProxyHost](/README/ProxyHostSettings.png?raw=true)
![CreateProxyHost](/README/ProxyHostSettings1.png?raw=true)  

**Save and you are done with http proxy!**  

<br>  

---
   ## **Configure station in AP mode**

  Now you need to configure your station in AP mode to send data to your local HA installation.
* hold WiFi button on the back of station for 6 seconds until AP will flash on display.
* select your station from available AP on your computer
* connect to stations setup page: `http://192.168.1.1` from your browser
* in the third URL section fill in address to your local HA installation

![APScreen](/README/weatherstationAP.png?raw=true)
  
   > **URL:**         IP address of your http proxy set in HA or server
   > **Station ID:**  not needed. Might be filled in for security reasons  
   > **Station Key:** not needed. Might be filled in for security reasons

*you can change `/weatherstation` to whatever you want, but you have to change NR configuration appropriately.*  

---
## **Node-Red flow**  
To receive data from your station you need to add Node-Red flow
* Download `NR-flow.json` and import it to Node-Red

The flow implements MQTT discovery so the sensor will show in Home assistant automatically under name: sencorsws12500

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
