Xiaomi Air Purifier 3C and Smartmi Evaporative Humidifer 2 Brightness Level

##Setup
```
input_select:
  smartmi_humidifier_led_brightness:
    name: Smartmi Humidifer Led Brightness
    options:
     - 'Off'
     - Dim
     - Bright

  mi_airpurifier_led_brightness_level:
    name: Mi Purifier Brightness Level
    options:
     - 'Off'
     - Dim
     - Bright

automation
  - alias: Mi Air Purifer 3C - Monitor favorite level and update input select
    description: ''
    trigger:
    - platform: state
      entity_id: fan.mi_air_purifier_3c
    action:
    - service: input_select.select_option
      entity_id: input_select.mi_airpurifier_led_brightness_level
      data_template:
        option: >
          {% if is_state_attr('fan.mi_air_purifier_3c', 'led_brightness_level', 0) %} Off
          {% elif is_state_attr('fan.mi_air_purifier_3c', 'led_brightness_level', 3) %} Dim
          {% elif is_state_attr('fan.mi_air_purifier_3c', 'led_brightness_level', 8) %} Bright
          {% endif %}
    mode: single

  - alias: Mi Air Purifier 3C - Select brightness level by input select
    description: ''
    trigger:
    - platform: state
      entity_id: input_select.mi_airpurifier_led_brightness_level
    action:
    - service: xiaomi_miio_airpurifier.fan_set_led_brightness_level
      data_template:
        entity_id: fan.mi_air_purifier_3c
        brightness_level: >
          {% if is_state('input_select.mi_airpurifier_led_brightness_level', 'Off') %} 0
          {% elif is_state('input_select.mi_airpurifier_led_brightness_level', 'Dim') %} 3
          {% elif is_state('input_select.mi_airpurifier_led_brightness_level', 'Bright') %} 8
          {% endif %}
    mode: single

  - alias: Smartmi Humidifier - Monitor led brightness and update input select
    description: ''
    trigger:
    - platform: state
      entity_id: fan.smartmi_evaporative_humidifer_2
    action:
    - service: input_select.select_option
      entity_id: input_select.smartmi_humidifier_led_brightness
      data_template:
        option: >
          {% if is_state_attr('fan.smartmi_evaporative_humidifer_2', 'led_brightness', 0) %} Off
          {% elif is_state_attr('fan.smartmi_evaporative_humidifer_2', 'led_brightness', 1) %} Dim
          {% elif is_state_attr('fan.smartmi_evaporative_humidifer_2', 'led_brightness', 2) %} Bright
          {% endif %}
    mode: single

  - alias: Smartmi Humidifier - Select led brightness by input select
    description: ''
    trigger:
    - platform: state
      entity_id: input_select.smartmi_humidifier_led_brightness
    action:
    - service: xiaomi_miio_airpurifier.fan_set_led_brightness
      data_template:
        entity_id: fan.smartmi_evaporative_humidifer_2
        brightness: >
          {% if is_state('input_select.smartmi_humidifier_led_brightness', 'Off') %} 0
          {% elif is_state('input_select.smartmi_humidifier_led_brightness', 'Dim') %} 1
          {% elif is_state('input_select.smartmi_humidifier_led_brightness', 'Bright') %} 2
          {% endif %} 
    mode: single
```