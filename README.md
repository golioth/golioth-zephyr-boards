# Golioth Zephyr Board Definitions

This repository contains Zephyr board definitions for custom Golioth boards. It
can be included in a west manifest file so that it is available in standalone
repositories for Reference Designs and other demos.

## Aludel Elixir

### Flashing the Espressif Bluetooth HCI Sample

The Aludel Elixir has an ESP32-C3 Mini module that may be used as a Bluetooth HCI module. To use
this featured, the Espressif Bluetooth HCI Sample application must first be flashed onto the ESP32
module. A binary built for the Aludel Elixir is included in this repository for your convenience.

1. Connect the GND, TX, & RX pins of a USB-to-serial programmer to the J17 header on the Elixir Rev
   B as shown in the attached image.

   ![ESP32-C3 serial pinout on the Golioth Aludel
   Elixir](images/aludel_elixir_esp32_programming_header.png)

2. Holding down the ESP32-C3 BOOT button, press the ESP32-C3 RST button to reset the ESP32-C3 in
   programming mode
4. Flash the HCI binary from this repository (make sure to replace the serial port with the serial
   port of your USB-to-serial programmer):

   ```
   esptool.py --chip auto --port /dev/ttyUSB0 --baud 115200 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size 4MB 0x0 binaries/esp32c3-mini-ble-hci-espidf5.4.1.bin
   ```

5. Verify the binary is running opening the USB-to-serial port in a serial terminal. When you press
   the ESP32-C3 RST button, and you should see output that ends like this:

   ```
   ...
   I (410) UHCI: HCI messages can be communicated over UART1:
   --PINs: TxD 7, RxD 6
   --Baudrate: 115200
   I (410) main_task: Returned from app_main()
   ```

Check the section below on Using Bluetooth HCI to configure the Elixir to use ESP32 BLE HCI.

**OPTIONAL:** If you wish to build the HCI binary yourself:

1. Navigate to [the Espressif Bluetooth HCI
   sample](https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/hci/controller_hci_uart_esp32c3_and_esp32s3).
2. Disable flow control by adding the following to `sdkconfig.defaults`:

    ```
    CONFIG_EXAMPLE_HCI_UART_FLOW_CTRL_ENABLE=n
    ```
3. Update the pin definitions in the `src/main.c` as follows:

    ```
    #define GPIO_UART_TXD_OUT  (7)
    #define GPIO_UART_RXD_IN   (6)
    #define GPIO_UART_RTS_OUT  (-1)
    #define GPIO_UART_CTS_IN   (-1)
    ```

4. Remove CTS/RST from `*_PIN_SEL` definitions so they match as follows:

   ```
   #define GPIO_OUTPUT_PIN_SEL  (1ULL<<GPIO_UART_TXD_OUT)
   #define GPIO_INPUT_PIN_SEL   (1ULL<<GPIO_UART_RXD_IN)
   ```

6. Set target, build, and merge into single binary

    ```
    idf.py set-target esp32c3
    idf.py build
    cd build
    esptool.py --chip esp32c3 merge_bin -o merged-flash.bin @flash_args
    ```

5. Your new pre-compiled binary is now `build/merged-flash.bin`

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

5. Verify the binary is running opening the USB-to-serial port in a serial terminal. When you press
   the ESP32-C3 RST button, and you should see output that ends like this:

   ```
   ...
   AT cmd port:uart1 tx:7 rx:6 cts:5 rts:4 baudrate:115200
   module_name: MINI-1
   max tx power=78, ret=0
   v3.3.0.0
   ```

Check the section below on Using WiFi to configure the Elixir to use ESP32 WiFi.

### Using Bluetooth HCI

To enable Bluetooth HCI, first include the DTS file in your `boards/aludel_elixir_ns.overlay` file:

```
#include <aludel_elixir_ble_hci.dtsi>
```

Then disable flow control in your `boards/aludel_elixir_ns.conf` file:

```
CONFIG_BT_HCI_ACL_FLOW_CONTROL=n
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


