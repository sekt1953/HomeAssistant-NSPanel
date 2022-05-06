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

## Nextions Display

* Here the data for the Nextion Editor
  * [000_NextionRunOnes.md](./Nextion/000_NextionRunOnes.md)
  * [001_koktimer.md](./Nextion/001_koktimer.md)
  * [002_screensaver.md](./Nextion/002_screensaver.md)

## ESPHome

* Her is the ESPHome files fron Home Assistant  /config/esphome/
  * [/config/esphome/nspanel-demo.yaml](./ESPHome/nspanel-demo.yaml)
  * [/config/esphome/secrets.yaml](./ESPHome/secrets.yaml)

## Home Assistant

### Configuration:

* [/config/configuration.yaml](./HomeAssistant/configuration.yaml)
* [/config/secrets.yaml](./HomeAssistant/secrets.yaml)

#### Helper
Timer:
* [/config/timer_merge_named/kitchens.yaml](./HomeAssistant/timer_kitchens.yaml)

Input Select:
* [/config/input_select_merge_named/kitchens.yaml](./HomeAssistant/input_select_kitchens.yaml)

Text:
* [/config/input_text_merge_named/kok_display_helper.yaml](./HomeAssistant/kok_display_helper.yaml)

### Automation:

  * [/config//000_nspanel.yaml](./HomeAssistant/000_nspanel.yaml)
  * [/config//001_Create_Input_Select_Option_from_Timers](./HomeAssistant/001_Create_Input_Select_Option_from_Timers.yaml)
  * [/config//Update_demo_panel_TFT_on_file_change.yaml](./HomeAssistant/Update_demo_panel_TFT_on_file_change.yaml)
  
### lovelace:
![My test page layout in Lovelace](./HomeAssistant/Lovelace/LovelaceKitchenTestPage.png)


#### Clock & Timer view:
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
#### Text Helper veiw:
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
#### NSPanel-Diagnostic:
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

#### NSPanel-Sensors
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

#### NSPanel-Sensors:
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

#### Input_select Helper veiw:
```
type: entities
entities:
  - entity: input_select.select_cooking_timer
title: Choose Cooking Timer
state_color: true
```