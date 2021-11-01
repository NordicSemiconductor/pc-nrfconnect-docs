---
---

# How to create a new app for nRF Connect for Desktop

First make sure, all the needed [prerequisites](getting_started#prerequisites)
are met.

## Create new app project

1.  Choose a template for the new project: While you could start out with a
    blank Node.js project, we recommend to begin with
    [`pc-nrfconnect-boilerplate`](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate).
    You could also begin with a more full fledged project like a
    [`pc-nrfconnect-ble`](https://github.com/NordicSemiconductor/pc-nrfconnect-ble)
    or a smaller one like
    [`pc-nrfconnect-rssi`](https://github.com/NordicSemiconductor/pc-nrfconnect-rssi)
    and strip out what you do not need, but our recommendation is to just look
    at these for reference. If you choose something different than using
    `pc-nrfconnect-boilerplate`, then adjust the next step accordingly.

1.  Clone `pc-nrfconnect-boilerplate` under the `~/.nrfconnect-apps/local`
    directory. In a terminal on Linux/macOS or Git bash on Windows:

        cd $HOME/.nrfconnect-apps/local
        git clone https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate.git pc-nrfconnect-myapp
        cd pc-nrfconnect-myapp
        rm -rf .git

    If you prefer it, you can also create the project directory anywhere else
    and create a symbolic link from `~/.nrfconnect-apps/local` to the project
    directory.

1.  Modify relevant properties in `package.json`. At least consider changing:

    - `name`
    - `displayName`
    - `version`
    - `author`
    - `license`
    - `repository.url`

## Next steps

You now have everything set up for
[everyday app development](./app_development), so read up about that. Then
change the implementation in `index.jsx` to adjust the behavior of the app.

Read the further documentation beginning with the
[API reference](./api_reference) and source code of the official apps to get a
more thorough understanding of how apps work.
