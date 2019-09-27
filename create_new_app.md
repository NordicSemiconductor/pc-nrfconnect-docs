---
---

# How to create a new app for nRF Connect for Desktop

## Install nRF Connect

To create a new app, it is sufficient to have nRF Connect for Desktop installed
as a binary. If you do not have it installed already, just follow the
[instructions on how to install nRF Connect for Desktop](https://github.com/NordicSemiconductor/pc-nrfconnect-core#using-nrf-connect-for-desktop).

## Create new app project

1.  Create the `~/.nrfconnect-apps/local` directory if it does not already
    exist:

    - Linux/macOS: `mkdir -p "$HOME/.nrfconnect-apps/local"`
    - Windows: `md "%USERPROFILE%\.nrfconnect-apps\local"`

2.  Choose a template for the new project: While you could start out with a
    blank Node.js project, we recommend to begin with
    [`pc-nrfconnect-boilerplate`](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate).
    You could also begin with a more full fledged project like a
    [`pc-nrfconnect-ble`](https://github.com/NordicSemiconductor/pc-nrfconnect-ble)
    or a smaller one like
    [`pc-nrfconnect-rssi`](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi)
    and strip out what you do not need, but our recommendation is to just look
    at these for reference. If you choose something different than using
    `pc-nrfconnect-boilerplate`, then adjust the next step accordingly.

3.  Clone `pc-nrfconnect-boilerplate` under the `~/.nrfconnect-apps/local`
    directory. In a terminal on Linux/macOS or Git bash on Windows:

        cd $HOME/.nrfconnect-apps/local
        git clone https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate.git pc-nrfconnect-myapp
        cd pc-nrfconnect-myapp
        rm -rf .git

    If you prefer it, you can also create the project directory anywhere else
    and create a symbolic link from `~/.nrfconnect-apps/local` to the project
    directory.

4.  Modify relevant properties in `package.json`. At least consider changing:

    - `name`
    - `displayName`
    - `version`
    - `author`
    - `license`
    - `repository.url`

5.  Install dependencies:

        npm install

## Compiling, running and testing

When you are using `pc-nrfconnect-boilerplate` or any official app by Nordic as
a template, everything is already set up using
[`pc-nrfconnect-devdep`](https://github.com/NordicSemiconductor/pc-nrfconnect-devdep)
to use tools like [webpack](https://webpack.js.org),
[ESLint](https://eslint.org) and [Jest](https://jestjs.io).

### Compiling

Run `npm run build` to compile and pack everything once, which creates
`dist/bundle.js`.

During active development we recommend to run `npm run dev` instead, which
watches your code and creates a new `dist/bundle.js` repeatedly after every
change (`Ctrl+C` to stop).

### Running

Start nRF Connect for Desktop and it will automatically find the app in
`~/.nrfconnect-apps/local` and display it in the list of all apps with a “local”
underneath it. You can launch it from there.

When you edit the source of the app and it is recompiled (probably by keeping
`npm run dev` running), pressing `Ctrl+R` (Windows or Linux) or `Cmd+R` (macOS)
in the running app window will make it reload.

Chrome Developer Tools can be opened by pressing `Ctrl+Alt+I` (Windows/Linux) or
`Cmd+Option+I` (macOS).

### Testing

You can run the linter with `npm run lint`.

While you can run the tests once with `npm test`, we recommend to keep them
running repeatedly during development with `npm run test-watch`.

## Next steps

Change the implementation in `index.jsx` to adjust the behavior of the app. Read
the further documentation beginning with the [API reference](./api_reference)
and [source code of the official apps](./supported_apps) to get a more thorough
understanding of how apps work.
