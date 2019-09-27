---
---

# How to work on an existing app of nRF Connect for Desktop

## Install nRF Connect

To work on an existing app, it is sufficient to have nRF Connect for Desktop
installed as a binary. If you do not have it installed already, just follow the
[instructions on how to install nRF Connect for Desktop](https://github.com/NordicSemiconductor/pc-nrfconnect-core#using-nrf-connect-for-desktop).

## Get app sources

1.  Create the `~/.nrfconnect-apps/local` directory if it does not already
    exist:

    - Linux/macOS: `mkdir -p "$HOME/.nrfconnect-apps/local"`
    - Windows: `md "%USERPROFILE%\.nrfconnect-apps\local"`

2.  Clone the project to the `~/.nrfconnect-apps/local`
    directory. If you want to work on the [RSSI Viewer](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi) for example, do this in a terminal on Linux/macOS or Git bash on Windows:

        cd $HOME/.nrfconnect-apps/local
        git clone https://github.com/NordicSemiconductor/pc-nrfconnect-rssi

    If you prefer it, you can also create the project directory anywhere else
    and create a symbolic link from `~/.nrfconnect-apps/local` to the project
    directory.

3.  Install dependencies:

        npm install

## Compiling, running and testing

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
