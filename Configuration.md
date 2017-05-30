## package.json

The following package.json properties are especially relevant for nRF Connect apps:

| Property | Description |
| -------- | ----------- |
| `name`   | The name used to identify the app on npmjs.org. The recommended naming convention is `pc-nrfconnect-<appname>`, as this makes it easier to identify it as an nRF Connect app. |
| `displayName` | The name shown in the nRF Connect launcher and in the app's window title. |
| `version` | The current version of the app. Should be a valid [semver string](http://semver.org/). The nRF Connect launcher will display an upgrade button when new versions of the app are made available on the npm registry. |
| `main` | The entry file of the app, which is loaded by nRF Connect. When using webpack, this should point to the webpack bundle file, typically in the dist/ directory. |
| `engines.nrfconnect` | The nRF Connect version(s) the app supports. Should be a valid [semver range](https://github.com/npm/node-semver#ranges). The launcher will show a warning if this is missing or incompatible. |
| `files` | The files to include when publishing the app on the npm registry. Make sure that this contains everything the app needs at runtime, e.g. code, icon, and resources. |

Other than these, we also recommend setting at least `description`, `license`, `author`, and `repository.url`.

## App icon

If there is an `icon.png` file in the app directory, then this will appear next to the app in the nRF Connect launcher and in the app's window title. The default nRF Connect icon is used if no such file exists.

The icon is displayed at 40x40 pixels in the nRF Connect launcher, so make sure the icon is displaying nicely at that size.

## Dependencies

Usually, apps will depend on other modules from the npm registry. Dependencies can be specified in the app's package.json file. 

When using webpack, we recommended using `devDependencies` instead of `dependencies`. This will keep the app's size to a minimum. The `devDependencies` are only installed during development, while `dependencies` are installed when the user installs the app in nRF Connect. With webpack the code from all dependencies is bundled in when building the app, so the dependencies are only needed during development.

Some modules may not be possible to bundle with webpack. This could be native modules or modules that use some special syntax that webpack does not support. Those modules need to be added to `dependencies` instead of `devDependencies`.