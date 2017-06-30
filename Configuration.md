## File system location

nRF Connect stores apps in the following directory:

* Linux/macOS: `$HOME/.nrfconnect-apps`
* Windows: `%USERPROFILE%\.nrfconnect-apps`

### Official apps

When adding an app through the *Add/remove apps* UI, the app is installed and added as a dependency in `.nrfconnect-apps/package.json`. This allows nRF Connect to keep track of installed apps and available upgrades just by using npm.

### Local apps

Apps that are unofficial or in development are retrieved from the `.nrfconnect-apps/local` directory. Adding an app here will make it appear in the nRF Connect launcher, so that you can test it.

## Properties in package.json

The following package.json properties should be configured by nRF Connect apps:

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

When using webpack, we recommended using `devDependencies` instead of `dependencies`. This will keep the app's size to a minimum. The `dependencies` are installed when the user installs the app in nRF Connect, while `devDependencies` are only installed during development. With webpack, code from all dependencies is bundled when building the app, so the dependencies should not be needed anymore after the app has been built.

Some modules may not be possible to bundle with webpack. This could be native modules or modules that use some special syntax that webpack does not support. Those modules need to be added to `dependencies` instead of `devDependencies`.

## Webpack

The [nRF Connect boilerplate app](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate) comes with a webpack configuration that is ready to use. Normally, app developers should not need to edit this, but in some special cases it might be necessary, e.g. to add extra loaders, file extensions, etc. Refer to the [webpack documentation](https://webpack.js.org/) for more information. The below sections describe a few peculiarities about webpack in nRF Connect that might be useful to know.

### Externals

Apps can import a few [[modules|Modules]] from nRF Connect. The default webpack configuration ignores these by adding them as [externals](https://webpack.js.org/configuration/externals/), as they are only available at runtime. The same is automatically done for any `dependencies` from package.json.

### Public path and static resources

The apps can import static resources such as images, which is bundled by webpack. For this to work, the app needs to set `__webpack_public_path__` at runtime, before importing static resources. This should be done in a separate file, e.g. `setup.js` that is imported before anything else in `index.jsx`:

```
// setup.js:

import path from 'path';
import core from 'nrfconnect/core';

__webpack_public_path__ = path.join(core.getAppDir(), 'dist/');
```

```
// index.jsx:

import './setup';
...
```

When this is configured, React components can import and render images, e.g:

```
import image from './resources/image.png';

const MyComponent = () => (
    <img src={image} alt='My image' />
);
```