# M5Stack CoreP4

-----------
## Overview

- **MCU**: ESP32-P4
- **PSRAM**: 8MB
- **Flash**: 16MB
- **Wi-Fi**: ESP32-C6
- **Display**: 480×480 MIPI LCD, Capacitive Touch Panel

-----------
## Hardware

* **I2C1**
    * SCL -- G9
    * SDA -- G11
* **M5STP2 / PM1 (Power)**
    * Interface: I2C1 @**0x6E**
* **IO Expander: M5IOE1**
    * Interface: I2C1 @**0x6F**
* **Wi-Fi**
    * RST -- G42
    * SDIO
        * CMD -- G44
        * CLK -- G43
        * D0 -- G45
        * D1 -- G46
        * D2 -- G47
        * D3 -- G48
* **Display**
    * Driver: ST7102
    * Interface: MIPI
    * PWR_EN -- M5IOE1_G10 (Active High)
    * RST    -- M5IOE1_G11
    * BL     -- M5IOE1_G9/PWM1
* **Touch Panel**
    * Driver: CST3xx
    * Interface: I2C1
    * RST -- M5IOE1_G8
    * INT -- G1
* **Audio**
    * PWR_EN -- M5IOE1_G1 (Active High)
    * ADC: ES7210
        * Control Interface: I2C1
        * Data Interface: I2S0
    * DAC: ES8311
        * Control Interface: I2C1
        * Data Interface: I2S0
            * MCLK -- G2
            * BCLK -- G6
            * WS   -- G4
            * DOUT -- G5
            * DIN  -- G3
    * PA: AW8737A
        * EN -- M5IOE1_G3 (Active High)
* **IMU: BMI270**
    * Interface: I2C1 @0x68
    * INT -- G0
* **RTC: RX8130CE**
    * Interface: I2C1 @0x51
    * INT -- G4

---------------
## Build & Test

### Configuration

```shell
idf.py menuconfig
```

### merge bin

```shell
esptool.py --chip esp32p4 merge_bin \
    -o M5Stack-CoreP4-xiaozhi-v2.2.3-20260415_0x00.bin \
    --flash_mode dio --flash_freq 80m --flash_size 16MB \
    0x2000 build/bootloader/bootloader.bin \
    0x20000 build/xiaozhi.bin \
    0x8000 build/partition_table/partition-table.bin \
    0xd000 build/ota_data_initial.bin \
    0x800000 build/generated_assets.bin
```

### burn firmware

```shell
esptool.py -b 1500000 write_flash -z 0 M5Stack-CoreP4-xiaozhi-v2.2.3-20260415_0x00.bin
```

### release firmware

> [!NOTE]
> Not support

```shell
python scripts/release.py m5stack-corep4
```