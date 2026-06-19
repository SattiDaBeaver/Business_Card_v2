# RISC-V Supercluster Business Card

A business-card-sized PCB featuring a miniature distributed compute cluster with one master MCU coordinating eight worker nodes over a shared UART bus, powered and programmed entirely through a PCB-edge USB-C connector.

Inspired by distributed systems, Network-on-Chip architectures, and AI accelerator fabrics — shrunk down to fit in a wallet.

## Architecture

- **1× CH32X035F7P6** (master) — RV32IMAC, native USB 2.0 + USB PD PHY, USB CDC terminal, command parser, task scheduler, packet router
- **8× CH32V006F4P6** (workers) — RV32EC, distributed task execution, per-node LED, packet forwarding

## Communication

Workers and master share a half-duplex, open-drain UART bus (`BUS_MOSI` / `BUS_MISO`) with pull-up resistors to prevent contention. A simple addressed packet protocol (STX/DST/SRC/CMD/LEN/PAYLOAD/CRC8/ETX) handles routing, pings, LED control, and distributed compute tasks.

The master can individually reset and reflash any worker over UART using its factory bootloader — no physical programmer access needed after initial setup.

## Hardware Features

- USB-C via PCB edge connector (0.8mm board, no connector part needed)
- Shared 3.3V rail (AMS1117-3.3 LDO)
- Per-node SWIO/SDI debug pads for direct programming/recovery
- Individual master-controlled NRST per worker node
- Exposed GPIO test point grid on the reverse side

## Programming

- **Master:** USB bootloader or SWIO via WCH-LinkE
- **Workers:** SWIO via WCH-LinkE (initial flash) or UART bootloader via master (field updates)
