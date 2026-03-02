# simplelink-ble_sdk_2_02_08_12
This is Texas Instruments BLE SDK for CC26XX Devices. Texas Instruments does not support an active GitHub for this. 
The following link is provided https://software-dl.ti.com/simplelink/esd/ble_stack_2_x/2.02.08.12/exports/release_notes_BLE_Stack_2_2_8.html

## Overview
This repository is intended to stay unmodified. Any specific source files that need modified should be copied into the specific project. 

### Important Note
The source files in this SDK follow the relative path or sublfolder/structured inclusion. For example _npi_tlc.c_
```
// ****************************************************************************
// includes
// ****************************************************************************
...
#include "inc/npi_config.h"
```
Where __npi_config.h__ lives at the path: _ble_sdk_2_02_08_12/src/components/npi/src/inc_

Projects that use this SDK will need to make sure the include paths account for this.

### Usage
This repository is designed to be used as a submodule.

#### Submodule Instructions
##### Example add ble_sdk_2_02_08_12 to current project
``` git submodule add https://github.com/kyle620/simplelink-ble_sdk_2_02_08_12 ```

##### Example clone Repository with this as submodule already
``` git submodule update --init --recursive ```
