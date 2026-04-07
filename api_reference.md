---
---

# API reference

This page documents API usage when developing apps for nRF Connect for Desktop.

## Minimal API requirements

When developing an app for nRF Connect for Desktop, you must create a JavaScript package with the following:

* A [`package.json` with the needed properties](./configuration#properties-in-packagejson).
* A React component exposed by the packages as the entry point for default export.

This alone is sufficient for the launcher to display and start your app.

## Recommended API changes

If you want to build a more feature-rich app, you need more than [Minimal API requirements](#minimal-api-requirements).
In addition, use the prepared components from `pc-nrfconnect-shared` as it is demonstrated in
[`pc-nrfconnect-boilerplate`](https://github.com/nordicsemi/pc-nrfconnect-boilerplate)
and [`pc-nrfconnect-rssi`](https://github.com/nordicsemi/pc-nrfconnect-rssi).

The following sections explain only the main building blocks from
`pc-nrfconnect-shared`. For guidance on most components exported in
[`src/index.ts`](https://github.com/nordicsemi/pc-nrfconnect-shared/blob/main/src/index.ts),
look at their TypeScript types and their existing uses in other
apps.

### `App` component

Take a look at the [example use in `pc-nrfconnect-rssi`](https://github.com/nordicsemi/pc-nrfconnect-rssi/blob/f18c63f728824bbd7609f42e61eb5156ed70b3e3/src/index.tsx#L22-L27) and at the [RSSI Viewer overview](https://docs.nordicsemi.com/bundle/nrf-connect-rssi-viewer/page/overview.html) while reading this section.

Most apps will use the [`App`](https://github.com/nordicsemi/pc-nrfconnect-shared/blob/master/src/App/App.tsx) component to create their main export. This component provides the general app look and feel, including:

- [Device selector](#component-deviceselector) on the top left
- Sidebar on the left side (that can be hidden)
- Multiple panes (tabs), including **About**, which can be switched by clicking on the
  navigation bar at the top
- Log viewer below the main area (that can be hidden)

The `App` component provides a scaffolding to place your own components, as well as a Redux store,
which includes state and actions for the shared components.

**Note:** In the React component that uses the `App` component, you cannot yet use
the Redux store. This is because the store will be created and provided by that `App` component.
However, you can already use it about everywhere else, usually through React Redux using
[hooks](https://react-redux.js.org/api/hooks) or
[the `connect` function](https://react-redux.js.org/api/connect).

#### Properties

The `App` component has the following properties:

- `appReducer` (optional) - A reducer function (`(state, action) => newState`).
  If your app wants to maintain a slice of Redux state itself, this is the root
  reducer to handle that. It handles the slice of state under the name `app`.

- `deviceSelect` (optional) - The React element that appears in the upper left
  corner of the app. Apps usually use the component
  [`DeviceSelector`](#component-deviceselector) for this, as described below.

- `sidePanel` (optional) - The React element that appears in the hidable side
  panel on the left side. There is no shared component for this, as different
  side panels do not have enough in common.

- `panes` - Describes the panes that users can see in the main view. Each has a
  clickable name in the navigation at the top and when clicked, the pane is
  displayed in the main view of the app.

  The `panes` property is an array containing `Pane` objects. These must contain
  properties `name` and `Main` (the React component shown in the pane) and
  optional properties `SidePanel` (if that pane has its own side panel),
  `preHidden`, and `preDisabled`.

- `showLogByDefault` (optional) - Boolean, defaults to `true`.
- `documentation` (optional) - Documentation references shown in the About pane.
- `feedbackCategories` (optional) - Feedback categories used in the feedback
  section in the About pane.
- `autoReselectByDefault` (optional) - Boolean, defaults to `false`. If a device
  is automatically selected again by the app, after being removed and attached
  again.

### `DeviceSelector` component

Take a look at the [example use in `pc-nrfconnect-rssi`](https://github.com/nordicsemi/pc-nrfconnect-rssi/blob/f18c63f728824bbd7609f42e61eb5156ed70b3e3/src/app/DeviceSelector.tsx) when reading this section.

Most apps want to present a device selector to the users. The [`DeviceSelector`](https://github.com/nordicsemi/pc-nrfconnect-shared/blob/master/src/Device/DeviceSelector/DeviceSelector.tsx) component is
the easiest way to achieve that. Configure it appropriately for the app and then
pass it to the `deviceSelect` property of the [`App` component](#component-app).

#### Properties

The `DeviceSelector` component has the following properties:

- `deviceListing` - Configures which device types to show in the device selector.
  For example, whether to show only J-Link devices or also those just connected through
  a normal serial port. The object shape
  [`DeviceTraits`](https://github.com/nordicsemi/pc-nrfconnect-shared/blob/main/nrfutil/device/common.ts#L77-L87)
  corresponds to
  [the device traits of nRF Util](https://docs.nordicsemi.com/bundle/nrfutil/page/nrfutil-device/guides/programming.html#device-traits).

- `deviceSetup` (optional) - If your app requires devices to be set up with a
  certain firmware, use this property to specify how they are to be programmed.

- `virtualDevices` (optional) - An array of strings. Virtual devices are always
  shown in the list of available devices.

- Callbacks for different lifetime events (all optional):

  - `onDeviceSelected`
  - `onDeviceDeselected`
  - `onDeviceConnected`
  - `onDeviceDisconnected`
  - `onDeviceIsReady`
  - `deviceFilter`
  - `virtualDevices`
  - `onVirtualDeviceSelected`
  - `onVirtualDeviceDeselected`

### `getAppFile` function

Take a look at the [example use in `pc-nrfconnect-rssi`](https://github.com/nordicsemi/pc-nrfconnect-rssi/blob/f18c63f728824bbd7609f42e61eb5156ed70b3e3/src/features/rssiDevice/rssiDeviceEffects.ts#L28) when reading this section.

Use the [`getAppFile`](https://github.com/nordicsemi/pc-nrfconnect-shared/blob/main/src/utils/appDirs.ts#L31) function if the app needs to access a file bundled with it. Remember to
include these files in
[the `files` configuration of the app](./configuration#properties-in-packagejson).
