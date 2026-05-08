---
---

# Creating a new app

Before you create a new app for nRF Connect for Desktop, make sure you meet
all the required [prerequisites](getting_started#prerequisites).

You could start out with a blank Node.js project, but it is recommended to begin with the [`pc-nrfconnect-boilerplate`](https://github.com/nordicsemi/pc-nrfconnect-boilerplate) project. While you could also copy an existing app (for example, [`pc-nrfconnect-npm`](https://github.com/nordicsemi/pc-nrfconnect-npm) or [`pc-nrfconnect-rssi`](https://github.com/nordicsemi/pc-nrfconnect-rssi)) and strip it of unneeded elements, it is better to start small and use the existing apps for reference.

Complete the following steps to create a new app:

1. Clone `pc-nrfconnect-boilerplate` under the `~/.nrfconnect-apps/local`
   directory by running the following commands in a terminal on Linux or macOS, or in Git bash on Windows:

        cd $HOME/.nrfconnect-apps/local
        git clone https://github.com/nordicsemi/pc-nrfconnect-boilerplate.git pc-nrfconnect-myapp
        cd pc-nrfconnect-myapp
        rm -rf .git

   If you prefer it, you can also create the project directory anywhere else
   and create a symbolic link from `~/.nrfconnect-apps/local` to the project
   directory.

1. Modify relevant properties in `package.json`. At the very least, consider changing the following ones:

    - `name`
    - `displayName`
    - `description`
    - `homepage`
    - `version`
    - `author`
    - `license`
    - `repository.url`

1. Change the template implementation in `index.tsx` according to your application needs.

## Next steps

You now have everything set up for
[everyday app development](./app_development).

Read the further documentation beginning with the
[API reference](./api_reference) and source code of the official apps to get a
more thorough understanding of how apps work.
