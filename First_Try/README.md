# NSP-Kitchen, First Try
My work with Nextion Display,  ESPHome and Home Assistant, for this i use Sonoff's NSPanel EU-version

## Nextions Display
First try is all about getting data all the way from Home Assistant via ESPHome to NSPanel, and button to send signal all the way back from NSPanel via ESPHome to Home Assistant.
In my Home Assistant I have created timer to used when I am cooking and baking, I want to bee able to select the rigth timer and start it from my NSPanel


* Here the data for the Nextion Editor
  * [000_NextionRunOnes.md](./Nextion/000_NextionRunOnes.md)
  * [001_koktimer.md](./Nextion/001_koktimer.md)
  * [002_screensaver.md](./Nextion/002_screensaver.md)

## ESPHome

* Her is the ESPHome files fron Home Assistant  /config/esphome/
  * [nspanel-demo.yaml](./ESPHome/nspanel-demo.yaml)
  * [secrets.yaml](./ESPHome/secrets.yaml)

## Home Assistant

* Here are the file used for this project in Home Assistant
  * [/config/configuration.yaml](./HomeAssistant/configuration.yaml)
  * [/config/timer_merge_named/kitchens.yaml](./HomeAssistant/timer_kitchens.yaml)
  * [/config/input_select_merge_named/kitchens.yaml](./HomeAssistant/input_select_kitchens.yaml)
  * [/config//000_nspanel.yaml](./HomeAssistant/000_nspanel.yaml)
  * [/config//001_Create_Input_Select_Option_from_Timers](./HomeAssistant/001_Create_Input_Select_Option_from_Timers)
  
