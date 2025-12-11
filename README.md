# Mesh Printer

The Mesh Printer listens on Mashtashtic channel zero and prints any message recieved.

The heart of the Mesh Printer is a Raspberry Pi Zero and a RFM95W LoRa radio module.

Mesh Printer uses a thermal receipt printer. These can come with a USB or Serial interface.

We will use the ESCPos Python library to send text to the printer. You will need to identify your printer and test using the Python library to print text.

Read the instructions for Python-ESCPos and use the code examples to verify you can send text to your printer.

https://python-escpos.readthedocs.io/en/latest/
https://github.com/python-escpos/python-escpos




Use the Raspberry Pi Getting Started guide to install Ubuntu on your Raspberry Pi Zero.

https://www.raspberrypi.com/documentation/computers/getting-started.html

I found the USB Ethernet on the Raspberry Pi to be unreliable so I used a USB to Ethernet adaptor to put the Pi Zero on my home network.


Once your Raspberry Pi zero is running, use the install instructions for Meshtasticd to turn the Pi Zero into a Meshtastic node. The wiring pins to connect the RFM95W to the Pi Zero are included in the RFM95W configuration file for Meshtasticd.

https://meshtastic.org/docs/software/linux/installation/

Setup and test Meshtasticd


Install Python-escpos and meshtastic-cli-receive-text

https://github.com/python-escpos/python-escpos
https://github.com/brad28b/meshtastic-cli-receive-text


Once Python-escpos and meshtastic-cli-receive-text are installed you will have all the required libraries and prerequisites to be able to run the msprinter.py Python script.


Copy the msprinter.py script to the Pi users home.

To run msprinter as a service, use the following systemd startup configuration.

sudo nano /etc/systemd/system/msprinter.service

```
[Unit]
Description=MsPrinter
After=multi-user.target

[Service]
ExecStartPre=/bin/sleep 30
Type=simple
ExecStart=/home/pi/mesh/bin/python3 /home/pi/msprinter.py

[Install]
WantedBy=multi-user.target
```


sudo systemctl daemon-reload
sudo systemctl enable msprinter
sudo systemctl start msprinter
journalctl -u msprinter -b

