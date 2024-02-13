
# Project Title

## Overview

This project utilizes the DWM1001-dev board from Decawave to measure the distance of a tag from four anchors.

The DWM1001 module is a Ultra Wideband (UWB) and Bluetooth hardware based on DecaWave's DW1000 IC and Nordic Semiconductor nrF52832 SoC. It allows to build a scalable Two-Way-Ranging (TWR) RTLS systems with up to thousands of tags.

## Getting Started

These instructions will get your copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

List all the necessary hardware and software prerequisites required for this project.

- DW10001-DEV dev board with UWB chip and Nordic nrf52832_xxaa microcontroller
- SEGGER Embedded Studio for Arm V5.64 (64-bit)
- Windows 10/11 64bit

### Installation

#### Step 1: Clone the Repository

Start by cloning this repository to your local machine. This repository contains the necessary source code and makefile for the project.

```
git clone https://github.com/ChenAshkenazi13/Dwm1001-dev-Indoor-Positioning.git
```

#### Step 2: Base Project
This project is based the following Github repos:
- https://github.com/jonathanrjpereira/DWM1001-Real-Time-Localization-System

### Setting Up the Development Environment

#### Creating a New Project

To include the make file necessary for the code to run, follow these steps:

1. Open your development environment.
2. Navigate to `File` -> `New Project`.
3. Select `A C/C++ executable for a (your core type)`, using both internal tools and external GNU tools.

For more detailed explanation see this: https://forum.segger.com/index.php/Thread/4726-SOLVED-Importing-ARMGCC-Makefile/

### Configuration

#### Initializing an Anchor

To initialize a device as an anchor:

1. Open the `main.c` file.
2. Set the `NodeType` to `Anchor`
```
  /* Set the node type to either Anchor (0) or Tag (1) */
  enum nodeType node_type = Anchor;

```
4. Configure at least one anchor as the initiator by setting `set_a_cfg.initiator = 1`.

#### Initializing a Tag

To initialize a device as a tag:

1. Open the `main.c` file.
2. Set the `NodeType` to `Tag`.
```
  /* Set the node type to either Anchor (0) or Tag (1) */
  enum nodeType node_type = Tag;

```

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Acknowledgments
