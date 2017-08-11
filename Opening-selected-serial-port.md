## Introduction

By default, the serial port selector detects and displays available ports. It is up to the different apps to implement what should happen when selecting a port. Typically, you would want to open the port when it is selected - either using the serialport library or pc-ble-driver-js. This example shows how to open the selected port using the serialport library.

## Serial port actions

First, create a new app project as described in the [[getting started]] guide and launch the app from nRF Connect. During development it is often useful to open [[Chrome developer tools|https://developer.chrome.com/devtools]] by pressing Ctrl+Shift+I (Windows/Linux) or Cmd+Alt+I (macOS). All actions that are dispatched at runtime are logged to the developer tools console, so that you can see what is happening under the hood.

Now, click the serial port selector and select a port. In the console you will see several actions appearing. One of these is `SERIAL_PORT_SELECTED`. If you expand it, you will see something like this:

```
action
  Object
    port: Record
      _map: Map
        comName: "/dev/ttyACM0",
        manufacturer: "SEGGER",
        productId: "0x0105",
        serialNumber: 680551615,
        vendorId: "0x1366"
    type: "SERIAL_PORT_SELECTED"
```

Similarly, if you close the serial port, you will see the the `SERIAL_PORT_DESELECTED` action appearing in the console:

```
action
  Object
    type: "SERIAL_PORT_DESELECTED"
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

Now that we know which actions to use and how to use middleware, we can implement opening and closing the selected serial port. We listen for the `SERIAL_PORT_SELECTED` and `SERIAL_PORT_DESELECTED` action types, pull out the `comName` from the action, and open/close the port with the serialport library.

A full example of how to do this in the `index.jsx` file of the app can be seen below. Here we are also logging the status out to the log viewer.

```
import SerialPort from 'serialport';
import { logger } from 'nrfconnect/core';

const options = {
    baudRate: 1000000,
};

let port;

export function middleware(store) {
    return next => action => {
        if (action.type === 'SERIAL_PORT_SELECTED') {
            port = new SerialPort(action.port.comName, options, err => {
                if (err) {
                    logger.error(`Failed to open port: ${err.message}`);
                } else {
                    logger.info('Port is open');
                }
            });
        } else if (action.type === 'SERIAL_PORT_DESELECTED') {
            port.close(() => {
                logger.info('Port is closed');
            });
        }
        next(action);
    };
}
```