nRF Connect apps are universal Node.js modules that exposes one or more of the following methods:

<table>
  <tbody>
    <tr>
      <th>Method</th>
      <th>Description and parameters</th>
    </tr>
    <tr>
      <td>
        <code>onInit</code>
      </td>
      <td>
        <p>Invoked right before the app is rendered for the first time.</p>
        <p>Parameters:</p>
        <table>
          <tbody>
            <tr>
              <td><code>dispatch</code></td>
              <td>The Redux dispatch function, which may be invoked to dispatch actions.</td>
            </tr>
            <tr>
              <td><code>getState</code></td>
              <td>The Redux getState function, which may be invoked to read the current app state.</td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td>
        <code>onReady</code>
      </td>
      <td>
        <p>Invoked right after the app has been rendered for the first time.</p>
        <p>Parameters:</p>
        <table>
          <tbody>
            <tr>
              <td><code>dispatch</code></td>
              <td>The Redux dispatch function, which may be invoked to dispatch actions.</td>
            </tr>
            <tr>
              <td><code>getState</code></td>
              <td>The Redux getState function, which may be invoked to read the current app state.</td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td>
        <code>decorateErrorDialog</code><br />
        <code>decorateFirmwareDialog</code><br />
        <code>decorateLogo</code><br />
        <code>decorateLogEntry</code><br />
        <code>decorateLogHeader</code><br />
        <code>decorateLogHeaderButton</code><br />
        <code>decorateLogViewer</code><br />
        <code>decorateMainMenu</code><br />
        <code>decorateMainView</code><br />
        <code>decorateNavBar</code><br />
        <code>decorateNavMenu</code><br />
        <code>decorateSerialPortSelector</code><br />
        <code>decorateSerialPortSelectorItem</code><br />
        <code>decorateSidePanel</code>
      </td>
      <td>
        <p>Invoked with the component that is to be decorated. Must return a Higher-Order Component (HOC). See [[examples|API reference#component decoration]].</p>
        <p>Parameters:</p>
        <table>
          <tbody>
            <tr>
              <td><code>Component</code></td>
              <td>The component to decorate.</td>
            </tr>
            <tr>
              <td><code>env</code></td>
              <td>The environment object. Currently only contains a reference to <code>React</code>.</td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td>
        <code>mapErrorDialogDispatch</code><br />
        <code>mapFirmwareDialogDispatch</code><br />
        <code>mapLogHeaderDispatch</code><br />
        <code>mapLogViewerDispatch</code><br />
        <code>mapMainMenuDispatch</code><br />
        <code>mapMainViewDispatch</code><br />
        <code>mapNavMenuDispatch</code><br />
        <code>mapSerialPortSelectorDispatch</code><br />
        <code>mapSidePanelDispatch</code><br />
      </td>
      <td>
        <p>Allows overriding props that are passed to the components. Receives <code>dispatch</code> and the original <code>props</code>, and must return a new map of props. See [[examples|API reference#dispatching actions]].</p>
        <p>Parameters:</p>
        <table>
          <tbody>
            <tr>
              <td><code>dispatch</code></td>
              <td>The Redux dispatch function.</td>
            </tr>
            <tr>
              <td><code>props</code></td>
              <td>The original props that were passed to the component.</td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td>
        <code>mapErrorDialogState</code><br />
        <code>mapFirmwareDialogState</code><br />
        <code>mapLogHeaderState</code><br />
        <code>mapLogViewerState</code><br />
        <code>mapMainMenuState</code><br />
        <code>mapMainViewState</code><br />
        <code>mapNavMenuState</code><br />
        <code>mapSerialPortSelectorState</code><br />
        <code>mapSidePanelState</code><br />
      </td>
      <td>
        <p>Allows overriding props that are passed to the components. Receives the <code>state</code> object and the original <code>props</code>, and must return a new map of props. See [[examples|API reference#passing information from state to components]].</p>
        <p>Parameters:</p>
        <table>
          <tbody>
            <tr>
              <td><code>state</code></td>
              <td>The Redux state object.</td>
            </tr>
            <tr>
              <td><code>props</code></td>
              <td>The original props that were passed to the component.</td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td>
        <code>middleware</code><br />
      </td>
      <td>
        <p>A custom <a href="http://redux.js.org/docs/advanced/Middleware.html">Redux middleware</a> that can intercept any action. The middleware is invoked after an action has been dispatched, but before it reaches the reducers.</p>
        <p>This is useful e.g. when the app wants to perform some asynchronous operation when an action is dispatched by core. Refer to the <a href="https://github.com/NordicSemiconductor/pc-nrfconnect-core/tree/master/lib/windows/app/actions">core actions</a> to see which actions may be intercepted. See [[examples|API reference#intercepting actions with middleware]].</p>
      </td>
    </tr>
    <tr>
      <td>
        <code>reduceApp</code><br />
      </td>
      <td>
        <p>Invoked when an action is dispatched. This is where the app can keep its own custom state. See [[examples|API reference#adding information to state]].</p>
        <p>Parameters:</p>
        <table>
          <tbody>
            <tr>
              <td><code>state</code></td>
              <td>The part of the state that the reducer is concerned with.</td>
            </tr>
            <tr>
              <td><code>action</code></td>
              <td>The action that was dispatched.</td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
  </tbody>
</table>

## Example usage

### Component decoration

Decoration allows the app to render custom components or override props that are passed to components. Also, if a component is not relevant for the app, it can simply choose not to render it.

#### Rendering a custom component

Using a [functional component](https://facebook.github.io/react/docs/components-and-props.html#functional-and-class-components):

```
export function decorateNavMenu() {
    return props => (
        <CustomNavMenu {...props} />
    );
}
```

The same thing can be done using a class component, but it is a little more verbose:

```
export function decorateNavMenu() {
    return class extends React.Component {
        render() {
            return (
                <CustomNavMenu {...props} />
            );
        }
    };
}
```

#### Overriding props that are passed to components

```
export function decorateNavMenu(NavMenu) {
    return props => (
        <NavMenu
            {...props}
            menuItems={[
                { id: 0, text: 'Search', iconClass: 'icon-search' },
            ]}
        />
    );
}
```

#### Removing a component

```
export function decorateNavMenu() {
    return () => <div />;
}
```

### Dispatching actions

When the user clicks a button or types some text inside a component, the app will typically want to update information in state. This is done by dispatching actions. The actions are then processed by the reducers, which is responsible for setting information in state.

To dispatch an action when a button is clicked, the app should create a function that dispatches the action, and pass that function to the component as a prop. Apps can do this by implementing the `map<ComponentName>Dispatch` methods.

For example, if an action should be dispatched when clicking a button in the `SidePanel`, the app can implement `mapSidePanelDispatch`:

```
export function mapSidePanelDispatch(dispatch, props) {
    return {
        ...props,
        onButtonClicked: () => dispatch({
            type: 'SIDE_PANEL_BUTTON_CLICKED'
        }),
    };
}
```

The `SidePanel` can then assign this function to the button's `onClick` property:

```
export function decorateSidePanel(SidePanel) {
    return props => (
        <SidePanel>
            <button onClick={props.onButtonClicked}>Button</button>
        </SidePanel>
    );
}
```

Instead of passing an action object to `dispatch`, the app can also pass a function. This is useful when the app needs to perform an asynchronous operation. Refer to the [redux-thunk](https://github.com/gaearon/redux-thunk) documentation to see how this is done.

### Passing information from state to components

The nRF Connect core keeps its state under `state.core`. Refer to the [coreReducer](https://github.com/NordicSemiconductor/pc-nrfconnect-core/blob/master/lib/windows/app/reducers/coreReducer.js) to see what information may be found there. The app can maintain its own state under `state.app` by implementing the `reduceApp` method.

For example, the selected navigation menu item is kept in `state.core.navMenu.selectedItemId`. To pass it to the `MainView`, implement `mapMainViewState` as shown below:

```
export function mapMainViewState(state, props) {
    return {
        ...props,
        selectedMenuItemId: state.core.navMenu.selectedItemId,
    };
}
```

The `MainView` will now receive a `selectedMenuItemId` prop:

```
export function decorateMainView(MainView) {
    return props => (
        <MainView>
            <p>Selected menu item is { props.selectedMenuItemId }</p>
        </MainView>
    );
}
```

### Intercepting actions with middleware

By implementing a [Redux middleware](http://redux.js.org/docs/advanced/Middleware.html), apps can intercept or act upon actions before they are received by the reducers. This is useful for changing or expanding on the default nRF Connect behavior. Refer to the [core actions](https://github.com/NordicSemiconductor/pc-nrfconnect-core/tree/master/lib/windows/app/actions) to see the list of actions that may pass through the middleware.

A common scenario is that the app should open serial port when a port has been selected, and close the port when is has been deselected. This can be done using middleware:

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

Note that the final line of the middleware calls `next(action)`. This passes the action on to the next middleware in the chain, and finally the action is received by the reducers. Also note that we can dispatch new actions from middleware using `store.dispatch()`, and there is also a `store.getState()` function for reading information from state.

### Adding information to state

The app can maintain its own state under `state.app` by implementing the `reduceApp` method. This can either be a single reducer function, or multiple nested reducers. All actions that are dispatched will be received by the reducer(s).

#### Single reducer function

```
const initialState = {
    clickCount: 0,
};

export function reduceApp(state = initialState, action) {
    switch (action.type) {
        case 'SIDE_PANEL_BUTTON_CLICKED':
            return {
                ...state,
                clickCount: state.clickCount + 1,
            };
        default:
            return state;
    }
}
```

#### Multiple reducer functions

Multiple reducer functions can be added using the Redux [combineReducers](http://redux.js.org/docs/api/combineReducers.html) function:

```
import { combineReducers } from 'redux';

const initialFooState = {};
const initialBarState = {};

function foo(state = initialFooState, action) {
    /* reducer implementation */
}

function bar(state = initialBarState, action) {
    /* reducer implementation */
}

export const reduceApp = combineReducers({
    foo,
    bar,
});
```