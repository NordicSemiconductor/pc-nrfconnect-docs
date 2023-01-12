---
---

# API reference

## Minimal app

When developing an app for nRF Connect for Desktop, the minimal requirements are
that you create a JavaScript package with these two things:

1. A
   [`package.json` with the needed properties](./configuration#properties-in-packagejson).
2. The entry point of your package must expose a React component as default
   export.

This alone would be sufficient for the launcher to display and start your app.

## Normal app

While you could start with the mentioned minimal app and build the rest from
scratch, it will be easier to use the prepared components from
`pc-nrfconnect-shared` as it is demonstrated in
[`pc-nrfconnect-boilerplate`](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate)
and
[`pc-nrfconnect-rssi`](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi).

In the following sections the main building blocks from `pc-nrfconnect-shared`
are explained:

### Component: [`App`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/master/src/App/App.jsx)

[Example use in `pc-nrfconnect-rssi`.](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi/blob/b0216e3d0e50eec1b9149194564f47700e5b43c3/src/index.jsx#L46-L51)

Most apps will use the `App` component to create their main export. Visible to
the user it provides the general app look and feel:

- [Device selector](#component-deviceselector) on the top left
- Hidable sidebar on the left side
- Multiple panes, including an “About” pane, which can be switched in the
  navigation bar at the top and appear in the main area below it
- Hidable log viewer below the main area

For developer it provides a scaffolding to place their own components and a
prepared Redux store, which includes state and actions for the shared components
(note that you cannot easily use the Redux store when creating the `App`
element, as the store will be created and provided by the `App` component. But
you can use it about everywhere else, usually through React Redux using either
[hooks](https://react-redux.js.org/api/hooks) or
[the `connect` function](https://react-redux.js.org/api/connect).)

#### Properties

- `appReducer` (optional): A reducer function (`(state, action) => newState`).
  If your app wants to maintain a slice of Redux state itself, this is the root
  reducer to handle that. It will handle the slice of state under the name
  `app`.

- `deviceSelect`: The React element that appears in the upper left corner of the
  app. Apps usually utilise the component
  [`DeviceSelector`](#component-deviceselector) for this, as described below.

- `sidePanel`: The React element that appears in the hidable side panel on the
  left side. There is no shared component for this, as different side panel do
  not have enough in common.

- `panes`: Describes the panes that users can see in the main view. Each has a
  clickable name in the navigation at the top and when clicked, the pane is
  displayed in the main view of the app.

  The `panes` property is an array containing two element arrays: Each of the
  two element arrays has the name of the pane as the first element (which is
  displayed in the navigation bar) and the React component as the second
  element. E.g.
  `[['Connection Map', ConnectionMap], ['Server Setup', ServerSetup]]`.

### Component: [`DeviceSelector`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/master/src/Device/DeviceSelector/DeviceSelector.jsx)

[Example use in `pc-nrfconnect-rssi`.](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi/blob/b0216e3d0e50eec1b9149194564f47700e5b43c3/src/RssiDeviceSelect.jsx)

Most apps want to present a device selector to the users and this component is
the easiest way to achieve that. Configure it appropriately for the app and then
pass it to the `deviceSelect` property of the [`App` component](#component-app).

#### Properties

- `deviceListing`: Configures which device types to show in the device selector,
  e.g. whether to show only J-Link devices or also those just connected through
  a normal serial port. The object shape is the same as the
  [object passed to the constructor of `nrf-device-lister-js`](https://github.com/NordicSemiconductor/nrf-device-lister-js#usage-as-a-library).

- `deviceSetup` (optional): If your app requires devices to be set up with a
  certain firmware, use this property to specify how they are to be programmed.
  The
  [format of the configuration is describes in the project `nrf-device-setup-js`](https://github.com/NordicSemiconductor/nrf-device-setup-js#configuration).

  Note that the `nrf-device-setup-js` documentation also mentions
  `promiseConfirm` and `promiseChoice`, but apps usually do not have to provide
  these, as callbacks that show appropriate dialogs to the user are already
  implemented by `pc-nrfconnect-shared`.

  When configuring a device setup, you will usually provide a firmware file with
  your app. The easiest way to access that file is to use the function
  [`getAppFile`](#function-getappfile) as can also be
  [seen in `pc-nrfconnect-rssi`](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi/blob/b0216e3d0e50eec1b9149194564f47700e5b43c3/src/RssiDeviceSelect.jsx#L50).

- `onDeviceSelected` (optional): This callback is invoked when a device is
  selected by the user. The callback receives the selected device as a
  parameter.

- `releaseCurrentDevice` (optional): This callback is invoked before a device is
  about to be programmed. If no `deviceSetup` is provided, this callback will
  not be invoked.

- `onDeviceIsReady` (optional): This callback is invoked when programming a
  device is finished. The callback receives the programmed device as a
  parameter. If no `deviceSetup` is provided, this callback will not be invoked.

- `onDeviceDeselected` (optional): This callback is invoked when a selected
  device is again deselected. This may be caused by the user deselecting the
  device but also automatically if programming a device failed.

### Logging: [`logger`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/master/src/logging/index.js)

[Example use in `pc-nrfconnect-rssi`.](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi/blob/b0216e3d0e50eec1b9149194564f47700e5b43c3/src/actions.js#L109)

A logger using [winston](https://github.com/winstonjs/winston), which apps can
use to add log messages to the log below the main view.

### Component: [`Slider`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/master/src/Slider/Slider.jsx)

[Example use in `pc-nrfconnect-rssi`.](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi/blob/b0216e3d0e50eec1b9149194564f47700e5b43c3/src/SidePanel/Delay.jsx#L72-L78)

A slider component, especially useful for configurations in the side panel. This
slider supports a single or also multiple values, which is especially helpful if
users can select a range.

#### Properties

- `id` (optional): Specify the id if you need to refer it from the outside, e.g.
  from a `<label>` using the `for` attribute.
- `values`: An array with the current values of the slider. If the slider has
  just a single handle to select a single value, then make this an array with a
  single element.
- `range`: An object with two properties: `min` and `max`. In case you specify
  multiple values, this range is valid for all of them.
- `onChange`: An array of callback functions, one for every value. This array
  must have the same length as `values`. When a handle is dragged by the user,
  the callback function corresponding to that value is called with the new
  value. The callback must take care that the `value` is updated, so that the
  change is also visible in the UI. This callback is called multiple times if
  users keep on dragging the handle.
- `onChangeComplete` (optional): A single callback function that is called, when
  the user releases a handle. Use this, if you want to trigger an additional
  action when the user has selected a final value.

### Component: [`ConfirmationDialog`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/master/src/Dialog/ConfirmationDialog.jsx)

[Example use in `pc-nrfconnect-launcher`.](https://github.com/NordicSemiconductor/pc-nrfconnect-launcher/blob/5df8f17694fd9972bbb6eeb3893853ad6a9278db/src/launcher/components/ConfirmRemoveSourceDialog.jsx#L47-L55)

A component to show a simple confirmation dialog.

#### Properties

- `isVisible`: A boolean, whether the dialog should be visible at the moment.
- `title`: The title of the dialog, defaults to “Confirm”.
- `children` (optional): The content of the dialog.
- `text` (optional): Alternatively to the `children` you can just specify a
  simple text which will appear as the content of the dialog.
- `onOk`: Invoked when users confirm the dialog.
- `onCancel`: Invoked when users cancel the dialog.
- `okButtonText` (optional): Defaults to “OK”.
- `cancelButtonText` (optional): Defaults to “Cancel”.
- `isInProgress` (optional): A boolean, whether there is some processing going
  on at the moment. Being in progress shows a spinner and disables the buttons.
- `isOkButtonEnabled` (optional): A boolean, whether (besides being in progress)
  the ok button should be disabled for another, external reason.

### Function: [`getAppFile`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/master/src/appDirs.js#L67)

[Example use in `pc-nrfconnect-rssi`.](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi/blob/b0216e3d0e50eec1b9149194564f47700e5b43c3/src/RssiDeviceSelect.jsx#L50)

Use this function if the app needs to access a file bundled with it. Remember to
include these files in
[the `files` configuration of the app](./configuration#properties-in-packagejson).

### Display an error message

If you want to display an error message in an app, you can dispatch the action
from the action creator `showDialog` from the `ErrorDialogActions`:
[Example use in `pc-nrfconnect-launcher`.](https://github.com/NordicSemiconductor/pc-nrfconnect-launcher/blob/5d4eb36/src/launcher/actions/appsActions.js#L378)

You can also specify a second parameter, which lists possible error resolutions,
from which the users may choose one when being displayed the error:
[Example use in `pc-nrfconnect-launcher`.](https://github.com/NordicSemiconductor/pc-nrfconnect-launcher/blob/1f5ce26c95cae654ea8e0db60f47e696dca047f6/src/launcher/actions/autoUpdateActions.js#L205-L218)
