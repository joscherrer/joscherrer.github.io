---
id: 1vas9emru4qd67ndevnbaun
title: Doorbell
desc: ""
updated: 1654200101237
created: 1654185774400
---

This is the documentation for the connected doorbell used at my parent's house.

## General Workflow

![doorbell-workflow](/assets/doorbell.png)

## Update Workflow

![doorbell-update](/assets/doorbell_update.png)

## Building the firmware

Follow Espressif's [Getting Started](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html) to install esp-idf.

Clone the `doorbell` project and open it in VSCode :
```bash
git clone git@github.com:joscherrer/doorbell.git
code doorbell/
```

Configure the project with the command `ESP-IDF: SDK Configuration editor (menuconfig)`.  
1. In the _OTA_ menu, configure the update URL and other parameters
2. In the _Serial Flasher config_ menu, update the Flash size to 4MB minimum
3. In the _Partition table_ menu, update the Partition table to `Factory app, two OTA definitions`
