# screensaver(Page)

## Picture in Use

## Page Atribute - home
|Atribute  | Value       |
|:---      | :---:       |
| type:    | 121         |
| id:      | 0           |
| vscope:  | local       |
| sta:     | solid color |
| pco:     | 0           |
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
dim=koktimer.brightness.val    // Un-blank the screen
koktimer.sleep_timer.en=1      // Reenable the sleeptimer
page koktimer                  // Switch back to page 0
```
* Page exit Event
```
```
