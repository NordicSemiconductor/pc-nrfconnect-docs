---
---

# Programming selected device

## Introduction

In the [opening selected device](./opening_selected_device) example we described
how to open a device's serial port when the device is selected. This works fine
when we do not require any setup of the device before opening it. However, in
many cases apps require a specific firmware to be programmed on the device
before it can be opened. This example describes how to set up (program) a device
using the built-in device setup feature in nRF Connect.

## Configuration

The app can configure how a device should be set up by adding a
`config.deviceSetup` property. nRF Connect will pass this to
[nrf-device-setup-js](https://github.com/NordicSemiconductor/nrf-device-setup-js),
which performs the actual firmware validation and programming. Refer to the
documentation in nrf-device-setup-js for details on this configuration format.

The app should specify one or both of `dfu` or `jprog` depending on which types
of devices the app needs to support. Note that the nrf-device-setup-js
documentation also mentions `promiseConfirm` and `promiseChoice`, but the app
does not have to provide these, as they are mapped to UI dialogs by the nRF
Connect core framework by default.

## Device setup actions

When `config.deviceSetup` is specified by the app, two actions will be
dispatched in nRF Connect when selecting a device:

- `DEVICE_SELECTED`: Dispatched when the device is selected in the device
  selector, and includes the selected device object, ref.
  [nrf-device-lister-js](https://github.com/NordicSemiconductor/nrf-device-lister-js).
- `DEVICE_SETUP_COMPLETE`: Dispatched when the device has been successfully set
  up according to the `config.deviceSetup` configuration, or if the device is
  already set up. Includes the device object, ref.
  [nrf-device-lister-js](https://github.com/NordicSemiconductor/nrf-device-lister-js).

If something fails during device setup, an error is logged to the log viewer,
and a `DEVICE_DESELECTED` action is dispatched.

## Opening a device after it has been set up

In the [opening selected device](./opening_selected_device) example, we
described how to act upon actions using middleware, and opening a device when
`DEVICE_SELECTED` is dispatched. Now, we take this a step further and wait until
the device has been set up before opening it.

A full example of how to do this for PCA10040 can be seen below. When a PCA10040
device is selected, this will first check if `rssi-fw-1.0.0` is present on
memory location `0x2000`. If not, nRF Connect will open a dialog asking the user
to confirm programming of the device. If the user confirms, then
`fw/rssi-10040.hex` will be written to the device, and a `DEVICE_SETUP_COMPLETE`
action will be dispatched. Finally, the app listens to this action and opens the
serial port of the device.

```
import path from 'path';
import SerialPort from 'serialport';
import { logger, getAppDir } from 'nrfconnect/core';

const options = {
    baudRate: 1000000,
};

let port;

export const config = {
    selectorTraits: {
        serialport: true,
        jlink: true,
    },
    deviceSetup: {
        jprog: {
            // Firmware configuration for pca10040
            nrf52: {
                // Path to the firmware to program
                fw: path.resolve(getAppDir(), 'fw/rssi-10040.hex'),
                // The memory location where we expect to find the fwVersion data
                fwIdAddress: 0x2000,
                // The data we expect to find on fwIdAddress
                fwVersion: 'rssi-fw-1.0.0',
            },
        },
    },
};

export function middleware(store) {
    return next => action => {
        if (action.type === 'DEVICE_SELECTED') {
            logger.info('Validating firmware for the selected device');
        } else if (action.type === 'DEVICE_SETUP_COMPLETE') {
            const device = action.device;
            port = new SerialPort(device.serialport.comName, options, err => {
                if (err) {
                    logger.error(`Failed to open port: ${err.message}`);
                } else {
                    logger.info('Port is open');
                }
            });
        } else if (action.type === 'DEVICE_DESELECTED') {
            port.close(() => {
                logger.info('Port is closed');
            });
        }
        next(action);
    };
}
```
