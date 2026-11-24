# OpenInverterGatewaySA

Firmware for custom-built stick (based on Wemos D1 mini (ESP8266) module) that goes into the Growatt 1-3000-s series solar inverters.
This firmware is a customized version of the original; no additional functionality has been added, merely the tweaks needed to setup and operate the hardware (LEDs etc).
The hardware lives on <https://boardrepo.com/mafiltenborg/shineassistant>
Also, on the HW repo, you will find a 3D-printed box thing.

## Features

* Graphical UI using a built-in webserver with live visualization. [Screenshots](#screenshot).
* The inverter is queried using Modbus protocol.
* Inverter data is made available via
* The data received will be transmitted using MQTT to a server of your choice.
* The data received is also provided as JSON and Prometheus format
* Firmware update via [WifiManager](https://github.com/tzapu/WiFiManager)
* Basic access to arbitrary modbus data
* Tries to autodetect which stick type to use
* Configuration access point for initial configuration of Wifi and MQTT server ([IP, SSID, Password](#flashing--hardware))
* Supports Growatt protocol versions v1.20, v1.24 and v3.05
* Other Growatt protocol versions can easily be implemented / modified
* TLS support for ESP32
* Debugging via Web and Telnet
* Power limiting (allows zero export)

## Screenshot

<p align="center"><img width="500px" alt="creenshot of the OpenInverterGateway Web UI" class="recess" src="./Doc/Screenshot.png" /></p>

## How to install

### Compile a release yourself

1. Download / clone this repo.
2. Setup [PlatformIO](https://platformio.org/).
3. Open the project folder and choose the correct environment for your hardware by opening i.e.the **Project Tasks > ShineWiFiX** section.
4. Rename the [`Config.h.example`](SRC/ShineWiFi-ModBus/Config.h.example) to `Config.h` and adapt it according to your requirements.
5. Compile using the **Build** task from PlatformIO.
6. Follow the [flashing / hardware section below](#flashing--hardware).

### Flashing / Hardware

1. Flash the image to your hardware. [Details on how to do this are provided in the documentation](/Doc/).
2. A configuration WiFi access point (SSID: `GrowattConfig`, Pass: `growsolar`) will be available.
   Connect to it and [configure the firmware via web interface at http://192.168.4.1](http://192.168.4.1).
3. If you need to reconfigure the stick, you have to either
   * Press the AP button (configured with `ENABLE_AP_BUTTON` in `Config.h`)
   * Reset the stick twice within 2-10 seconds.

## Hardware compatability

* A specific design using Wemos-D1 that communicates via RS232. See <https://boardrepo.com/mafiltenborg/shineassistant> for more info.

Check out the [documentation for more details including pictures](Doc/).

### Particulars

* It does *not* communicate to Growatt Cloud at all.

## Supported Inverters

* Growatt 1000-3000S (protocol 3.05)

## Modbus Protocol Versions

Growatt 1-3000-s inverters apparently use Protocol v3.05 from year 2013.

## HTTP JSON Endpoint

For IoT applications, the raw data can be read in JSON format (`Content-Type: application/json`) by calling `http://<ip>/status`.

## Prometheus Scrape Endpoint

If you want to scrape the metrics with a Prometheus server, you can use the endpoint `http://<ip>/metrics`.
A possible configuration is described [in the documentation](Doc/Prometheus.md).

## [Home Assistant configuration](Doc/MQTT.md)

## [Changelog](CHANGELOG.md)

## Acknowledgements

This project is based on [Jethro Kairys work on the Modbus interface](https://github.com/jkairys/growatt-esp8266).
