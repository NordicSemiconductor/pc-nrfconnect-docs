---
---

## Prerequisites

### Recommended knowledge

nRF Connect is based on [Electron](https://electron.atom.io/),
[React](https://facebook.github.io/react/), and [Redux](http://redux.js.org/).
Some familiarity with React and Redux is recommended in order to create apps.
You should at least understand concepts like JSX, props, actions, reducers, and
immutable state. Also, familiarity with
[ECMAScript 2015](https://babeljs.io/learn-es2015/) syntax such as arrow
functions, modules, const/let, and template strings is recommended.

### Install nRF Connect

Before you can create apps, you need to have nRF Connect up and running. Follow
the instructions in the
[project README](https://github.com/NordicSemiconductor/pc-nrfconnect-core) to
either install the binaries or build it from source.

### Install development tools

Development tools required:

- [Node.js](https://nodejs.org)
- [Git](https://git-scm.com/downloads)
- An editor with good JavaScript support (e.g. VS Code, Atom, WebStorm)

## Create an app project

We recommend using tools like webpack, babel, eslint, and jest when creating
apps. Fortunately, you do not need to configure these yourself. We have created
an
[nRF Connect boilerplate app](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate)
that gives you what you need to get started. Follow the instructions in the
boilerplate app README to create an empty app project. If you are already
familiar with our apps, you could develop your own app based on any of our apps.

### Clone repository

Since _nRF Connect_ expects local apps in

- `$HOME/.nrfconnect-apps/local` (Linux/macOS) or
- `%USERPROFILE%/.nrfconnect-apps/local` (Windows) directory,

make sure your repository is cloned or linked there, e.g.

```
git clone https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate.git
```

### Compiling

When _nRF Connect_ have been installed, you are ready to start the compilation.
Run the following command from the command line, standing in the root folder of
the repository:

    npm install

When the procedure has completed successfully you can run the application by
running:

    npm run dev

The built app can be loaded by _nRF Connect_ launcher.

### Testing

Unit testing can be performed by running:

    npm test
