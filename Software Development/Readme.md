# SOFTWARE DEVELOPMENT

In this section, you can examine how the software side of the project works.

## Table of Contents

- [Introduction](#introduction)
- [How It Works](#how-it-works)

## How It Works ?

The entire control of the plotter machine is managed by a **Raspberry Pi Zero 2W** board and a mobile application that controls it. Two main programs run on the Raspberry Pi: the `server` and the `hardware control program (the hardware control program is named Raspi Hardware Controller)`. These programs are run by linux services that start themselves after the **Raspberry Pi OS** boots.

### Raspberry Pi Zero 2W Side

The folder structure for the two main programs running on the Raspberry Pi is as follows:

```bash
|___ PlotterController
     |
     |___ plotter-express-server
     |
     |___ embedded
```

- The `plotter-express-server` directory contains the server program that facilitates communication between the `mobile application` and the `Raspi Hardware Controller`. This server is written with `express.js`.

- The `embedded` directory contains the program written in `cpp` that controls the hardware (stepper motors, servo motors, limit switches...) which I have named `Raspi Hardware Controller`.

#### Linux Services

The above two programs are automatically started by Linux services after the **Raspberry Pi OS** (installed on the Raspberry Pi Zero 2W) boots. These Linux services are as follows:

- The following linux service runs the server written in `express.js`.

```sh
[Unit]
Description=Server Controller Service
After=multi-user.target

[Service]
Type=idle
ExecStart=/usr/bin/node /home/oxygen/Desktop/PlotterController/plotter-express-server/index.js

[Install]
WantedBy=multi-user.target
```

- The other Linux service runs the `Raspi Hardware Controller`.

```sh
[Unit]
Description=Raspi Hardware Controller
After=multi-user.target

[Service]
Type=idle
ExecStart=/home/oxygen/Desktop/PlotterController/embedded/run

[Install]
WantedBy=multi-user.target
```

#### Server

Server program project link: https://github.com/antinucleus/plotter-express-server

#### Hardware Controller Program

Raspi hardware controller project link: https://github.com/antinucleus/plotter-embedded-controller

### Mobile Application Side

The mobile application is built with `expo`. With the mobile application, the plotter machine can be used in manual mode or to print desired image files. You can click on the following link to access the mobile application project.

Mobile application project link: https://github.com/antinucleus/plotter-mobile-application
