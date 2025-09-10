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

For guidance on most components exported in
[`src/index.ts`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/main/src/index.ts),
it is best to look at their TypeScript types and their existing uses in other
apps. The following sections explain only the main building blocks from
`pc-nrfconnect-shared`:

### Component: [`App`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/master/src/App/App.tsx)

[Example use in `pc-nrfconnect-rssi`.](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi/blob/f18c63f728824bbd7609f42e61eb5156ed70b3e3/src/index.tsx#L22-L27)

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

- `deviceSelect` (optional): The React element that appears in the upper left
  corner of the app. Apps usually utilise the component
  [`DeviceSelector`](#component-deviceselector) for this, as described below.

- `sidePanel` (optional): The React element that appears in the hidable side
  panel on the left side. There is no shared component for this, as different
  side panel do not have enough in common.

- `panes`: Describes the panes that users can see in the main view. Each has a
  clickable name in the navigation at the top and when clicked, the pane is
  displayed in the main view of the app.

  The `panes` property is an array containing `Pane` objects: These must contain
  properties `name` and `Main` (the React component shown in the pane) and
  optional properties `SidePanel` (if that pane has its own side panel),
  `preHidden`, and `preDisabled`.

- `showLogByDefault` (optional): Boolean, defaults to `true`.
- `documentation` (optional): Documentation references shown in the About pane.
- `feedbackCategories` (optional): Feedback categories used in the feedback
  section in the About pane.
- `autoReselectByDefault` (optional): Boolean, defaults to `false`. If a device
  is automatically selected again by the app, after being removed and attached
  again.

### Component: [`DeviceSelector`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/master/src/Device/DeviceSelector/DeviceSelector.tsx)

[Example use in `pc-nrfconnect-rssi`.](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi/blob/f18c63f728824bbd7609f42e61eb5156ed70b3e3/src/app/DeviceSelector.tsx)

Most apps want to present a device selector to the users and this component is
the easiest way to achieve that. Configure it appropriately for the app and then
pass it to the `deviceSelect` property of the [`App` component](#component-app).

#### Properties

- `deviceListing`: Configures which device types to show in the device selector,
  e.g. whether to show only J-Link devices or also those just connected through
  a normal serial port. The object shape
  [`DeviceTraits`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/main/nrfutil/device/common.ts#L77-L87)
  corresponds to
  [the device traits of nRF Util](https://docs.nordicsemi.com/bundle/nrfutil/page/nrfutil-device/guides/programming.html#device-traits).

- `deviceSetup` (optional): If your app requires devices to be set up with a
  certain firmware, use this property to specify how they are to be programmed.

- `virtualDevices` (optional): An array of strings. Virtual devices are always
  shown in the list of available devices.

- Callbacks, all optional, for different lifetime events:

  - `onDeviceSelected`
  - `onDeviceDeselected`
  - `onDeviceConnected`
  - `onDeviceDisconnected`
  - `onDeviceIsReady`
  - `deviceFilter`
  - `virtualDevices`
  - `onVirtualDeviceSelected`
  - `onVirtualDeviceDeselected`

### Function: [`getAppFile`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/main/src/utils/appDirs.ts#L31)

[Example use in `pc-nrfconnect-rssi`.](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi/blob/f18c63f728824bbd7609f42e61eb5156ed70b3e3/src/features/rssiDevice/rssiDeviceEffects.ts#L28)

Use this function if the app needs to access a file bundled with it. Remember to
include these files in
[the `files` configuration of the app](./configuration#properties-in-packagejson).
