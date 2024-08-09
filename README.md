# Huawei Solar EMS
**Energy Management System**

**Huawei Solar EMS package** enrich sensors, services and information provided by [Huawei Solar Integration](https://github.com/wlcrs/huawei_solar), because this custom Home Assistant integration ONLY exposes the information and functions made available by Huawei Solar inverters directly via one of its Modbus interfaces.

## Project Description

This project assist you with a set of custom Home Assistent template sensors also referred to as the *"Huawei Solar EMS package"*. These custom sensors will calculate all the power and energy flows of your Huawei FusionSolar PV installation with Battery (without the corrections we see in FusionSolar). On top of this the provided sensors will calculate expenses with and without solar PV and the Net Return of Investment (NRI) - this will give you a fairly exact idea about the profitability of your investment in several aspects.

This README guides you through a simple installation and setup process. For an overview and a more detailed description of the sensors included in the *"Huawei Solar PEES package"*, please refer to the [Wiki Pages](https://github.com/JensenNick/huawei_solar_pees/wiki). The experienced Home Assistant user may find this guide banal - but this is to include all users, also the ones just starting out.

![Economy Sensors](pictures/economy_sensors.jpg)

> *Economy Sensors. Track your solar PV investment, compare it to what your economy would have been without solar PV, and track the net retur of your investment.*

The provided custom sensors are based on a setup with two inverters and one battery. This is reflected throughout this README and the [Wiki Pages](https://github.com/JensenNick/huawei_solar_pees/wiki). The custom sensors are available as a package for an easy "installation". See more about this in chapter 3. Installation.
