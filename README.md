# Huawei Solar EMS

Home Assistent (HA) **Energy Management System**

[![License][license-shield]](LICENSE) [![GitHub Last Commit][last-commit-shield]][commits] ![GitHub Stars][stars-shield] ![GitHub Watchers][watchers-shield] ![GitHub Forks][forks-shield]

[![Community Forum][forum-shield]][forum]

**Huawei Solar EMS** assist you with **a set of custom** HA **template sensors**, **automations** and **dashboards** also referred to as the *"Huawei Solar EMS package"*.

These custom sensors and automations calculate all the power and energy flows of your Huawei FusionSolar PV installation (including your Batteries, but without the corrections we see in FusionSolar) based on the sensors, services and information provided by [Huawei Solar Integration](https://github.com/wlcrs/huawei_solar), because this custom HA integration ONLY exposes the information and functions made available by Huawei Solar inverters directly via one of its Modbus interfaces.

Additional EMS features:

- **PV Solar Forecasts** by the usage of [Solcast integration](https://github.com/BJReplay/ha-solcast-solar).
  </br>This custom component integrates the [Solcast Hobby PV Forecast API](https://solcast.com/free-rooftop-solar-forecasting) into HA.
- **EPEX Spot electricity price forecasts** by the usage of [EPEXSpot integration](https://github.com/mampfes/ha_epex_spot).
  </br>This custom component adds electricity prices from the European Power EXchange ( [EPEX Spot](https://www.epexspot.com/en/about) ) into HA.
  </br>It covers Austria, Belgium, Denmark, Germany, Finland, France, Luxembourg, the Netherlands, Norway, Poland, Sweden, the United Kingdom and Switzerland at this time.
- **Hourly Weather Forecasts**
- **House load and batteries SOC forecasts** by the usage of the [EMHASS Add-on](https://github.com/davidusb-geek/emhass-add-on).
  </br>[EMHASS](https://github.com/davidusb-geek/emhass) (Energy Management for Home Assistant) is an optimization tool designed for residential households connected to HA.
- **Tibber consumption/cost exports**

## Table of Contents

- [Screenshots](#screenshots)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Inverter polling frequency](#inverter-polling-frequency)
- [FAQ - Troubleshooting](#faq---troubleshooting)

## Project Description

This project assist you with a set of custom Home Assistent template sensors also referred to as the *"Huawei Solar EMS package"*. These custom sensors will calculate all the power and energy flows of your Huawei FusionSolar PV installation with Battery (without the corrections we see in FusionSolar). On top of this the provided sensors will calculate expenses with and without solar PV and the Net Return of Investment (NRI) - this will give you a fairly exact idea about the profitability of your investment in several aspects.

This README guides you through a simple installation and setup process. For an overview and a more detailed description of the sensors included in the *"Huawei Solar PEES package"*, please refer to the [Wiki Pages](https://github.com/JensenNick/huawei_solar_pees/wiki). The experienced Home Assistant user may find this guide banal - but this is to include all users, also the ones just starting out.

![Economy Sensors](pictures/economy_sensors.jpg)

> *Economy Sensors. Track your solar PV investment, compare it to what your economy would have been without solar PV, and track the net retur of your investment.*

The provided custom sensors are based on a setup with two inverters and one battery. This is reflected throughout this README and the [Wiki Pages](https://github.com/JensenNick/huawei_solar_pees/wiki). The custom sensors are available as a package for an easy "installation". See more about this in chapter 3. Installation.

## Screenshots

| **Inverter**                                                           | **Battery**                                                  |
|:----------------------------------------------------------------------:|:------------------------------------------------------------:|
|![Inverter Sensors](images/inverter_sensors.png)                        | ![Battery Sensors](images/battery_sensors.png)               |
|![Inverter Diagnostics](images/inverter_configuration_diagnostics.png)  | ![Battery Configuration](images/battery_configuration.png)   |


|**Power Meter**                                          | **Optimizer**                                       |
|:-------------------------------------------------------:|:---------------------------------------------------:|
|![Power Meter Sensors](images/power_meter_sensors.png)   | ![Optimizer Sensors](images/optimizer_sensors.png)  |


## Prerequisites

This integration supports two connection modes to SUN2000 inverters:
- direct serial connection to the RS485A1 and RS485B1 pins of the COM port 
- network connection

Detailed information can be found on the ['Connecting to the inverter' Wiki-page](https://github.com/wlcrs/huawei_solar/wiki/Connecting-to-the-inverter)

**Services**

[Huawei Solar Integration](https://github.com/wlcrs/huawei_solar/) exposes multiple services, allowing you to [actively control the amount of electricity exported to the grid](https://github.com/wlcrs/huawei_solar/wiki/Changing-Active-Power-Control) and [forcibly charge/discharge your battery](https://github.com/wlcrs/huawei_solar/wiki/Force-charge-discharge-battery). 

To enable these advanced features, you need to select 'Elevate permissions' during the setup of [Huawei Solar Integration](https://github.com/wlcrs/huawei_solar/) otherwise otherwise the majority of EMS features provided within this solution will not work.

6. When using the `elevate permissions` feature in combination with certain connection methods (most TCP-connections, not for serial connections), 
   you will be asked to enter the credentials to the `installer` account in a next step. These are the
credentials used to connect to the inverter in the "Device Commissioning" section of
the FusionSolar App. The default password is `00000a`. If necessary, you can [perform a password reset](https://support.huawei.com/enterprise/en/doc/EDOC1100136173/8aa1f88a/resetting-password). This will not reset other parameters like the FusionSolar cloud connection or other changes made by the firm which did your solar installation.

## Installation

1. Install this integration with HACS, or copy the contents of this
repository into the `custom_components/huawei_solar` directory
2. Restart HA
3. Start the configuration flow:
   - [![Start Config Flow](https://my.home-assistant.io/badges/config_flow_start.svg)](https://my.home-assistant.io/redirect/config_flow_start?domain=huawei_solar)
   - Or: Go to `Configuration` -> `Integrations` and click the `+ Add Integration`. Select `Huawei Solar` from the list

4. Choose whether you want to connect via serial or network connection


![](images/select-connection-type.png)


### Serial configuration

5. Select the "USB to RS485 converter" that you connected to the RS485A1 and RS485B1 pins of your inverter. The Slave ID should be identical to the *Com address* set in the *RS485_1* settings.

![](images/usb-device.png)

### Network configuration

5. Enter the IP address and port on which the Modbus-TCP interface is available. Some pointers:
   - The port is either `502` or `6607`.
   - When connecting to the inverter AP the host IP is typically `192.168.200.1` and the slave id is typically `0`. 
   - When connecting to an SDongle, the slave id is typically `1`. Make sure to give this device a fixed IP!
   
   Checking the `Advanced: elevate permissions` checkbox will:
   - give you access to optimizer data
   - enable you to dynamically change your inverter and battery settings

![](images/network-configuration.png)

### Inverter polling frequency

The integration will poll the inverter for new values every 30 seconds. If you wish to receive fresh inverter data less (or more) frequently, you can disable the automatic refresh in the integration's system options (Enable polling for updates) and create your own automation with your desired polling frequency.

```yaml
- alias: "Huawei Solar inverter data polling"
  trigger:
    - platform: time_pattern
      hours: "*"
      minutes: "*"
      seconds: "/10"
  action:
    - service: homeassistant.update_entity
      target:
        entity_id: sensor.inverter_daily_yield
```

Note that optimizer data is refreshed only every 5 minutes, which matches how frequently the inverter refreshes this data.

## Dashboards

Copy the following folder

- **YAML packages** `<config>/packages`

to your HA `<config>` folder and modify your `<config>/configuration.yaml`:

```
homeassistant:
  customize_glob: !include customize_glob.yaml
  customize: !include customize.yaml
  packages:
    huawei_solar_yield: !include packages/huawei-solar-yield-package.yaml
    huawei_solar_battery_card: !include packages/huawei-solar-battery-card-package.yaml
    huawei_solar_battery_correction: !include packages/huawei-solar-battery-correction-package.yaml
    huawei_solar_diagnostic: !include packages/huawei-solar-diagnostic-package.yaml
    huawei_solar_power_flow_card: !include packages/huawei-solar-power-flow-card-package.yaml
    huawei_solar_energy_flow_card: !include packages/huawei-solar-energy-flow-card-package.yaml
    solcast_solar: !include packages/solcast-solar-package.yaml
```

Copy the following folders

- **YAML dashboards** `<config>/dashboards`<br>
- **Jinja templates used in cards and template sensors** `<config>/custom_templates`<br>
- **Card-Mod theme** (css styles) `<config>/themes/dashboards.yaml`<br>
- **Card background images** `<config>/www/images`<br>

to your HA `<config>` folder and modify your `<config>/configuration.yaml` and `<config>/ui-lovelace.yaml`.

`<config>/configuration.yaml`

```
automation: !include automations.yaml
group: !include groups.yaml
lovelace: !include ui-lovelace.yaml
script: !include scripts.yaml
scene: !include scenes.yaml
frontend:
  themes: !include_dir_merge_named themes
```

`<config>/ui-lovelace.yaml`

```
# UI managed dashboards
#
mode: storage
```

```
# Additional YAML dashboards
#
dashboards:
  100-rooms-yaml:
    mode: yaml
    title: 100_Rooms
    icon: mdi:home
    show_in_sidebar: true
    filename: dashboards/100-rooms/index.yaml
```

![image](https://github.com/heinemannj/home-assistant-config/blob/master/assets/100-tablet-home.png)

```
  420-pv-yaml:
    mode: yaml
    title: 420_PV
    icon: mdi:solar-power-variant
    show_in_sidebar: true
    filename: dashboards/420-pv/index.yaml
```

![image](https://github.com/heinemannj/home-assistant-config/blob/master/assets/421-pv.png)

![image](https://github.com/heinemannj/home-assistant-config/blob/master/assets/422-pv.png)

![image](https://github.com/heinemannj/home-assistant-config/blob/master/assets/423-pv.png)

## Notes

- Private information is stored in secrets.yaml (not uploaded).

---

## FAQ - Troubleshooting

**Q**: Why do I get the error "Connection succeeded, but failed to read from inverter." while setting up this integration?

**A**: While the integration was able to setup the initial connection to the Huawei Inverter, it did not respond to any queries in time. This is either caused by using an invalid slave ID (typically 0 or 1, try both or ask your installer if unsure), or because an other device established a connection with the inverter, causing the integration to lose it's connection

---

**Q**: Will the FusionSolar App still work when using this integration?

**A**: The inverter will still send it's data to the Huawei cloud, and you will still be able to see live statistics from your installation in the FusionSolar App. However, if you are using this integration via the network, and you (or your installer) need to use the 'Device commissioning' feature of the app, you will need to disable this integration.

---

<a name="daily-yield"></a>

**Q**: The "Daily Yield" value reported does not match with the value from FusionSolar?

**A**: The "Daily Yield" reported by the inverter is the *output* yield of the inverter, and not the *input* from your solar panels. It therefore includes the yield from discharging the battery, but misses the yield used to charge the battery. FusionSolar computes the "Yield" by combining the values from "Daily Yield", "Battery Day Charge" and "Battery Day Discharge". [More information on the Wiki ...](https://github.com/wlcrs/huawei_solar/wiki/Daily-Solar-Yield)

---

<a name="debugging"></a>

**Q**: I can't get this integration to work. What am I doing wrong?

**A**: First make sure that ['Modbus TCP' access is enabled in the settings of your inverter](https://forum.huawei.com/enterprise/en/modbus-tcp-guide/thread/789585-100027). Next, check if the port is correct. Some inverters use port 6607 instead of 502 (this can change for you after a firmware update!). If that doesn't work for you, and you intend to write an issue, make sure you have the relevant logs included. For this integration, you can enable all relevant logs by including the following lines in your `configuration.yaml`:

```yaml
logger:
  logs:
    pymodbus: debug # only include this if you're having connectivity issues
    huawei_solar: debug
    homeassistant.components.huawei_solar: debug
```
By providing logs directly when creating the issue, you will likely get help much faster.

[commits-shield]: https://img.shields.io/github/commit-activity/y/heinemannj/huawei_solar_ems.svg
[commits]: https://github.com/heinemannj/huawei_solar_ems/commits/master
[actions-shield]: https://github.com/heinemannj/huawei_solar_ems/workflows/Home%20Assistant%20CI/badge.svg
[actions]: https://github.com/heinemannj/huawei_solar_ems/actions
[contributors]: https://github.com/heinemannj/huawei_solar_ems/graphs/contributors
[discord-shield]: https://img.shields.io/discord/330944238910963714.svg
[discord]: https://discord.gg/c5DvZ4e
[forum-shield]: https://img.shields.io/badge/community-forum-brightgreen.svg
[forum]: https://community.home-assistant.io/?u=heinemannj
[heinemannj]: https://github.com/heinemannj
[travis-shield]: https://travis-ci.org/heinemannj/huawei_solar_ems.svg?branch=master
[travis]: https://travis-ci.org/heinemannj/huawei_solar_ems
[home-assistant]: https://home-assistant.io
[issue]: https://github.com/heinemannj/huawei_solar_ems/issues
[license-shield]: https://img.shields.io/badge/license-MIT-green.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2023.svg
[last-commit-shield]: https://img.shields.io/github/last-commit/heinemannj/huawei_solar_ems.svg
[stars-shield]: https://img.shields.io/github/stars/heinemannj/huawei_solar_ems.svg?style=social&label=Stars
[forks-shield]: https://img.shields.io/github/forks/heinemannj/huawei_solar_ems.svg?style=social&label=Forks
[watchers-shield]: https://img.shields.io/github/watchers/heinemannj/huawei_solar_ems.svg?style=social&label=Watchers
[black-duck-shield]: https://copilot.blackducksoftware.com/github/repos/heinemannj/huawei_solar_ems/branches/master/badge-risk.svg
[black-duck]: https://copilot.blackducksoftware.com/github/repos/heinemannj/huawei_solar_ems/branches/master/
