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
        <p>Invoked with the component that is to be decorated. Must return a Higher-Order Component (HOC).</p>
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
        <p>Allows overriding props that are passed to the components. Receives <code>dispatch</code> and the original <code>props</code>, and must return a new map of props.</p>
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
        <p>Allows overriding props that are passed to the components. Receives the <code>state</code> object and the original <code>props</code>, and must return a new map of props.</p>
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
        <p>This is useful e.g. when the app wants to perform some asynchronous operation when an action is dispatched by core. Refer to the <a href="https://github.com/NordicSemiconductor/pc-nrfconnect-core/tree/master/lib/windows/app/actions">core actions</a> to see which actions may be intercepted.</p>
      </td>
    </tr>
    <tr>
      <td>
        <code>reduceApp</code><br />
      </td>
      <td>
        <p>Invoked when an action is dispatched. This is where the app can keep its own custom state. Multiple reducers may be nested below this by using `combineReducers` from react-redux.</p>
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