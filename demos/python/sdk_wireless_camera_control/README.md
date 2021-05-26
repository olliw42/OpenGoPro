# Open GoPro Python SDK

<img alt="GoPro Logo" src="https://raw.githubusercontent.com/gopro/OpenGoPro/main/logo.png?token=ASEL3UPDVM4EG6P53WZWG73AT4EOA" width="50%" style="max-width: 500px;"/>

[![Build and Test](https://github.com/gopro/OpenGoPro/actions/workflows/test_sdk_wireless_camera_control.yml/badge.svg)](https://github.com/gopro/OpenGoPro/actions/workflows/test_sdk_wireless_camera_control.yml)
[![Build Docs](https://github.com/gopro/OpenGoPro/actions/workflows/docs_sdk_wireless_camera_control.yml/badge.svg)](https://github.com/gopro/OpenGoPro/actions/workflows/docs_sdk_wireless_camera_control.yml)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

TODO  add PyPi badge once released on PyPi

This is a Python package that provides an interface for the user to exercise the Open GoPro Bluetooth Low
Energy (BLE) and Wi-Fi API's as well as install command line interfaces to take photos, videos, and view
the preview stream.

-   Free software: MIT license
-   Documentation: TODO Add link
-   View on Github: https://github.com/gopro/OpenGoPro/tree/main/demos/python/sdk_wireless_camera_control

## Documentation

This README is an overview of the package. Complete documentation can be found on Open GoPro: TODO ADD LINK.

## Installation

```console
    $ pip install open_gopro
```

## Features

-   Top-level GoPro class interface to use both BLE / WiFi
-   Cross-platform (tested on MacOS Big Sur, Windows 10, and Ubuntu 20.04)
    -   BLE implemented using `bleak <https://pypi.org/project/bleak/>`\_
    -   Wi-Fi controller provided in the Open GoPro package (loosely based on the `Wireless Library <https://pypi.org/project/wireless/>`\_ )
-   Supports all `Open GoPro API's <https://github.com/gopro/OpenGoPro>`\_
-   Automatically handles some required functionality:
    -   manage camera ready / encoding
    -   periodically sends keep alive signals
-   Includes demo scripts installed as command-line applications to show BLE and WiFi functionality

## Usage

To automatically connect to GoPro device via BLE and WiFI, take a video, and download all files:

```python
from open_gopro import GoProBLE, params

with GoPro() as gopro:
    gopro.ble_command.load_preset(params.Preset.CINEMATIC)
    gopro.ble_setting.resolution.set(params.Resolution.RES_5k)
    gopro.ble_setting.fps.set(params.FPS.FPS_30)
    gopro.ble_command.set_shutter(params.Shutter.ON)
    time.sleep(2) # Record for 2 seconds
    gopro.ble_command.set_shutter(params.Shutter.OFF)

    # Download all of the files from the camera
    media_list = [x["n"] for x in gopro.wifi_command.get_media_list()["media"][0]["fs"]]
    for file in media_list:
        gopro.wifi_command.download_file(camera_file=file)
```

And much more!

## Demos

> Note! These demos can be found on [Github](https://github.com/gopro/OpenGoPro/tree/main/demos/python/sdk_wireless_camera_control/open_gopro/demos)

Demos can be found in the installed package in the "demos" folder. They are installed as a CLI entrypoint
and can be run via:

```bash
$ gopro-photo
```

```bash
$ gopro-video
```

```bash
$ gopro-stream
```

For more information on each, try running with help as such:

```bash
$ gopro-photo --help

usage: gopro-photo [-h] [-i IDENTIFIER] [-l LOG] [-o OUTPUT]

Connect to a GoPro camera, take a photo, then download it.

optional arguments:
  -h, --help            show this help message and exit
  -i IDENTIFIER, --identifier IDENTIFIER
                        Last 4 digits of GoPro serial number, which is the last 4 digits of the default camera SSID. If not used, first discovered GoPro will be connected to
  -l LOG, --log LOG     Location to store detailed log
  -o OUTPUT, --output OUTPUT
                        Where to write the photo to. If not set, write to 'photo.jpg'
```