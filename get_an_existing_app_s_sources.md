---
---

# Getting app sources

Before you get the sources of an existing nRF Connect for Desktop app, make sure you meet
all the required [prerequisites](getting_started#prerequisites).

To get an existing app's sources, clone the project to the `~/.nrfconnect-apps/local` directory.

For example, if you want to work on the [RSSI Viewer](https://github.com/nordicsemi/pc-nrfconnect-rssi), run
the following commands in a terminal on Linux or macOS, or in Git bash on Windows:

    cd $HOME/.nrfconnect-apps/local
    git clone https://github.com/nordicsemi/pc-nrfconnect-rssi

Optionally, you can also create the project directory anywhere else and create a symbolic link
from `~/.nrfconnect-apps/local` to the project directory.

## Next steps

You now have everything set up for
[everyday app development](./app_development).
