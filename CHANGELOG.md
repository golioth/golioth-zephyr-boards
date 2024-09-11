<!-- Copyright (c) 2024 Golioth, Inc. -->
<!-- SPDX-License-Identifier: Apache-2.0 -->

# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.1] - 2024-09-11

### Changed

- Aludel Elixir: external flash is now disabled by default

## [2.0.0] - 2024-09-05

### Breaking Changes

- The regulator library is no longer enabled by default for the Aludel Elixir and should be enabled
  in the board overlay file of applications that use it: `CONFIG_REGULATOR=y`.

### Changed

- Update Aludel Elixir to Zephyr Hardware Model v2
- Update Aludel Mini to Zephyr Hardware Model v2

## [1.2.0] - 2024-06-12

### Breaking Changes

- The onboard SPI NOR flash (node label `w25q32jv`) has been removed from the Aludel Mini
  (`aludel_mini_v1_sparkfun9160_ns`) & Aludel Elixir Rev A (`aludel_elixir_ns@A`) boards. On these
  boards, the SPI flash and mikroBUS SPI interface do not share the same pins, so they must be
  assigned to different peripheral numbers (0-3) in the nRF9160. This prevented the mikroBUS serial,
  I2C, and SPI interfaces from being used simultaneously on these boards. Removing the SPI flash
  node allows these mikroBUS peripherals to be enabled by default.

  On these boards, the peripheral assignments have been updated as follows:
  - `UART0`: USB-to-serial console
  - `UART1`: mikroBUS serial
  - `I2C2`: onboard I2C devices, QWIIC, and mikroBUS I2C
  - `SPI3`: mikroBUS SPI

### Added

- Add `pwm-buzzer0` alias for Aludel Elixir boards.
- Add BME280 DT node to Aludel Elixir Rev B board.
- Add `rtc0` alias for Aludel Elixir boards.
- Add `accel0` alias for Aludel Elixir Rev B board.
- Add startup & on-off delays for Aludel Elixir regulators.
- Add `mikrobus-serial`, `mikrobus-i2c`, and `mikrobus-spi` aliases.

### Fixed

- Fix Aludel Elixir board PWM buzzer DT definition.
- Fix `cs-gpios` property for `spi3` node on Aludel Elixir boards.

### Changed

- Deprecate `click-uart` and `click-i2c` aliases (use `mikrobus-*` aliases instead).

### Removed

- Remove non-working nrf9160dk_nrf9160 overlay for arduino_uno_click shield.

## [1.1.1] - 2024-03-15

### Fixed

- Remove `v1` version specifier from Aludel Elixir board (`aludel_elixir_v1` → `aludel_elixir`).

## [1.1.0] - 2024-03-15

### Added

- Add .editorconfig and .clang-format.
- Add `NRF_PS_EN` to feather connector GPIO nexus for the `aludel_mini_v1_sparkfun9160` board.
- Add `feather_{serial,i2c,spi}` node labels to `aludel_mini_v1_sparkfun9160` board.
- Add mikroBUS connector GPIO nexus nodes to `aludel_mini_v1_sparkfun9160` board.
- Add `nrf9160dk_nrf9160` overlays for `arduino_uno_click` shield.
- Add support for the Aludel Elixir v1 (`aludel_elixir_v1`) board.

### Fixed

- Suppress `unique_unit_address_if_enabled` warnings for the `aludel_mini_v1_sparkfun9160` board.

### Changed

- Disable external NOR flash (`CONFIG_SPI_NOR=n`) on the `aludel_mini_v1_sparkfun9160` board to fix a conflict with the SPI3 peripheral.
- Enable internal I2C pull-ups for I2C1 peripheral pins on the `aludel_mini_v1_sparkfun9160` board.

## [1.0.1] - 2023-09-01

### Fixed

- Fix `UART_CONSOLE` build warnings for the `aludel_mini_v1_sparkfun9160` board.

## [1.0.0] - 2023-08-14

### Breaking Changes

- This repo is now a Zephyr module. It should be included as modules/lib
  subdirectory (e.g. modules/lib/golioth-boards) using west.yml so that the
  board definition is automatically available in your build.

### Added

- Changelog
- zephyr/module.yml; include this library at modules/lib/golioth-boards using
  west.yml

### Fixed

- aludel_mini_v1_sparkfun9160
  - Params removed from initialization function (void) to adhere to new Zephyr
    SYS_INIT
  - Rename included DTS files to .dtsi
  - Remove erroneous timer nodes in devicetree
