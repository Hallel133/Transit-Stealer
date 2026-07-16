# Transit Stealer — V2

Open-hardware platform for **authorized NFC/RFID transit-card security research**: reading, analyzing, and interacting with transit fare cards for the purpose of testing and improving their security. This is not intended for, and must not be used for, unauthorized card cloning, skimming, or fare evasion.

## Board overview

V2 is a two-processor board:

- **RK3566** — main SoC (Cortex-A55 quad-core), running the host-side application. Referenced across three sheets since its pins are grouped by function (power, storage, RAM).
- **STM32G0B1CEUx** — dedicated MCU driving the NFC front end, largely independent of the RK3566 (own USB-C, own power sequencing), sharing only power/ground and a communication link with the rest of the board.
- **ST25R3911B-AQW** — NFC reader/writer IC, driven by the STM32G0 over SPI, feeding a custom hand-wound PCB loop antenna (see `Docs/Custom_Antenna.png`).
- **RK817-5 PMIC** — USB-C power in, battery charge/protection, all board rails (3.3V / 1.8V / DDR / CPU / GPU-NPU).
- **KLM8G1GETF-B041** eMMC and **D0811PM2FDGUK-U** LPDDR4 for RK3566 storage/RAM.
- **RTL8723BS** — WiFi/BT module (SDIO + UART).

Schematic is fully wired and ERC-clean (0 errors) as of this upload; PCB layout has not started.

## Repository layout

```
Hardware/
  Schematic/      V2 project + all hierarchical sheets (.kicad_sch/.kicad_pro/.kicad_prl), sym-lib-table
  PCB/            V2.kicad_pcb (layout, currently minimal/near-empty)
  Libraries/      Symbol libraries referenced by the schematic (bundled here so the project is self-contained)
Firmware/
  STM32G0_NFC_MCU/  Firmware for the NFC-side MCU (not started)
  RK3566_Host/      Host-side code for the RK3566 (not started)
Docs/
  Custom_Antenna.png  Hand-wound PCB loop antenna reference drawing
```

## Opening the project

1. Open `Hardware/Schematic/V2.kicad_pro` in KiCad 9.0.
2. The symbol libraries used by the LCD and eMMC parts live in `Hardware/Libraries/` and are referenced by `Hardware/Schematic/sym-lib-table` via relative (`${KIPRJMOD}`-based) paths, so the project should resolve them without any local path edits.
3. Run ERC (`kicad-cli sch erc` or the KiCad GUI) after any schematic change — sheets share nets, so the hierarchy is checked as one flat netlist.

## Status / next steps

- [x] Schematic: full net connectivity pass, power tree, decoupling, differential pairs verified
- [ ] PCB layout
- [ ] STM32G0 ↔ RK3566 communication firmware (next phase)
