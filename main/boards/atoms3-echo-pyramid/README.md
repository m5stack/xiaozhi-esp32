# AtomS3 + EchoPyramid

> **Note:** AtomS3 无 PSRAM，不支持语音唤醒，可通过按压屏幕或按键唤醒。

----------
## 快速体验

下载、安装 [M5Burner](https://docs.m5stack.com/zh_CN/uiflow/m5burner/intro) ，打开 M5Burner 搜索 EchoPyramid 下载小智固件，烧录。

----------
## 发布固件

```shell
python scripts/release.py atoms3-echo-pyramid
```

生成的固件在 releases 目录下

----------
## 编译固件

**配置编译目标为 ESP32S3 **

```bash
idf.py set-target esp32s3
```

**配置**

```bash
idf.py menuconfig
```

**选择板子**

```
Xiaozhi Assistant -> Board Type -> AtomS3 + Echo Pyramid
```

**修改 flash 大小**

```
Serial flasher config -> Flash size -> 8 MB
```

**选择分区表**

```
Partition Table -> Custom partition CSV file -> partitions/v2/8m.csv
```

**编译烧录固件**

```bash
idf.py build flash monitor
```
