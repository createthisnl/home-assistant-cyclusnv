[![made-with-python](https://img.shields.io/badge/Made%20with-Python-1f425f.svg)](https://www.python.org/)

# Cyclus NV Sensor Component
This is a Custom Component for Home-Assistant (https://home-assistant.io), it fetches garbage pickup dates for parts of The Netherlands using Cyclus NV's REST API. Based on Cyberjunkys' HVC Groep sensor. Tested for region Krimpenerwaard.


## Installation

### Manual
- Copy directory `custom_components/cyclusnv` to your `<config dir>/custom_components` directory.
- Configure with config below.
- Restart Home-Assistant.

## Usage
To use this component in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry

sensor:
  - platform: cyclusnv
    postcode: 1234AB
    huisnummer: 1
    resources:
      - gft
      - plastic
      - papier
      - restafval
```

Configuration variables:

- **postcode** (*Required*): Your postal code.
- **huisnummer** (*Required*): Your house number.
- **resources** (*Required*): This section tells the component which types of garbage to get pickup dates for.

You can create 2 extra sensors which hold the type of garbage to pickup today and tomorrow:
```
  - platform: template
    sensors:
      afval_vandaag:
        friendly_name: 'Vandaag'
        value_template: >-
          {% if is_state_attr('sensor.cyclusnv_gft', 'day', 'Vandaag') %}
          {% set gft = 'Groene bak' %}
          {% endif %}
          {% if is_state_attr('sensor.cyclusnv_papier', 'day', 'Vandaag') %}
          {% set papier = 'Oud papier' %}
          {% endif %}
          {% if is_state_attr('sensor.cyclusnv_plastic', 'day', 'Vandaag') %}
          {% set plastic = 'Plastic' %}
          {% endif %}
          {% if is_state_attr('sensor.cyclusnv_restafval', 'day', 'Vandaag') %}
          {% set restafval = 'Grijze bak' %}
          {% endif %}
             {{gft}} {{papier}} {{plastic}} {{restafval}}

  - platform: template
    sensors:
      afval_morgen:
        friendly_name: 'Morgen'
        value_template: >-
          {% if is_state_attr('sensor.cyclusnv_gft', 'day', 'Morgen') %}
          {% set gft = 'Groene bak' %}
          {% elif is_state_attr('sensor.cyclusnv_papier', 'day', 'Morgen') %}
          {% set papier = 'Oud papier' %}
          {% if is_state_attr('sensor.cyclusnv_plastic', 'day', 'Morgen') %}
          {% set plastic = 'Plastic' %}
          {% endif %}
          {% if is_state_attr('sensor.cyclusnv_restafval', 'day', 'Morgen') %}
          {% endif %}
          {% set restafval = 'Grijze bak' %}
          {% endif %}
             {{gft}} {{papier}} {{plastic}} {{restafval}}
```

And you can group them like so:
```
Afval Ophaaldagen:
  - sensor.cyclusnv_gft
  - sensor.cyclusnv_papier
  - sensor.cyclusnv_plastic
  - sensor.cyclusnv_restafval
  - sensor.afval_vandaag
  - sensor.afval_morgen
```
Thing to fix/add is multiple pickups per day for 'today' and 'tomorrow' sensor.

## Screenshots

![alt text](https://github.com/createthisnl/home-assistant-cyclusnv/blob/master/screenshots/sensor-example-card.png?raw=true "Screenshot Waste Sensor")

## Donation
Donate Cyberjunky for his awesome work on this component:
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.me/cyberjunkynl/)
