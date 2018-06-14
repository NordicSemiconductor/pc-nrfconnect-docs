## Introduction

The device selector detects and displays available devices based on the app's `config.selectorTraits` configuration. It is up to the different apps to implement what should happen when selecting a device. Typically, you would want to open the device when it is selected - using f.ex. the serialport library or pc-ble-driver-js. This example shows how to open the selected device using the serialport library.

## Device actions

First, create a new app project as described in the [[getting started]] guide and launch the app from nRF Connect. During development it is often useful to open [[Chrome developer tools|https://developer.chrome.com/devtools]] by pressing Ctrl+Shift+I (Windows/Linux) or Cmd+Alt+I (macOS). All actions that are dispatched at runtime are logged to the developer tools console, so that you can see what is happening under the hood.

Verify that the app has the following `config.selectorTraits`, which will show devices with a serial port in the device selector:

```
export const config = {
    selectorTraits: {
        serialport: true,
    },
};
```

Now, click the device selector and select a device. In the console you will see several actions appearing. One of these is `DEVICE_SELECTED`. If you expand it, you will see something like this:

```
action
  Object
    device: Object
      serialNumber: 000680551615,
      serialport: Object
        comName: "/dev/ttyACM0",
        manufacturer: "SEGGER",
        productId: "0105",
        serialNumber: 000680551615,
        vendorId: "1366"
    type: "DEVICE_SELECTED"
```

Similarly, if you close the device, you will see the the `DEVICE_DESELECTED` action appearing in the console:

```
action
  Object
    type: "DEVICE_DESELECTED"
```

Apps can listen to actions and implement custom behavior when a certain action is dispatched. Now we know which actions to listen to, plus the information that can be pulled out from the actions. To see all actions that are available, see [[lib/windows/app/actions|https://github.com/NordicSemiconductor/pc-nrfconnect-core/tree/master/lib/windows/app/actions]].

## Acting upon actions with middleware

By implementing [[middleware|API-reference#intercepting-actions-with-middleware]], apps can intercept or act upon actions before they are received by the [[reducers|https://github.com/NordicSemiconductor/pc-nrfconnect-core/tree/master/lib/windows/app/reducers]]. Middleware may seem a bit odd at first, and you can read more about it in the [[Redux documentation|http://redux.js.org/docs/advanced/Middleware.html]]. However, you do not need to know all the details to use it. A middleware function looks like this:

```
function middleware(store) {
    return next => action => {
        if (action.type === 'SOME_ACTION') {
            // Implement custom behavior here. Can read state
            // or dispatch new actions by using store.getState
            // or store.dispatch, e.g:
            //
            // const value = store.getState().value;
            // store.dispatch({ type: 'SOME_OTHER_ACTION' });
        }

        // Pass the action on, so that it is eventually received
        // by the reducers. Can also be omitted for certain
        // actions if you want to disable the default behavior.
        next(action);
    };
}
```

## Opening port with the serialport library

Now that we know which actions to use and how to use middleware, we can implement opening and closing the serial port of the selected device. We listen for the `DEVICE_SELECTED` and `DEVICE_DESELECTED` action types, pull out the `comName` from the action, and open/close the port with the serialport library.

A full example of how to do this in the `index.jsx` file of the app can be seen below. Here we are also logging the status out to the log viewer.

```
import SerialPort from 'serialport';
import { logger } from 'nrfconnect/core';

const options = {
    baudRate: 1000000,
};

let port;

export const config = {
    selectorTraits: {
        serialport: true,
    },
};

export function middleware(store) {
    return next => action => {
        if (action.type === 'DEVICE_SELECTED') {
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