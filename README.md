# RPi-MQTT
This project is as a sketch for home automation and control system. Aim is to connect daily household devices (such as air conditioner, lights or curtain motors) and control them over internet. Raspberry Pi fits perfects to control the house indoor electronics and establish the connection between home and web UI server. This is a demo application, however it can easily be applicable in real world scenarios. This project can be divided into 3 main parts:

## 1-) Physical Home Control
Some household devices are controlled via radio frequency. To emulate the remote controller of those devices, Raspberry Pi and an RF transmitter(which has same frequency with the receiver) can be used. First thing is needed to be done is finding the message pattern of remote controller and it can be done using various decoder tools. Then Raspberry Pi emulates that pattern, which "rpi-rf" library is used in this project. By that, Raspberry Pi emulates a pattern to on/off switch and send that message to device using RF transmitter.

## 2-) Server
To carry the control system into internet, MQTT broker is a nice way to work with. Taking the advantage of publish/subscribe architecture and MQTT, user inputs can be conducted to Raspberry Pi to trigger RF send/receive commands. In this project MQTT local broker is used in node-red.

## 3-) User Interface
User interface is responsible to make interaction between application and human. That interface is designed taking the advantage of node-red in this project.

## How it works ?

Raspberry Pi 3/B is used in this project with Raspbian Bullseye OS. Connection between host PC has made via ethernet cable to configure the RPi and install required software and packages. Node-red V 2.2.2, mosquitto server, rpi-rf are installed after configuration. IP address of RPi is 10.42.0.57. By launching node-red and entering this address with :1883 suffix, node-red programming interface opened on PC. I created 2 flows: Switch control UI and CPU Temperature UI. In switch control UI, push bottons are added to run scripts on RPi with various variables such as ("4478259 -p 183 -t 1"). "Injects" and "debugs" are added to simulate the inputs to debug if something is wrong. 6 buttons are added (3 on, 3 off) in UI which are printing the information to debug console and trigger the RPi rpi-rf scripts.

In CPU Temperature UI flow, MQTT broker(local) is used. Topics are: "cpu/temperature/data" and "/check/temperature". "cpu/temperature/data" topic is responsible to publish the temperature data when "/check/temperature" topic is triggered. These triggers are made with "injects" from node-red.
