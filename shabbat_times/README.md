# Shabbat Times Sensor</br>![Maintenance](https://img.shields.io/maintenance/no/2019.svg)

**NOT MAINTAINED!**

I'm no longer maintaining this custom component!</br>
That doesn't mean I don't use is, It just means this repository will not be maintained.

This custom component can be found in [my home assistant configuration](https://github.com/TomerFi/my_home_assistant_configuration).

You can still use the files :point_up:, if you want.</br>
These :point_down: are the instructions.

**Component Type** : `platform`</br>
**Platform Name** : `shabbat_times`</br>
**Domain Name** : `sensor`</br>
**Component Script** : [`custom_components/sensor/shabbat_times.py`](custom_components/sensor/shabbat_times.py)</br>

[Community Discussion](https://community.home-assistant.io/t/get-shabbat-times-from-hebcal-api-custom-sensor/32429)</br>

#### Component Description
The component works in a similar manner as the *rest* type sensor, it send an api request towards [Hebcal's Shabbat Times API](https://www.hebcal.com/home/197/shabbat-times-rest-api) and retrieves the **next** or **current** Shabbat start and end date and time, and sets them as attributes within a created sensor.</br>
The component can create multiple sensors for multiple cities around the world, the selected city is identified by its geoname which can selected [here](https://github.com/hebcal/dotcom/blob/master/hebcal.com/dist/cities2.txt).

**Table Of Contents**
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
  - [Configuration Keys](#configuration-keys)
- [States](#states)
- [Special Notes](#special-notes)

## Requirements
- **Home Assistant version 0.88.0 or higher**.

## Installation

- Copy files [`custom_components/shabbat_times`](custom_components/shabbat_times) to your `ha_config_dir/custom_components/shabbat_times` directory.
- Configure like instructed in the Configuration section below.
- Restart Home-Assistant.

## Configuration
To use this component in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml

sensor:
  - platform: shabbat_times
    geonames: "IL-Haifa,IL-Rishon LeZion"
    candle_lighting_minutes_before_sunset: 0
    havdalah_minutes_after_sundown: 40
```
This configuration will create two sensors:
- *sensor.shabbat_times_il_haifa*
- *sensor.shabbat_times_il_rishon_lezion*

Each sensor will have its own set of attributes:
- *shabbat_start*
- *shabbat_end*

Which will be calculated based on configuration optional values **candle_lighting_minutes_before_sunset** and **havdalah_minutes_after_sundown**.
These attributes are available for use within templates like so:
- *{{ states.sensor.shabbat_times_il_haifa.attributes.shabbat_start }}* will show the Shabbat start date and time in Haifa.
- *{{ states.sensor.shabbat_times_il_rishon_lezion.attributes.shabbat_end }}* will show the Shabbat end date and time in Rishon Lezion.

### Configuration Keys
- **geonames** (*Required*): A valid geoname selected [here](https://github.com/hebcal/dotcom/blob/master/hebcal.com/dist/cities2.txt), multiple geonames separated by a comma is allowed.
- **candle_lighting_minutes_before_sunset** (*Optional*): Minutes to subtract from the sunset time for calculation of the candle lighting time. (default = 30)
- **havdalah_minutes_after_sundown** (*Optional*): Minutes to add to the sundown time for calculation of the Shabbat end time. (default = 42)
- **scan_interval** (*Optional*): Seconds between updates. (default = 60)

## States
- *Awaiting Update*: the sensor hasn't been updated yet.
- *Working*: the sensor is being updated at this moment.
- *Error...*: the api has encountered an error.
- *Updated*: the sensor has finished updating.

## Special Notes
- The sensors will always show the date and time for the next Shabbat, unless the Shabbat is now, and therefore the sensors will show the current Shabbat date and time.
