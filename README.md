[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg)](https://github.com/custom-components/hacs)  [![made-with-python](https://img.shields.io/badge/Made%20with-Python-1f425f.svg)](https://www.python.org/) [![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.me/cyberjunkynl/)

# Cyclus NV Sensor Component
This is a Custom Component for Home-Assistant (https://home-assistant.io), it fetches garbage pickup dates for parts of The Netherlands using Cyclus NV's REST API. Based on Cyberjunkys' HVCGroep sensor.


## Installation

### HACS - Recommended
- Have [HACS](https://hacs.xyz) installed, this will allow you to easily manage and track updates.
- Search for 'cyclusnv'.
- Click Install below the found integration.
- Configure using the configuration instructions below.
- Restart Home-Assistant.

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
          {% if is_state_attr('sensor.cyclus_nv_gft', 'day', 'Vandaag') %}
          {% set gft = 'Groene Bak' %}
          {% endif %}
          {% if is_state_attr('sensor.cyclus_nv_papier', 'day', 'Vandaag') %}
          {% set papier = 'Blauwe Bak' %}
          {% endif %}
          {% if is_state_attr('sensor.cyclus_nv_plastic', 'day', 'Vandaag') %}
          {% set plastic = 'Plastic' %}
          {% endif %}
          {% if is_state_attr('sensor.cyclus_nv_restafval', 'day', 'Vandaag') %}
          {% set restafval = 'Grijze Bak' %}
          {% endif %}
             {{gft}} {{papier}} {{plastic}} {{restafval}}

  - platform: template
    sensors:
      afval_morgen:
        friendly_name: 'Morgen'
        value_template: >-
          {% if is_state_attr('sensor.cyclus_nv_gft', 'day', 'Morgen') %}
          {% set gft = 'Groene Bak' %}
          {% elif is_state_attr('sensor.cyclus_nv_papier', 'day', 'Morgen') %}
          {% set papier = 'Blauwe Bak' %}
          {% if is_state_attr('sensor.cyclus_nv_plastic', 'day', 'Morgen') %}
          {% set plastic = 'Plastic' %}
          {% endif %}
          {% if is_state_attr('sensor.cyclus_nv_restafval', 'day', 'Morgen') %}
          {% endif %}
          {% set restafval = 'Grijze Bak' %}
          {% endif %}
             {{gft}} {{papier}} {{plastic}} {{restafval}}
```

And you can group them like so:
```
Afval Ophaaldagen:
  - sensor.cyclus_nv_gft
  - sensor.cyclus_nv_papier
  - sensor.cyclus_nv_plastic
  - sensor.cyclus_nv_restafval
  - sensor.afval_vandaag
  - sensor.afval_morgen
```
Thing to fix/add is multiple pickups per day for 'today' and 'tomorrow' sensor.

## Screenshots

![alt text](https://github.com/createthisnl/home-assistant-cyclusnv/blob/master/screenshots/sensor-example-card.png?raw=true "Screenshot Waste Sensor")

## Donation
Donate Cyberjunky for his awesome work on this component:
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.me/cyberjunkynl/)
