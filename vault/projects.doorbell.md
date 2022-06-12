---
id: 1vas9emru4qd67ndevnbaun
title: Doorbell
desc: ""
updated: 1654992563336
created: 1654185774400
---

This is the documentation for the connected doorbell used at my parent's house.

## General Workflow

![doorbell-workflow](/assets/doorbell.png)

## Update Workflow

To update, the ESP32 is querying https://hq.mvd.ovh (password protected).

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
4. Configure your network connectivity.

Open the project in VSCode, then build the project.
Then you can either flash with UART, or [publish a new OTA version](#publishing-a-new-ota-version)

## Publishing a new OTA version

1. (Optional) Change the firmware version in the root CMakeLists.txt
2. (Optional) Build the project
3. Run the `publish.sh` script

## Setup Google IoT Core

Doorbell uses IoT Core. This is how it should be setup

Create keys in `.secrets/` folder following [this guide](https://cloud.google.com/iot/docs/how-tos/credentials/keys#generating_an_elliptic_curve_keys).

Create the topics :
```bash
gcloud pubsub topics create doorbell-events
gcloud pubsub topics create doorbell-state
```

Create the registry :
```bash
gcloud iot registries create home_nieder \
    --region="europe-west1" \
    --state-pubsub-topic="doorbell-state" \
    --event-notification-config="topic=doorbell-events" \
    --log-level="error" \
    --enable-http-config \
    --enable-mqtt-config
```

Create the device :
```bash
gcloud iot devices create doorbell-0 \
    --region="europe-west1" \
    --registry=home_nieder \
    --log-level=error \
    --public-key="path=.secrets/ec_public.pem,type=es256-pem"
```