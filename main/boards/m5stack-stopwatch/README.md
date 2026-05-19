# StopWatch

-----------
## hardware

* MCU:ESP32-S3
    * Flash: 16MiB
    * PSRAM: 8MiB
    * SYS_I2C/I2C1
        * SCL -- G48
        * SCA -- G47
* PMIC: M5PM1@0x6E
    * Interface: SYS_I2C
* IO Expander: M5IOE1@0x4F
    * Interface: SYS_I2C
* Power
    * Charge Enable -- M5PM1_CHG_EN_PP
    * Charge State -- M5PM1_G2
    * Charge Current Prog -- M5PM1_G3
    * Motor -- M5IOE1_G9
    * Ext output -- M5PM1_BOOST5V_EN_PP
    * Ext output voltage -- M5PM1_5VOUT_ADC_IN
* Display
    * Power -- M5IO1_G8
    * Interface: QSPI
    * Driver: CO5300
    * Resolution: 466x466
    * 圆形显示屏 1.75 Inch Amoled
    * Pin Map
        * RST      -- M5IOE1_G5
        * TE       -- G38
        * QSPI_CS  -- G39
        * QSPI_SCK -- G40
        * QSPI_D0  -- G41
        * QSPI_D1  -- G42
        * QSPI_D2  -- G46
        * QSPI_D3  -- G45
* CTP: CST820B@0x15
    * RST -- M5IOE1_G4
    * INT -- G13
    * Interface: SYS_I2C
* Audio
    * Power -- M5IOE1_G3
    * CODEC: ES8311@0x18
        * Contral Interface: SYS_I2C
        * Data Interface:
            * I2S_MCLK -- G18
            * I2S_BCLK -- G17
            * I2S_LRCK -- G15
            * I2S_DIN  -- G21
            * I2S_DOUT -- G16
    * PA -- M5IOE1_G10
* Button
    * Btuuon1 - G1
    * Button2 - G2
* RTC: RX8130CE@0x32
    * Interface: SYS_I2C
* Grove
    * GND
    * 5V
    * G10
    * G11

---------------
## Build & Test

### Configuration

```shell
idf.py set-target esp32s3
idf.py menuconfig
# Xiaozhi Assistant -> Board Type -> M5Stack StopWatch
idf.py build flash monitor
```

### merge bin

```shell
esptool.py --chip esp32s3 merge_bin \
    -o M5Stack-StopWatch-xiaozhi_0x00.bin \
    --flash_mode dio --flash_freq 80m --flash_size 16MB \
    0x00 build/bootloader/bootloader.bin \
    0x20000 build/xiaozhi.bin \
    0x8000 build/partition_table/partition-table.bin \
    0xd000 build/ota_data_initial.bin \
    0x800000 build/generated_assets.bin
```

### burn firmware

```shell
esptool.py -b 1500000 write_flash -z 0 M5Stack-StopWatch-xiaozhi_0x00.bin
```

### release firmware

```shell
python scripts/release.py m5stack-stopwatch
```
