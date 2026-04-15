# Loogootee Manufacturing, Inc. — Training Rack Simulation

**42U Multi-Platform Automation and IIoT Integration Training Platform**

---

## Overview

A purpose-built industrial automation training platform simulating a two-site precision metal stamping operation. The project covers multi-vendor PLC programming, IIoT architecture, data infrastructure, and predictive maintenance — running 11 controllers across 5 vendors, 14 communication protocols, and a complete five-layer IIoT data stack.

The simulation company is named Loogootee Manufacturing, Inc. — a fictional precision metal stamping operation with facilities in Loogootee, Illinois (Site 1) and a remote packaging site (Site 2).

---

## Platform Summary

| | |
|---|---|
| **Controllers** | 11 across 5 vendors |
| **Vendors** | Allen-Bradley · Siemens · Beckhoff · Opto22 · Wago |
| **Protocols** | 14 — EtherNet/IP · PROFINET · PROFIBUS · EtherCAT · OPC-UA · Modbus TCP/RTU · DF1 · DH-485 · CIP · Sparkplug B · MQTT · IO-Link · CAN · CANopen |
| **IIoT Stack** | Mosquitto · Node-RED · Ignition · Telegraf · InfluxDB · Grafana |
| **Data Standard** | Sparkplug B · ISA-95 tag naming · Unified Namespace |
| **Rack** | 42U open frame · wheeled |
| **Simulation** | Two-site precision metal stamping — Loogootee Mfg., Inc. |

## Rack Photos

<p align="center">
  <img src="06_rack_documentation/rack_photos/IMG_5098.jpeg" width="45%"/>
  <img src="06_rack_documentation/rack_photos/IMG_5101.jpeg" width="45%"/>
</p>

---

## Controllers

### Site 1 — Loogootee, Illinois

| Controller | Platform | Simulation Role | Protocol |
|---|---|---|---|
| MicroLogix 1400 | Allen-Bradley | Stamping Press #2 | EtherNet/IP via MGate |
| MicroLogix 1000 | Allen-Bradley | Stamping Press #1 | DF1 via MGate EIP3170 |
| Micro850 | Allen-Bradley | Palletizer | EtherNet/IP |
| CompactLogix L24ER | Allen-Bradley | Wash Process | EtherNet/IP |
| GuardLogix + Point I/O | Allen-Bradley | Wash Safety | EtherNet/IP |
| S7-1200 #1 + ET200SP | Siemens | Inspection | PROFINET |
| S7-1200 #2 + ET200SP | Siemens | Conveyor | PROFINET / PROFIBUS |
| Beckhoff CX5120 + EK1100 | Beckhoff | High Speed Forming | EtherCAT / OPC-UA |
| groov EPIC-PR1 | Opto22 | Line Supervisor / Edge Device | Native MQTT / Sparkplug B |

### Site 2 — Remote

| Controller | Platform | Simulation Role | Protocol |
|---|---|---|---|
| Wago PFC200 750-8206 | Wago | Packaging | Native MQTT / Sparkplug B |
| Raspberry Pi 4B | Raspberry Pi | MQTT Broker Node | Mosquitto 2.0.21 |

---

## IIoT Stack Architecture

Five-layer stack. Each layer has a single non-overlapping responsibility.

```
OT Layer        →  11 PLC controllers — native protocols
Transport       →  EPIC Node-RED · Wago · Mosquitto broker · Node-RED
Intelligence    →  Ignition SCADA — single calculation engine — UNS intelligence
Historian       →  Telegraf → InfluxDB OSS — single source of historical truth
Visualization   →  Grafana OSS — read only — queries InfluxDB only
```

**Three governing principles:**
- One broker — Mosquitto is the real-time transport spine. Current state only. No processing.
- One calculation engine — Ignition calculates all derived metrics once and publishes back to broker. No consumer calculates independently.
- One historian — Telegraf captures everything that touches the broker into InfluxDB. Grafana reads InfluxDB only.

Infrastructure runs on an AOOSTAR WTR Max (Ryzen 7 PRO 8845HS · 32GB DDR5) running Proxmox VE with LXC containers for each stack component.

---

## Phased Development Plan

| Phase | Focus | Status |
|---|---|---|
| 1 | All 11 controllers programmed — simulation running — data to UNS | In Progress |
| 2 | Grafana dashboards · InfluxDB historian · OEE visible in real time | Planned |
| 3 | Two-rack split — OT rack and IIoT rack | Planned |
| 4 | IO-Link smart sensors — IFM AL1590 master — pump module | Planned |
| 5 | MaintainX CMMS integration — predictive maintenance — auto work orders | Planned |
| 6 | WireGuard VPN · webcam · remote multi-site demonstration | Planned |

---

## Repository Structure

```
loogootee-mfg-training-simulation/
│
├── 03_Architecture/
│   ├── IIoT_Architecture/           # Data layer architecture · ADR
│   ├── Infrastructure_Architecture/ # NAS · Proxmox · network
│   └── UNS_Architecture/            # UNS diagram · topology
│
├── 04_Program_Specifications/       # PLC program specs per controller
│
├── 05_Programs/                     # PLC program files
│   ├── Allen_Bradley/
│   ├── Siemens/
│   ├── Beckhoff/
│   ├── Opto22/
│   └── Wago/
│
├── 06_Rack_Documentation/           # Hardware register · phased plan · operations
│
├── 07_Standards/                    # ISA-95 naming standard · UNS subscription register
│
├── 08_IO_Link_Implementation/       # IFM AL1590 · pump module · vendor docs
│
├── 09_MaintainX_Implementation/     # CMMS integration concept · API architecture
│
└── README.md
```

---

## Document Family

| Document | Description |
|---|---|
| IIoT Architecture Decision Record | Nine ADRs — architectural decisions with rationale and rejected alternatives |
| IIoT Data Layer Architecture | Hardware inventory · software stack · data flow · credential structure |
| ISA-95 Tag Naming Standard | Two-tier naming convention — Tier 1 RSLogix 500 · Tier 2 UNS |
| UNS Subscription Register | Full topic hierarchy for both sites |
| Rack Phased Development Plan | Six-phase development roadmap with status |
| NAS Infrastructure Implementation | AOOSTAR platform · Proxmox · VM allocation |
| IO-Link Pump Module Concept | Three integration patterns · predictive maintenance demonstration |
| MaintainX Implementation Concept | API architecture · Node-RED flows · work order automation |
| MicroLogix 1000 Program Spec | Stamping press program specification |
| Pi Broker Configuration | Mosquitto setup · credentials · security · recovery procedure |

---

## Learning Objectives

This project covers the following areas across its six phases:

- Multi-vendor PLC programming — Allen-Bradley, Siemens, Beckhoff, Opto22, Wago
- Industrial communication protocols — EtherNet/IP, PROFINET, PROFIBUS, EtherCAT, OPC-UA, Modbus, DF1
- IIoT architecture — Unified Namespace, Sparkplug B, ISA-95 tag naming
- Data infrastructure — MQTT broker, time series historian, dashboards
- Edge computing — Node-RED protocol bridging, data contextualization
- IO-Link smart sensors — device integration, process monitoring
- Predictive maintenance — vibration baseline, threshold detection, automated work orders
- Remote access — VPN, secure tunnel, multi-site demonstration

---

*Loogootee Manufacturing, Inc. is a fictional company created for training and demonstration purposes.*
