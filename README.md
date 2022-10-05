# Matter ESP32 Firmware files

These are pre-built Matter images that are ready to flash onto ESP32 boards. They are built from the repo https://github.com/project-chip/connectedhomeip.

- `matter-program.bin` is the merged firmware file from the other files within respective folder
- `all-clusters` example has all features like tempearture, thermostat, heater, light, color, brightness, on/off, lock, window, etc.
- `lock-app` is a door lock example
- `lighting-app` is a light bulb example with brightness, on/off, and color control (via hue & saturation behind the scenes)
	- Note: folder contains `LEDWidget.cpp` which has been modified to work on 10 pixel color LED strip

# Pre-reqs
Install esptool
```cmd
pip install esptool
```

# Flash firmware

- Merge command
```cmd
esptool --chip esp32 merge_bin -o matter-program.bin --flash_mode dio --flash_size 4MB --flash_freq 40m 0x1000 bootloader.bin 0x8000 partition-table.bin 0xf000 ota_data_initial.bin 0x20000 chip-all-clusters-app.bin

esptool --chip esp32 merge_bin -o matter-program.bin --flash_mode dio --flash_size 4MB --flash_freq 40m 0x1000 bootloader.bin 0x8000 partition-table.bin 0x10000 chip-lock-app.bin

esptool --chip esp32 merge_bin -o matter-program.bin --flash_mode dio --flash_size 4MB --flash_freq 40m 0x1000 bootloader.bin 0x8000 partition-table.bin 0xf000 ota_data_initial.bin 0x20000 chip-lighting-app.bin
```

- Erase flash
```cmd
esptool erase_flash
```

- Flash firmware
```cmd
esptool -b 460800 --before default_reset --after no_reset write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x0 matter-program.bin

### For reflashing just the app, w/out erasing network credentials
esptool -b 460800 --before default_reset --after no_reset --chip esp32 write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x20000 chip-lighting-app.bin
```

# Monitor logs (note, it doesn't restart program, just monitors logs at current moment)
- Mac:
```cmd
### Get usb devices
ls /dev/cu.usb*

### Monitor
screen /dev/cu.usbserial-0 115200
```
- Windows:
	1. Open Putty
	2. Select Session
	3. Select Serial under Connection type
	4. Enter the com port and set speed to 115200
	5. Click open