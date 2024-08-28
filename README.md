# doc-rennes-quad

## Description

This repository contains the documentation for the Rennes ion-source quadrupole.

## Installation of Raspberry Pi

### Step 1: Download the Raspberry Pi OS

Download the Raspberry Pi OS from the official website:
[https://www.raspberrypi.org/software/operating-systems/]

### Step 2: Install the Raspberry Pi OS

Follow the instructions on the official website to install the Raspberry Pi OS on the SD card:
[https://www.raspberrypi.org/documentation/installation/installing-images/]

### Step 3: Configure the Raspberry Pi

Insert the SD card into the Raspberry Pi and power it on. Follow the on-screen instructions to configure the Raspberry Pi.

### Step 4: Install and setup the Mosquitto MQTT broker

Use GUI Add/Remove Software to install the Mosquitto MQTT broker.

Write a configuration file for the Mosquitto MQTT broker:

```bash
sudo nano /etc/mosquitto/conf.d/custom.conf
```

Add the following lines to the configuration file:

```bash
listener 1883
allow_anonymous true
log_timestamp_format %Y-%m-%dT%H:%M:%S
```

Restart the Mosquitto MQTT broker:

```bash
sudo systemctl restart mosquitto
```

The Mosquitto MQTT broker is now running on port 1883 and allows anonymous connections. To check the status of the Mosquitto MQTT broker, use the following command:

```bash
sudo systemctl status mosquitto
```

To view the log file of the Mosquitto MQTT broker, use the following command:

```bash
sudo journalctl -u mosquitto
```

### Step 5: Install required python packages

Install the required python packages:

```bash
sudo apt-get install python3-paho-mqtt python3-yaml python3-pyvisa python3-scipy python3-pymeasure
```

### Step 6: Install the janascard-qsource3 package

Download the janascard-qsource3 package from the GitHub repository:

```bash
git clone https://github.com/jurajjasik/janascard-qsource3.git
```

Install the janascard-qsource3 package:

```bash
cd janascard-qsource3
sudo python3 setup.py install
```

### Step 7: Install and setup the QSource3-MQTT service

Download the QSource3-MQTT service from the GitHub repository:

```bash
git clone https://github.com/jurajjasik/qsource3-mqtt
```

Modify the configuration file of the QSource3-MQTT service:

```bash
nano qsource3-mqtt/config.yaml
```

Test the QSource3-MQTT service:

```bash
python qsource3-mqtt/qsource3_mqtt_main.py qsource3-mqtt/config.yaml
```

Inspect the MQTT messages using the Mosquitto MQTT client:

```bash
mosquitto_sub -F '@Y-@m-@dT@H:@M:@S@z : %t : %J' -t qsource3/# -h localhost -p 1883
```

Install the QSource3-MQTT service:

```bash
sudo cp qsource3-mqtt/qsource3-mqtt.service /etc/systemd/system/
sudo chmod 644 /etc/systemd/system/qsource3-mqtt.service
sudo systemctl daemon-reload
sudo systemctl enable qsource3-mqtt
sudo systemctl start qsource3-mqtt
```

The QSource3-MQTT service is now running and will start automatically on boot. To check the status of the QSource3-MQTT service, use the following command:

```bash
sudo systemctl status qsource3-mqtt
```

To view the log file of the QSource3-MQTT service, use the following command:

```bash
sudo journalctl -u qsource3-mqtt
```

### Step 8: Install and setup the QSource3-MQTT-GUI client

Download the QSource3-MQTT-GUI client from the GitHub repository:

```bash
git clone https://github.com/jurajjasik/qsource3-mqtt-gui.git
```

Install the required python packages:

```bash
git clone https://github.com/slightlynybbled/engineering_notation.git
cd engineering_notation
sudo python3 setup.py install
```
