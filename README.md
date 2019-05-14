# Mist Browser for PUFFScoin

[![Build Status develop branch](https://travis-ci.org/puffscoin/mist.svg?branch=develop)](https://travis-ci.org/puffscoin/mist)
[![Build status](https://ci.appveyor.com/api/projects/status/bcfm3v0y2ovq9xob?svg=true)](https://ci.appveyor.com/project/puffscoin/mist)

The Mist browser is a robust application for users to browse and use dApps on the PUFFScoin blockchain.

For the Mist API see [MISTAPI.md](https://github.com/puffscoin/mist/blob/develop/MISTAPI.md).

This repository is also the Electron host for the [Meteor-based wallet dapp](https://github.com/puffscoin/meteor-dapp-wallet).

## Help and troubleshooting

In order to get help regarding Mist or PUFFScoin

1.  Please check the [Mist troubleshooting guide](https://github.com/ethereum/mist/wiki).
1.  Search for [similar issues](https://github.com/ethereum/mist/issues?q=is%3Aopen+is%3Aissue+label%3A%22Type%3A+Canonical%22) and potential help.
1.  Or create a [new issue](https://github.com/puffscoin/mist/issues) and provide as much information as you can to recreate your problem.

## Installation

If you want to install the app from a pre-built version on the [release page](https://github.com/puffscoin/mist/releases), you can simply run the executable after download.

For updating, simply download the new version and copy it over the old one (keep a backup of the old one if you want to be sure).

#### Linux .zip installs

In order to install from .zip files, please install `libgconf2-4` first:

```bash
apt-get install libgconf2-4
```

### Config folder

The data folder for Mist depends on your operating system:

- Windows `%APPDATA%\Mist`
- macOS `~/Library/Application\ Support/Mist`
- Linux `~/.config/Mist`

## Development

For development, a Meteor server assists with live reload and CSS injection.

Once a Mist version is released the Meteor frontend part is bundled using the `meteor-build-client` npm package to create pure static files.

### Dependencies

To run mist in development you need:

- [Node.js](https://nodejs.org) `v7.x` (use the preferred installation method for your OS)
- [Meteor](https://www.meteor.com/install) javascript app framework
- [Yarn](https://yarnpkg.com/) package manager

Install the latter ones via:

```bash
$ curl https://install.meteor.com/ | sh
$ curl -o- -L https://yarnpkg.com/install.sh | bash
```

### Initialization

Now you're ready to initialize Mist for development:

```bash
$ git clone https://github.com/puffscoin/mist.git
$ cd mist
$ git submodule update --init --recursive
$ yarn
```

### Run Mist

For development we start the interface with a Meteor server for auto-reload etc.

_Start the interface in a separate terminal window:_

```bash
$ yarn dev:meteor
```

In the original window you can then start Mist with:

```bash
$ cd mist
$ yarn dev:electron
```

_NOTE: Client binaries (e.g. [gpuffs] (https://github.com/puffscoin/go-puffscoin)) specified in [clientBinaries.json](https://github.com/puffscoin/mist/blob/master/clientBinaries.json) will be checked during every startup and downloaded if out-of-date, binaries are stored in the config folder.

_NOTE: use `--help` to display available options, e.g. `--loglevel debug` (or `trace`) for verbose output_

### Run the Wallet

Start the wallet app for development, _in a separate terminal window:_

```bash
$ yarn dev:meteor
```

In another terminal:

```bash
$ cd my/path/meteor-dapp-wallet/app && meteor --port 3050
```

In the original window you can then start Mist using wallet mode:

```bash
$ cd mist
$ yarn dev:electron --mode wallet
```

### Connect your own node

This is useful if you are already running your own node or would like to connect with a private or development network.

```bash
$ yarn dev:electron --rpc path/to/gpuffs.ipc
```

### Passing options to Gpuffs

You can pass command-line options directly to Gpuffs by prefixing them with `--node-` in
the command-line invocation:

```bash
$ yarn dev:electron --mode mist --node-rpcport 11364 --node-networkid 420
```

The `--rpc` Mist option is a special case. If you set this to an IPC socket file
path then the `--ipcpath` option automatically gets set, i.e.:

```bash
$ yarn dev:electron --rpc path/to/gpuffs.ipc
```

...is the same as doing...

```bash
$ yarn dev:electron --rpc /my/gpuffs.ipc --node-ipcpath /path/to/gpuffs.ipc
```

### Creating a local private net

If you would like to quickly set up a local private network on your computer, run:

```bash
gpuffs --dev
```

Look for the IPC path in the resulting gpuffs output, then start Mist with:

```bash
$ yarn dev:electron --rpc path/to/gpuffs.ipc
```

### Deployment

Our build system relies on [gulp](http://gulpjs.com/) and [electron-builder](https://github.com/electron-userland/electron-builder/).

#### Dependencies

Cross-platform builds require [additional dependencies](https://www.electron.build/multi-platform-build) needed by Electron Builder. Please follow their instructions for up to date dependency information.

#### Generate packages

To generate the binaries for Mist run:

```bash
$ yarn build:mist
```

To generate the PUFFScoin Wallet:

```bash
$ yarn build:wallet
```

The generated binaries will be under `dist_mist/release` or `dist_wallet/release`. Both PUFFScoin Wallet and Mist ships with a meteor-dapp-wallet instance (https://github.com/puffscoin/meteor-dapp-wallet).

#### Options

##### platform

To build binaries for specific platforms (default: all available) use the following flags:

```bash
$ yarn build:mist --mac      # mac
$ yarn build:mist --linux    # linux
$ yarn build:mist --win      # windows
```

##### skipTasks

When building a binary, you can optionally skip some tasks — generally for testing purposes.

```bash
$ yarn build:mist --mac --skipTasks=build-interface,release-dist
```

##### Checksums

Prints the SHA-256 checksums of the distributables.

It expects installer/zip files to be in the generated folders e.g. `dist_mist/release`

```bash
$ yarn task checksums [--wallet]
```

#### Tasks found in gulpfile.js and gulpTasks/

Any other gulp task can be run using `yarn task`.

```bash
$ yarn task clean-dist
```

## Testing

Tests run using [Spectron](https://github.com/electron/spectron/), a webdriver.io runner built for Electron.

First make sure to build Mist with:

```bash
$ yarn build:mist
```

Then run the tests:

```bash
$ yarn test:unit:once
$ yarn test:e2e
```

_Note: Integration tests are not yet supported on Windows._
