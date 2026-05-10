# R2P2-ESP32 Installer

Web-based firmware installer for R2P2-ESP32 (PicoRuby on ESP32).

https://picoruby.org/R2P2-ESP32-installer/

## Usage

1. Connect your ESP32 device via USB
2. Open the installer page in Chrome, Edge, or Opera
3. Select your board and Ruby variant from the dropdowns
4. Click **Connect and Flash** and select the serial port
5. Wait for flashing to complete
6. After flashing completes, click **Open PicoRuby Terminal** or **OK**

## Requirements

- Browser with Web Serial API support (Chrome, Edge, Opera)
- USB-connected ESP32 device

## Supported combinations

Available variants match the [R2P2-ESP32 release workflow](https://github.com/picoruby/R2P2-ESP32/blob/master/.github/workflows/release.yml).

| Board | FemtoRuby | PicoRuby |
|-------|:---------:|:--------:|
| ESP32 | ✓ | — |
| ESP32-C3 | ✓ | — |
| ESP32-S3 | ✓ | ✓ |
| ESP32-S3 (USB Console) | ✓ | ✓ |

The Ruby variant dropdown is filtered automatically based on the selected board.

## Local development

```sh
python3 -m http.server 8080
```

Open `http://localhost:8080` in Chrome, Edge, or Opera.
Web Serial API requires a secure context; `localhost` qualifies without HTTPS.

## Updating firmware

### Directory structure

Firmware files are organized by chip in the `firmware/` directory:

```
firmware/
  storage.bin                          # shared across all chips
  esp32/
    bootloader.bin
    partition-table.bin
    femtoruby.bin
  esp32c3/
    bootloader.bin
    partition-table.bin
    femtoruby.bin
  esp32s3/
    bootloader.bin
    partition-table.bin
    femtoruby.bin
    picoruby.bin
  esp32s3-usb_console/
    bootloader.bin
    partition-table.bin
    femtoruby.bin
    picoruby.bin
```

### Flash offsets

| Part | ESP32 offset | ESP32-C3/S3 offset | Source |
|------|--------------|--------------------|--------|
| bootloader.bin | 0x1000 (4096) | 0x0000 (0) | ESP-IDF chip default |
| partition-table.bin | 0x8000 (32768) | 0x8000 (32768) | ESP-IDF default |
| femtoruby.bin / picoruby.bin | 0x10000 (65536) | 0x10000 (65536) | `partitions.csv` factory offset |
| storage.bin | 0x210000 (2162688) | 0x210000 (2162688) | factory offset + factory size (0x10000 + 2M) |

If `partitions.csv` changes, update the offsets accordingly.

### Manifest generation

The installer dynamically generates the manifest at runtime based on the selected board and Ruby variant. No static `manifest.json` is used. Firmware metadata is fetched from the [latest GitHub Release](https://github.com/picoruby/R2P2-ESP32/releases/latest) of the R2P2-ESP32 repository.

## Third-party components

This project uses [ESP Web Tools](https://github.com/esphome/esp-web-tools) by Open Home Foundation, licensed under the Apache License 2.0.

## License

This project is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.
