# Home Assistant, NSPanel & Nextions Editor
My work with Nextion Display,  ESPHome and Home Assistant, for this i use Sonoff's NSPanel EU-version

## Kilder:
* Youtube:
  * [Christopher Masto's](https://www.youtube.com/channel/UCwjMKIo9bpe8Nf5j8U3YIrw)
    * [Flashing the Sonoff NSPanel with ESPHome - how to get a custom UI for Home Assistant!](https://www.youtube.com/watch?v=Kdf6W_Ied4o)
    * [How to make a Sonoff NSPanel turn off the screen when you're not using it  ](https://www.youtube.com/watch?v=zndPIPLRjb8)
    * [Sonoff NSPanel Updates: dimming, sleep, and touch calibration, plus show and tell!](https://www.youtube.com/watch?v=aNVGZDUKtuI)
  * [DeanoXX](https://github.com/DeanoXX)
    * [ESPHome Configs](https://github.com/DeanoXX/esphome-config)
  * [Cables & Coffee](https://www.youtube.com/channel/UC8gX8L4G-Dki4AJpy4gijwA)
    * [SONOFF NSPanel Custom ESPHome example. Control entire apartment + dynamic events](https://www.youtube.com/watch?v=g8OUxJEw2dU)
    * [Sonoff NSPanel ESPHome Nextion Editor Tutorial Part 1: Inspiration](https://www.youtube.com/watch?v=mqHWsJLbplA)
    * [Sonoff NSPanel ESPHome Nextion Editor Tutorial Part 2: Nextion Editor, Understanding, Flashing](https://www.youtube.com/watch?v=5ghKIqYd_Bc)

* Github:
  * [Christopher Masto's Documentation on Github](https://github.com/masto)
    * [NSPanel-Demo-Files](https://github.com/masto/NSPanel-Demo-Files)
    * [HowTo](https://github.com/masto/NSPanel-Demo-Files/wiki/HowTo)
  * Cables & Coffee
    * [lillaeriika/NSPanel](https://github.com/lillaeriika/NSPanel)
  * ilyavolodin
    * [ilyavolodin/NSPanel](https://github.com/ilyavolodin/NSPanel) ColorWhell and more about sync.
    * [Syncing slider between HA and Nextion display](https://community.home-assistant.io/t/syncing-slider-between-ha-and-nextion-display/379132/5)

* Other:
  * Blakadder's
    * [Sonoff NSPanel EU Switch in Detail](https://blakadder.com/nspanel-teardown/)

* Dokumentation:
  * Nextion:
    * [Nextion Editor](https://nextion.tech/nextion-editor/)
    * [Nextion Editor Guide](https://nextion.tech/editor_guide/)
    * [The Nextion Instruction Set](https://nextion.tech/instruction-set/)
  * Other:
    * [Nextion HMI Color Converter](https://nodtem66.github.io/nextion-hmi-color-convert/index.html)
    * [Material Design Icons](https://materialdesignicons.com/)
  * ESPHome:
    * [Display Component](https://esphome.io/components/display/index.html)
    * [Nextion TFT LCD Display](https://esphome.io/components/display/nextion.html)

## First Try:
![First screeen](/First_Try/Nextion/Image/FirstTry.png)  

First try is all about getting data all the way from Home Assistant via ESPHome to NSPanel, and button to send signal all the way back from NSPanel via ESPHome to Home Assistant.
In my Home Assistant I have created timer to used when I am cooking and baking, I want to bee able to select the rigth timer and start it from my NSPanel, when timer is finish an automation will bee sewnd with Telegram, and that is werry nice I cn bee in garden and still a notification on my phone.


###  [*Click Here!* for *Explanation* and the *Files* from *First try!*](./First_Try/README.md)
 

## New Page Layout:
| | |
|:---:|:---:|
|![QuickGuide](./NewPageLayout/png/QuickPage.png)|![TimerPage](./NewPageLayout/png/TimerPage.png)|

###  [*Click Here!* for *Explanation* and the *Files* from *NewLayout!*](./NewPageLayout/README.md)


