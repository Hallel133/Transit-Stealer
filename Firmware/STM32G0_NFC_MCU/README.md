# STM32G0 NFC MCU Firmware

Firmware for the `STM32G0B1CEUx` (U8) on the NFC MCU Unit sheet (`NMU.kicad_sch`). Drives the `ST25R3911B-AQW` NFC/RFID reader-writer IC over SPI, and communicates with the RK3566 host.

Not yet started. Planned scope:
- SPI driver + IRQ handling for the ST25R3911B
- ISO14443 / transit-card protocol stack
- Host link to the RK3566 (interface TBD — see `Firmware/RK3566_Host`)
