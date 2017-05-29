## List of modules

nRF Connect includes several modules that apps can import:

<table>
  <tbody>
    <tr>
      <th>Module name</th>
      <th>Description</th>
    </tr>
    <tr>
      <td>
        <code>nrfconnect/core</code>
      </td>
      <td>
        <p>Functions and properties for interacting with nRF Connect:</p>
        <table>
        <tr>
          <td><code>getAppDir</code></td>
          <td><p>Function returning absolute path to where the app is loaded from.</p></td>
        </tr>
        <tr>
          <td><code>getUserDataDir</code></td>
          <td><p>Function returning absolute path to the data directory of the current user.</p></td>
        </tr>
        <tr>
          <td><code>logger</code></td>
          <td><p>The application logger, which is a Winston logger instance. Log messages will appear in the log viewer. See the <a href="https://github.com/winstonjs/winston">Winston documentation</a> on Github.</p></td>
        </tr>
        </table>
      </td>
    </tr>
    <tr>
      <td>
        <code>nrfconnect/programming</code>
      </td>
      <td>
        <p>Programming operations. Refer to the <a href="https://github.com/NordicSemiconductor/pc-nrfconnect-core/blob/master/lib/api/programming/index.js">programming API source code</a>.</p>
      </td>
    </tr>
    <tr>
      <td>
        <code>electron</code>
      </td>
      <td>
        <p>The electron API for the <i>renderer</i> process. Refer to the <a href="https://electron.atom.io/docs/api/">electron documentation</a>.</p>
      </td>
    </tr>
    <tr>
      <td>
        <code>pc-ble-driver-js</code>
      </td>
      <td>
        <p>Bluetooth® low energy driver. Refer to the <a href="https://github.com/NordicSemiconductor/pc-ble-driver-js">pc-ble-driver-js repository</a> on Github.</p>
      </td>
    </tr>
    <tr>
      <td>
        <code>serialport</code>
      </td>
      <td>
        <p>Node.js serialport library. Refer to the <a href="https://github.com/EmergingTechnologyAdvisors/node-serialport">serialport documentation</a> on Github.</p>
      </td>
    </tr>
  </tbody>
</table>

## Example

The modules are imported using `import` or `require()`, e.g.:

```
import SerialPort from 'serialport';
import { logger } from 'nrfconnect/core';

const port = new SerialPort('/dev/ttyACM0', err => {
  if (err) {
    logger.error(err.message);
  } else {
    logger.info('Port is open');
  }
});
```