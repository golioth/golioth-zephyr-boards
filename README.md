# Golioth Zephyr Board Definitions

This repository contains Zephyr board definitions for custom Golioth boards. It
can be included in a west manifest file so that it is available in standalone
repositories for Reference Designs and other demos.

## Aludel Elixir

### Flashing the Espressif AT Binary

The Aludel Elixir has an ESP32-C3 Mini module that may be used to add WiFi functionality to the
board. To use this featured, the Espressif AT Binary must first be flashed onto the ESP32 module.

1. Connect the GND, TX, & RX pins of a USB-to-serial programmer to the J17 header on the Elixir Rev
   B as shown in the attached image.

   ![ESP32-C3 serial pinout on the Golioth Aludel
   Elixir](images/aludel_elixir_esp32_programming_header.png)

2. Holding down the ESP32-C3 BOOT button, press the ESP32-C3 RST button to reset the ESP32-C3 in
   programming mode
3. Download ESP32-C3-MINI-1-AT-V3.3.0.0.zip ([AT
   Releases](https://docs.espressif.com/projects/esp-at/en/latest/esp32c3/AT_Binary_Lists/esp_at_binaries.html))
   and extract the zip
4. Flash the ESP-AT firmware (make sure to replace the serial port with the serial port of your
   USB-to-serial programmer):

   ```
   cd ESP32-C3-MINI-1-AT-V3.3.0.0/ESP32-C3-MINI-1-AT-V3.3.0.0/

   esptool.py --chip auto --port /dev/ttyUSB0 --baud 115200 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size 4MB 0x0 bootloader/bootloader.bin 0x60000 esp-at.bin 0x8000 partition_table/partition-table.bin 0xd000 ota_data_initial.bin 0x1e000 at_customize.bin 0x1f000 customized_partitions/mfg_nvs.bin
   ```

5. Verify the binary is running opening the USB-to-serial port in a serial terminal. When you press the
   ESP32-C3 RST button, and you should see output that ends like this:

   ```
   ...
   AT cmd port:uart1 tx:7 rx:6 cts:5 rts:4 baudrate:115200
   module_name: MINI-1
   max tx power=78, ret=0
   v3.3.0.0
   ```

### Using WiFi

To enable WiFi, first include the DTS file in your `boards/aludel_elixir_ns.overlay` file:

```
#include <aludel_elixir_wifi.dtsi>
```

Then enable WiFi in your `boards/aludel_elixir_ns.conf` file:

```
CONFIG_ALUDEL_WIFI=y
CONFIG_ALUDEL_WIFI_SHELL=y
```
