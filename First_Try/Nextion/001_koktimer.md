# koktimer(Page)

## Picture in Use

| koktimer |
| :---:     |
| ![mainpageOFF](./Nextion/Image/2.png)   |
|ID:2 - 480x320 pixel|


## Page Atribute - home
|Atribute  | Value       |
|:---      | :---:       |
| type:    | 121         |
| id:      | 0           |
| vscope:  | local       |
| sta:     | image       |
| pic:     | 2           |
| x:       | 0           |
| y:       | 0           |
| w:       | 480         |
| h:       | 320         |

## Page Event
* Preinitialize Event
```
```
* Postinitialize Event
```
```
* Touch Press Event
  * Send Componebt ID : Disabled
```
```
* Touch Release Event
  * Send Componebt ID : Disabled
```
```
* Page exit Event
```
```

## Page - Non Visual Components 
### Non Visual Components Attribute - sleep_timer(Timer)
|Atribute  | Value       |
|:---      | :---:       |
| type:    | 51          |
| id:      | 8           |
| objname: | sleep_timer |
| vscope:  | local       |
| tim:     | 30000       |
| en:      | 1           |

#### Timer Event  - sleep_display(Timer)
```
// Turn screen off
brightness.val=dim
dim=0
// Go to screensaver page
page screensaver
// Timer done
sleep_timer.en=0
```

### Non Visual Components Attribute - screen_touch(TouchCap)
|Atribute  | Value        |
|:---      | :---:        |
| type:    | 5            |
| id:      | 9            |
| objname: | screen_touch |
| vscope:  | local        |
| val:     | 0            |

#### Event - tc0(TouchCap)
* Touch Press Event
  * Send Componebt ID : Disabled
```
```
* Touch Release Event
  * Send Componebt ID : Disabled
```
//Reset sleep timer
sleep_timer.en=1
```

### Non Visual Components Attribute - brightness(Variable)
|Atribute  | Value      |
|:---      | :---:      |
| type:    | 52         |
| id:      | 10         |
| objname: | brightness |
| vscope:  | global     |
| sta:     | Number     |
| val:     | 100        |

#### Event - brightness(Variable)
```
```

### Component Atribute - Text
| Name      | friendlyname(Text)  | t1(Text)   | t2(Text)   | t3(Text)   | time(Text)          |
| :---      | :---:               | :---:      | :---       | :---:      | :---:               |
| type:     | 116                 | 116        | 116        | 116        | 116                 |
| id:       | 1                   | 5          | 6          | 7          | 11                  |
| objname:  | friendlyname        | t1         | t2         | t3         | time                | 
| vsccope:  | global              | global     | global     | global     | global              |
| sta:      | crop image          | crop image | crop image | crop image | crop image          |
| key:      | None                | None       | None       | None       | None                |
| font:     | 1                   | 1          | 1          | 1          | 1                   |
| picc:     | 2                   | 0          | 0          | 0          | 2                   |
| pco:      | 65530               | 65530      | 65530      | 65530      | 65530               |
| xcen:     | Center              | Right      | Right      | Right      | Left                |
| ycen:     | Center              | Center     | Center     | Center     | Center              |
| pw:       | Charater            | Charater   | Charater   | Charater   | Charater            |
| txt:      | Vælg en køken timer | newtxt     | newtxt     | newtxt     | 2022/04/10 23:59:59 |
| txt_maxi: | 40                  | 15         | 15         | 10         | 40                  |
| isbr:     | False               | False      | False      | False      | False               |
| spax:     | 0                   | 0          | 0          | 0          | 0                   |
| spay:     | 0                   | 0          | 0          | 0          | 0                   |
| x:        | 0                   | 349        | 349        | 349        | 0                   |
| y:        | 70                  | 179        | 215        | 251        | 0                   |
| w:        | 440                 | 100        | 100        | 100        | 240                 |
| h:        | 50                  | 30         | 30         | 30         | 30                  |

##### Event - t_temp
* Touch Press Event
  * Send Component ID : Disabled
```
```
* Touch Release Event
  * Send Component ID : Disabled
```
```

### Atribute - button
| Name      | b0(button)  | b1(button)  | b2(button)  |
| :---      | :---:       | :---:       | :---:       |
| type:     | 98          | 98          | 98          |
| id:       | 2           | 3           | 4           |
| style:    | 3D_Auto     | 3D_Auto     | 3D_Auto     |
| objname:  | b0          | b1          | b2          |
| vsccope:  | local       | local       | local       |
| sta:      | solid color | solid color | solid color |
| font:     | 0           | 0           | 0           |
| bco:      | 50712       | 50712       | 50712       |
| bco2:     | 1024        | 1024        | 1024        |
| pco:      | 0           | 0           | 0           |
| pco2:     | 65535       | 65535       | 65535       |
| xcen:     | Center      | Center      | Center      |
| ycen:     | Center      | Center      | Center      |
| txt:      | Prev        | Play        | Next        |
| txt_maxi: | 10          | 10          | 10          |
| isbr:     | False       | False       | False       |
| spax:     | 0           | 0           | 0           |
| spay:     | 0           | 0           | 0           |
| x:        | 0           | 174         | 349         |
| y:        | 120         | 120         | 120         |
| w:        | 100         | 100         | 100         |
| h:        | 50          | 50          | 50          |
  
#### Event - All Buttons
* Touch Press Event
  * Send Component ID : **Checked**
```
```
* Touch Release Event
  * Send Component ID : **Checked**
```
```