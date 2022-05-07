# NSP-Kitchen, First Try
![First screeen](/First_Try/Nextion/Image/FirstTry.png)  

First try is all about getting data all the way from Home Assistant via ESPHome to NSPanel, and button to send signal all the way back from NSPanel via ESPHome to Home Assistant.
In my Home Assistant I have created timer to used when I am cooking and baking, I want to bee able to select the rigth timer and start it from my NSPanel, when timer is finish an automation will bee sewnd with Telegram, and that is werry nice I cn bee in garden and still a notification on my phone.

When writing this the I am using this product:
  * Nextion Editor version is: V1.63.3.
  * VirtualBox version is: 5.2.22r126460(Qt595)
  * Linux version is: Ubuntu 18.04.6 LTS
  * Home Assistant running :
    * Core: Version core-2022.4.7 
    * Supervisor: Version supervisor-2022.04.0 
    * Host: Operating System Home Assistant OS 7.6 
    * [Raspberry Pi 4 Model B – 8 GB](https://raspberrypi.dk/produkt/raspberry-pi-4-model-b-8-gb/)
      * In [Argon ONE M.2 Case](https://raspberrypi.dk/produkt/argon-one-m-2-case-til-raspberry-pi-4/)
      * with 500GB M.2 SSD SATA-600

Herunder prøver jeg at beskrive hvad filen "[nspanel-demo.HMI](./Nextion/nspanel_demo.HMI)" to edit .HMI file you have to have this Editor [Nextion Editor](https://nextion.tech/nextion-editor/) it is for now free and it have to run on windows, I run it on win7 in a virtualBox from Oracle so I can have it running an my linix platform. 

## Home Assistant

### Configuration:

* [/config/configuration.yaml](./HomeAssistant/configuration.yaml)
```
# Configure a default setup of Home Assistant (frontend, api, etc)
default_config:

# Text to speech
tts:
  - platform: google_translate

group: !include groups.yaml
automation: !include automations.yaml
script: !include scripts.yaml
scene: !include scenes.yaml

# See this video about splitting:
#  Understanding YAML as it's used in Home Assistant Config files
#  https://www.youtube.com/watch?v=FfjSA2o_0KA

# Result is a LIST!
# Each file is an item in the list.
automation split: !include_dir_list ./automations_dir_list
script split: !include_dir_list ./scripts_dir_list
scene split: !include_dir_list ./scenes_dir_list

# All files merged into one big list (files MUST contain a list).
sensor: !include_dir_merge_list ./sensor_merge_list
binary_sensor: !include_dir_merge_list ./binary_sensor_merge_list

# Result is a DICTIONARY!
# Merge all files into a directory using the filename as the key. 

# Merge contents of all files.
timer: !include_dir_merge_named ./timer_merge_named
input_select: !include_dir_merge_named ./input_select_merge_named
input_text: !include_dir_merge_named ./input_text_merge_named

recorder: 
  db_url: !secret maria_db

homeassistant:
  allowlist_external_dirs:
    - /config/www/tft

folder_watcher:
  - folder: /config/www/tft

# Telegram
telegram_bot:
  - platform: polling
    api_key: !secret telegram_api_token
    allowed_chat_ids:
      - !secret telegram_chat_id_user1
      - !secret telegram_chat_id_group1

notify:
  - platform: telegram
    name: !secret telegram_NOTIFIER_User1
    chat_id: !secret telegram_chat_id_user1

  - platform: telegram
    name: !secret telegram_NOTIFIER_group1
    chat_id: !secret telegram_chat_id_group1
```
* [/config/secrets.yaml](./HomeAssistant/secrets.yaml)
```
# Use this file to store secrets like usernames and passwords.
# Learn more at https://www.home-assistant.io/docs/configuration/secrets/

# MariaDB mysql://user:password@SERVER_IP/DB_NAME?charset=utf8mb4
# Kilde: https://www.home-assistant.io/integrations/recorder/#custom-database-engines
maria_db: mysql://user:password@core-mariadb/database?charset=utf8mb4

# Telegram
telegram_api_token: 14xxxxxxxx:AAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
telegram_chat_id_user1: "83xxxxxxx"        
telegram_chat_id_group1: "-100xxxxxxxxxx"  

telegram_NOTIFIER_User1: "NOTIFIER_user1"
telegram_NOTIFIER_group1: "NOTIFIER_group_1"
```
#### Helper
* Timer:  [/config/timer_merge_named/kitchens.yaml](./HomeAssistant/timer_kitchens.yaml)
```
# Køkken timer
kitchen_001_aeg_blodkogte:
  name: "Æg Blødkogte"
  duration: 600
  icon: mdi:egg-easter

kitchen_002_aeg_hardkogte:
  name: "Æg Hårdkogte"
  duration: 900
  icon: mdi:egg-easter

kitchen_003_rugbrod_haevetid:
  name: "Rugbrød hævetid"
  duration: 5400
  icon: mdi:bread-slice

kitchen_004_rugbrod_bagetid:
  name: "Rugbrød bagetid ved 170°C"
  duration: 5400
  icon: mdi:bread-slice

kitchen_005_fodselsdagsboller_haevetid_1:
  name: "Fødselsdagsboller hævetid 1"
  duration: 1200
  icon: mdi:muffin

kitchen_006_fodselsdagsboller_haevetid_2:
  name: "Fødselsdagsboller hævetid 2"
  duration: 3600
  icon: mdi:muffin
  
kitchen_007_fodselsdagsboller_bagetid:
  name: "Fødselsdagsboller bagetid ved 200°C"
  duration: 720
  icon: mdi:muffin

kitchen_008_grahamsbrod_haevetid:
  name: "Grahamsbrød Hævetid"
  duration: 5400
  icon: mdi:baguette

kitchen_009_grahamsbrod_bagetid:
  name: "Grahamsbrød Bagetid ved 200°C"
  duration: 1500
  icon: mdi:baguette

kitchen_010_morgen_boller_bagetid:
  name: "Morgen Boller Bagetid ved 220°C"
  duration: 1020
  icon: mdi:coffee-outline
```

* Input Select: [/config/input_select_merge_named/kitchens.yaml](./HomeAssistant/input_select_kitchens.yaml)

```
select_cooking_timer:
  name: Cooking Timer
  options: ['vælg en timer']
```
* Text: [/config/input_text_merge_named/nspanel_helper.yaml](./HomeAssistant/kok_display_helper.yaml)
```
kitchen_display_entity_id:
  name: kitchen_display_entity_id

kitchen_display_friendly_name:
  name: kitchen_display_friendly_name

kitchen_display_duration:
  name: kitchen_display_duration

kitchen_display_icon:
  name: kitchen_display_icon
```
### Automation:

* [/config//001_Create_Input_Select_Option_from_Timers](./HomeAssistant/001_Create_Input_Select_Option_from_Timers.yaml)

```
alias: 001 Create Input Select Option from Timers
description: ''
trigger:
  - platform: event
    event_type: call_service
    event_data:
      domain: timer
      service: reload
      service_data: {}
  - platform: homeassistant
    event: start
condition: []
action:
  - service: input_select.set_options
    data:
      options: >
        {{ '[ "' + states | selectattr('entity_id', 'match', 'timer.kitchen_') |
        map(attribute='entity_id') | join('", "') + '" ]' }}  
    target:
      entity_id:
        - input_select.select_cooking_timer
mode: single
```
This piece of code, will create an input_select helper, contains a list of timer entity_id's when either a timer_reload service call is made or Home Assistant is restarted.

* [/config//000_nspanel_timer_navigate.yaml](./HomeAssistant/000_nspanel_timer_navigate.yaml)  

```
alias: 000_nspanel_timer_navigate
description: ''
trigger:
  - type: turned_off
    platform: device
    device_id: 6662312d0ff5820b133f24924f827ddb
    entity_id: binary_sensor.demo_nspanel_prev
    domain: binary_sensor
    id: prev
  - type: turned_off
    platform: device
    device_id: 6662312d0ff5820b133f24924f827ddb
    entity_id: binary_sensor.demo_nspanel_play
    domain: binary_sensor
    id: play
  - type: turned_off
    platform: device
    device_id: 6662312d0ff5820b133f24924f827ddb
    entity_id: binary_sensor.demo_nspanel_next
    domain: binary_sensor
    id: next
condition: []
action:
  - choose:
      - conditions:
          - condition: trigger
            id: prev
        sequence:
          - service: input_select.select_previous
            data: {}
            target:
              entity_id: input_select.select_cooking_timer
      - conditions:
          - condition: trigger
            id: play
        sequence:
          - service: timer.start
            data: {}
            target:
              entity_id: '{{ states(''input_select.select_cooking_timer'') }}'
      - conditions:
          - condition: trigger
            id: next
        sequence:
          - service: input_select.select_next
            data: {}
            target:
              entity_id: input_select.select_cooking_timer
    default: []
  - delay:
      hours: 0
      minutes: 0
      seconds: 15
      milliseconds: 0
  - service: input_select.select_first
    data: {}
    target:
      entity_id: input_select.select_cooking_timer
mode: restart
```
* [/config//stopwatch_change.yaml](./HomeAssistant/stopwatch_change.yaml)

```
alias: Stopwatch Change
description: ''
trigger:
  - platform: state
    entity_id: input_select.select_cooking_timer
condition: []
action:
  - service: input_text.set_value
    data:
      value: '{{ states(''input_select.select_cooking_timer'') }}'
    target:
      entity_id: input_text.kitchen_display_entity_id
  - service: input_text.set_value
    data:
      value: >-
        {% if
        state_attr(states("input_select.select_cooking_timer"),"friendly_name")
        != None %}
          {{ state_attr(states("input_select.select_cooking_timer"),"friendly_name") }}
        {% else %}
          {{ "None" }}.
        {% endif %}
    target:
      entity_id: input_text.kitchen_display_friendly_name
  - service: input_text.set_value
    data:
      value: >-
        {% if state_attr(states("input_select.select_cooking_timer"),'duration')
        != None %}
          {{ state_attr(states("input_select.select_cooking_timer"),'duration') }}
        {% else %}
          {{ "None" }}.
        {% endif %}
    target:
      entity_id: input_text.kitchen_display_duration
  - service: input_text.set_value
    data:
      value: >-
        {% if state_attr(states("input_select.select_cooking_timer"),'icon') !=
        None %}
          {{ state_attr(states("input_select.select_cooking_timer"),'icon') }}
        {% else %}
          {{ "None" }}.
        {% endif %}
    target:
      entity_id: input_text.kitchen_display_icon
mode: restart
```

* [/config//Update_demo_panel_TFT_on_file_change.yaml](./HomeAssistant/Update_demo_panel_TFT_on_file_change.yaml)

```
alias: Update demo panel TFT on file change
description: ''
trigger:
  - platform: event
    event_type: folder_watcher
    event_data:
      event_type: modified
      path: /config/www/tft/
condition: []
action:
  - service: esphome.nspanel_demo_upload_tft
    data: {}
mode: queued
max: 2
```

## Nextions Display

* Here the data for the Nextion Editor
  * [000_NextionRunOnes.md](./Nextion/000_NextionRunOnes.md)
  * [001_koktimer.md](./Nextion/001_koktimer.md)
  * [002_screensaver.md](./Nextion/002_screensaver.md)

## ESPHome

* Her is the ESPHome files fron Home Assistant  /config/esphome/
  * [/config/esphome/nspanel-demo.yaml](./ESPHome/nspanel-demo.yaml)
  * [/config/esphome/secrets.yaml](./ESPHome/secrets.yaml)

  
## lovelace:
![My test page layout in Lovelace](./HomeAssistant/Lovelace/LovelaceKitchenTestPage.png)

### Clock & Timer view:
```
type: grid
cards:
  - type: custom:simple-clock-card
    use_military: true
    hide_seconds: true
  - type: entities
    entities:
      - entity: timer.kitchen_001_aeg_blodkogte
      - entity: timer.kitchen_002_aeg_hardkogte
      - entity: timer.kitchen_003_rugbrod_haevetid
      - entity: timer.kitchen_004_rugbrod_bagetid
      - entity: timer.kitchen_005_fodselsdagsboller_haevetid_1
      - entity: timer.kitchen_006_fodselsdagsboller_haevetid_2
      - entity: timer.kitchen_007_fodselsdagsboller_bagetid
      - entity: timer.kitchen_008_grahamsbrod_haevetid
      - entity: timer.kitchen_009_grahamsbrod_bagetid
      - entity: timer.kitchen_010_morgen_boller_bagetid
    state_color: true
    show_header_toggle: false
columns: 1
square: false
```
### Text Helper veiw:
```
type: grid
cards:
  - type: markdown
    content: >
      ## Data :  

      Entity_ID : {{ states('input_select.select_cooking_timer') }}

      Friendly Name  : {{ state_attr(
      states('input_select.select_cooking_timer') ,'friendly_name') }}

      Duration : {{ state_attr( states('input_select.select_cooking_timer')
      ,'duration') }}

      Icon:  {{ state_attr(states('input_select.select_cooking_timer') ,'icon')
      }} 
    title: Køkken Timer
columns: 1
square: false
```
### NSPanel-Diagnostic:
```
square: false
columns: 1
type: grid
cards:
  - type: entities
    entities:
      - switch.demo_nspanel_relay_1
      - switch.demo_nspanel_relay_2
    title: NSPanel-Controls
    show_header_toggle: true
    state_color: true
  - type: entities
    entities:
      - entity: number.demo_nspanel_brightness
      - entity: button.restart_demo_nspanel
      - entity: button.demo_nspanel_tft_upload
    title: NSPanel-Configuration
    state_color: true
  - type: entities
    entities:
      - sensor.demo_nspanel_wifi_signal
    title: NSPanel-Diagnostic
    state_color: true
```

### NSPanel-Sensors
```
square: false
columns: 1
type: grid
cards:
  - type: entities
    entities:
      - entity: binary_sensor.demo_nspanel_left_button
      - entity: binary_sensor.demo_nspanel_right_button
      - entity: binary_sensor.demo_nspanel_prev
      - entity: binary_sensor.demo_nspanel_play
      - entity: binary_sensor.demo_nspanel_next
      - entity: sensor.demo_nspanel_temperature
      - entity: sensor.pwm_kokken_new_temp
      - entity: weather.home
        secondary_info: none
    title: NSPanel-Sensors
    state_color: true
```

### NSPanel-Sensors:
```
square: false
columns: 1
type: grid
cards:
  - type: entities
    entities:
      - entity: binary_sensor.demo_nspanel_left_button
      - entity: binary_sensor.demo_nspanel_right_button
      - entity: binary_sensor.demo_nspanel_prev
      - entity: binary_sensor.demo_nspanel_play
      - entity: binary_sensor.demo_nspanel_next
      - entity: sensor.demo_nspanel_temperature
      - entity: sensor.pwm_kokken_new_temp
      - entity: weather.home
        secondary_info: none
    title: NSPanel-Sensors
    state_color: true
```

### Input_select Helper veiw:
```
type: entities
entities:
  - entity: input_select.select_cooking_timer
title: Choose Cooking Timer
state_color: true
```