---
id: 1vas9emru4qd67ndevnbaun
title: Doorbell
desc: ""
updated: 1655497834534
created: 1654185774400
---

This is the documentation for the connected doorbell used at my parent's house.

## General Workflow

![doorbell-workflow](/assets/doorbell.png)

## Update Workflow

To update, the ESP32 is querying https://hq.mvd.ovh (password protected).

![doorbell-update](/assets/doorbell_update.png)

## Installing the environment

Follow Espressif's [Getting Started](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html) to install esp-idf.

Clone the `doorbell` project and open it in VSCode :

```bash
git clone git@github.com:joscherrer/doorbell.git
code doorbell/
```

Then you can either flash with UART, or [publish a new OTA version](#publishing-a-new-ota-version)

## Configuring the firmware

Configure the project with the command `ESP-IDF: SDK Configuration editor (menuconfig)`.

Here are the important values to set with `menuconfig`.

```bash
# Ethernet configuration
CONFIG_USE_INTERNAL_ETHERNET=y
CONFIG_ETH_PHY_LAN87XX=y
CONFIG_ETH_MDC_GPIO=23
CONFIG_ETH_MDIO_GPIO=18
CONFIG_ETH_PHY_RST_GPIO=-1
CONFIG_ETH_PHY_ADDR=0

# Ethernet
CONFIG_ETH_ENABLED=y
CONFIG_ETH_USE_ESP32_EMAC=y
CONFIG_ETH_PHY_INTERFACE_RMII=y
CONFIG_ETH_RMII_CLK_OUTPUT=y
CONFIG_ETH_RMII_CLK_OUT_GPIO=17
CONFIG_ETH_USE_SPI_ETHERNET=y
CONFIG_ETH_SPI_ETHERNET_W5500=y

# OTA
CONFIG_OTA_FIRMWARE_UPGRADE_URL="<firmware_url>"
CONFIG_OTA_ENABLE_PARTIAL_HTTP_DOWNLOAD=y

# Partition Table
CONFIG_PARTITION_TABLE_TWO_OTA=y
CONFIG_PARTITION_TABLE_CUSTOM_FILENAME="partitions.csv"

# Serial flasher config
CONFIG_ESPTOOLPY_FLASHSIZE_4MB=y
CONFIG_ESPTOOLPY_FLASHSIZE="4MB"

# Google IoT Core Configuration
CONFIG_GIOT_PROJECT_ID="project_id"
CONFIG_GIOT_LOCATION="us-central1"
CONFIG_GIOT_REGISTRY_ID="registry_id"
CONFIG_GIOT_DEVICE_ID="device_id"
```

## Publishing a new OTA version

1. (Optional) Change the firmware version in the root CMakeLists.txt
2. (Optional) Build the project
3. Run the `publish.sh` script

## Setup Google IoT Core

Doorbell uses IoT Core. This is how it should be setup

Create keys in `.secrets/` folder ([guide](https://cloud.google.com/iot/docs/how-tos/credentials/keys#generating_an_elliptic_curve_keys)).

```bash
openssl ecparam -genkey -name prime256v1 -noout -out ec_private.pem
openssl ec -in ec_private.pem -pubout -out ec_public.pem
```

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

To update the device credentials :

```bash
gcloud iot devices credentials delete 0 \
    --region="europe-west1" \
    --registry=home_nieder \
    --device="doorbell-0"

gcloud iot devices credentials create \
    --region="europe-west1" \
    --registry="home_nieder" \
    --device="doorbell-0" \
    --path=".secrets/ec_public.pem" \
    --type="es256-pem"
```
