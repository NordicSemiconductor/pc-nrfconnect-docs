---
---

# Configuration

## File system location

nRF Connect stores apps in the following directory:

- Linux/macOS: `$HOME/.nrfconnect-apps`
- Windows: `%USERPROFILE%\.nrfconnect-apps`

Other files, like configurations, logs or the `nrfutil` sandboxes are stored in
the following directory:

- macOS: `$HOME/Library/Application Support/nrfconnect`
- Linux: `$XDG_CONFIG_HOME/nrfconnect` or `$HOME/.config/nrfconnect`
- Windows: `%APPDATA%\nrfconnect`

### Official apps

Official apps are uploaded to `files.nordicsemi.com`. When clicking on _Install_
in the launcher, the app tarball is downloaded and extracted to
`.nrfconnect-apps/node_modules`.

### Local apps

Apps that are unofficial or in development are retrieved from the
`.nrfconnect-apps/local` directory. Adding an app here will make it appear in
the nRF Connect launcher, so that you can test it.

## Properties in package.json

The following package.json properties should be configured by nRF Connect apps:

| Property                           | Description                                                                                                                                                                                                   |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                             | The name used to identify the app. The recommended naming convention is `pc-nrfconnect-<appname>`, as this makes it easier to identify it as an nRF Connect app.                                              |
| `displayName`                      | The name shown in the nRF Connect launcher and in the app's window title.                                                                                                                                     |
| `description`                      | The description shown in the nRF Connect launcher.                                                                                                                                                            |
| `version`                          | The current version of the app. Should be a valid [semver string](http://semver.org/). The nRF Connect launcher will display an upgrade button when new versions of the app are made available on the server. |
| `engines.nrfconnect`               | The nRF Connect version(s) the app supports. Should be a valid [semver range](https://github.com/npm/node-semver#ranges). The launcher will show a warning if this is missing or incompatible.                |
| `files`                            | The files to include when publishing the app on the npm registry. Make sure that this contains everything the app needs at runtime, e.g. code, icon, and resources.                                           |
| `nrfConnectForDesktop.html`        | The html file the launcher displays in the app window, when loading the app. When using the default configurations from `pc-nrfconnect-shared`, this is `dist/index.html`.                                    |
| `nrfConnectForDesktop.nrfutilCore` | Which [core version of nRF Util](https://docs.nordicsemi.com/bundle/nrfutil/page/nrfutil-core/CHANGELOG.html) the app depends upon, defaults to 8.0.0.                                                        |
| `nrfConnectForDesktop.nrfutil.*`   | For each command of nRF Util the app uses (usually at least `device`) the version of this command to use.                                                                                                     |

Other than these, we also recommend setting at least `license`, `homepage`,
`author`, and `repository.url`.

## App icon

If there is (preferred) an `icon.svg` or an `icon.png` file in the app
directory, then this will appear next to the app in the nRF Connect launcher and
in the app's window title. The default nRF Connect icon is used if no such file
exists.

The icon is displayed at 40x40 pixels in the nRF Connect launcher, so make sure
the icon is displaying nicely at that size.

## Dependencies

Usually, apps will depend on other modules from the npm registry. Dependencies
can be specified in the app's package.json file.

As a general rule, apps should use `devDependencies` instead of `dependencies`
when possible. This will keep the app's size to a minimum. At build time,
esbuild will bundle all the code that the app needs to run, so the dependencies
are normally only needed at build time.

Some modules may not be possible to bundle with esbuild. This could be native
modules or modules that use some special syntax that esbuild does not support.
In this case, the module should be added to `dependencies` instead of
`devDependencies`, and also added to `bundledDependencies` so that it is
included in the tarball that is published to npm.

## esbuild

[`pc-nrfconnect-shared` provides a script `run-esbuild`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/caf1908a3ebf7f9a0a9b0ac0c660ab243db02687/package.json#L15)
to run `esbuild` with a configuration that is ready to use. The
[nRF Connect boilerplate app defines `scripts` to run `esbuild`](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate/blob/f337484c811d4725b4cff33dd2da4cd6e185f2ea/package.json#L24-L27)
to build the app.

Normally, app developers should not need to edit this, but you are free to
bundle apps in another way if that suits you better.

### Externals

Apps can import a few modules from nRF Connect for Desktop.
[The default `esbuild` configuration ignores these by adding them as `external`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/caf1908a3ebf7f9a0a9b0ac0c660ab243db02687/scripts/esbuild-renderer.ts#L51-L62),
as they are available at runtime. The same is automatically done for any
`dependencies` from package.json.

## Release notes

All official apps should have a file [`Changelog.md`](./changelogs). When
publishing apps, that changelog is automatically uploaded and users see it in
the launcher as release notes of the app.
