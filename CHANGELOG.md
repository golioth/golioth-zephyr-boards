<!-- Copyright (c) 2023 Golioth, Inc. -->
<!-- SPDX-License-Identifier: Apache-2.0 -->

# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
