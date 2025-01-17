# Domoticz Hyundai-Kia plugin
Author: Creasol https://www.creasol.it/domotics

For any support request, please write email to linux@creasol.it or enter the Telegram DomBus group https://t.me/DomBus 


# Introduction

This plugin is designed for [Domoticz](https://www.domoticz.com) home automation system and provide a way to get status from Hyunday and Kia cars.

It's based on the [hyundai-kia-connect-api](https://pypi.org/project/hyundai-kia-connect-api/) python library, written by Fuat Akgun.  

Using this plugin is possible to monitor the battery state of charge (for electric cars), that lead to the ability to enable / disable charging based on the power availability from photovoltaic, for example, improving the own-consumption. 

Also it works with non-EV cars from Hyundai and Kia, monitoring the fuel level and range, climate, location, vehicle sensors, ....

Actually 30 devices will be created, using the same language of Domoticz: only English, Italian and Dutch are supported, now, but if you want to contribute, just fetch the translation.txt file, add for each line the translation in your language, and send by email to linux AT creasol dot it.

This plugin works with **Domoticz Beta** (it may not work with Stable 2022.1 or older versions: please advice) and can be installed typing the following commands from a shell: instead of installing the plugin, (penultimate command),  **it's possible to use Python Plugin Manager or Python Plugins Manager** which also permit to update plugin easily or even automatically:

# Installation

```bash
#become root user
sudo su -

#install git, if not already installed
which git
if [ $? -ne 0 ]; then sudo apt install git; fi

#change to the domoticz directory / plugins
cd /home/pi/domoticz/plugins 

#fetch the Python Plugin Manager (that can be used to install/upgrade other plugins, including domoticz-hyundai-kia)
git clone https://github.com/ycahome/pp-manager

#install python3-dateutil
sudo apt install python3-dateutil

#install hyundai-kia-connect-api lib
pip3 install hyundai-kia-connect-api
#remove __pycache__ dir from the lib (it contains the __pycache__ with sources compiled by a different python version or different CPU)
for d in /usr/local/lib/python3*; do find $d -name __pycache__ -exec rm -r {} \; ; done

#fetch Creasol Plugin
git clone https://github.com/CreasolTech/domoticz-hyundai-kia

#restart Domoticz daemon
service domoticz restart
```

Then, enter Domoticz panel, go to Setup -> Hardware and enable the Hyundai Kia connect plugin specifying the Hyundai or Kia account credential: up to 4 vehicles associated to this account can be shown automatically on Domoticz.

Please note that there are some restrictions on the number of daily access to the cloud... for example EU customers cannot connect more than 200 times/day



# Features
* It's possible to configure 2 polling intervals, to limit accesses to the cloud (Kia Europe limit is 200 accesses/day): a standard interval, a shorter interval when charging or moving (to track battery and position/speed), and a longer interval during the night computed as 4x polling interval, max 120 minutes).

* Permits to monitor all variables provided by the API

* Also, permits to monitor the distance from home and have a map and address of the car location.



# Charging the electric car in a smart way

Creasol is developing a __cheap and smart DIY EVSE module__ that can work stand-alone or connected to Domoticz.

Video at https://www.youtube.com/watch?v=fyDtGO6S1UI

Features:
* detects plug connection and disconnection
* detects when the electric vehicle starts and stops charging
* detects alarms from vehicle
* interfaces a bidirectional energy meter to know the real time import or export power from grid
* operates as __stand-alone__ (no need for a domotic controller) with the possibility to select two charging mode:
    1. __use the maximum power allowed by electricity meter__, preventing overloads and disconnections, without exceeding the power supported by the charging cable
    2. __use only renewable energy (keep import power around 0W)__

* operates in a __controlled mode, with Domoticz__ home automation system: in this case it's possible to 
	1. easily set the __minimum and maximum battery level__
	2. easily set the __maximum charging current__
	3. __when battery level is below minimum, charge at the max power__ permitted by the electricity meter (in Italy, alternates 90 minutes at maximum power + 27% and 90 minutes at maximum power + 10%, ___it's not possible to charge faster!___ The electrical system must be checked carefully when using maximum power, to avoid overheating and fires!!)
	4. __when battery level is between minimum and maximum, charge using only power from renewable energy__ (from photovoltaic) keeping 0W imported from the grid.

More info at https://www.creasol.it/en/support/domotics-home-automation-and-diy/155-smart-electric-vehicle-charging

[![alt Kia car panel, in Domoticz, with some controls to charge the EV car by Creasol DomBusEVSE](https://images.creasol.it/kia_domoticz2.webp "Kia car panel, in Domoticz, with some controls to charge the EV car by Creasol DomBusEVSE")](https://www.creasol.it/domotics)

