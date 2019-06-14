macOS Build Instructions and Notes
====================================
The commands in this guide should be executed in a Terminal application.
The built-in one is located in `/Applications/Utilities/Terminal.app`.

Preparation
-----------
Install the macOS command line tools:

`xcode-select --install`

When the popup appears, click `Install`.

Then install [Homebrew](https://brew.sh).

Dependencies
----------------------

    brew install cmake automake berkeley-db4 libtool boost --c++11 --without-single --without-static miniupnpc openssl pkg-config protobuf qt5 libevent

See [dependencies.md](dependencies.md) for a complete overview.

If you want to build the disk image with `make deploy` (.dmg / optional), you need RSVG

    brew imagemagick --with-librsvg

Berkeley DB
-----------
It is recommended to use Berkeley DB 4.8. If you have to build it yourself,
you can use [the installation script included in contrib/](/contrib/install_db4.sh)
like so

```shell
./contrib/install_db4.sh .
```

from the root of the repository.

**Note**: You only need Berkeley DB if the wallet is enabled (see the section *Disable-Wallet mode* below).

Build Tripi Core
------------------------

1. Clone the Tripi Core source code and cd into `tripi`

        git clone --recursive https://github.com/tripiproject/tripi.git
        cd tripi
        git submodule update --init --recursive

2.  Build Tripi Core:

    Configure and build the headless Tripi Core binaries as well as the GUI (if Qt is found).

    You can disable the GUI build by passing `--without-gui` to configure.

        ./autogen.sh
        ./configure
        make

3.  It is recommended to build and run the unit tests:

        make check

4.  You can also create a .dmg that contains the .app bundle (optional):

        make deploy

Running
-------

Tripi Core is now available at `./src/tripid`

Before running, it's recommended that you create an RPC configuration file.

    echo -e "rpcuser=tripirpc\nrpcpassword=$(xxd -l 16 -p /dev/urandom)" > "/Users/${USER}/Library/Application Support/Tripi/tripi.conf"

    chmod 600 "/Users/${USER}/Library/Application Support/Tripi/tripi.conf"

The first time you run tripid, it will start downloading the blockchain. This process could take several hours.

You can monitor the download process by looking at the debug.log file:

    tail -f $HOME/Library/Application\ Support/Tripi/debug.log

Other commands:
-------

    ./src/tripid -daemon # Starts the tripi daemon.
    ./src/tripi-cli --help # Outputs a list of command-line options.
    ./src/tripi-cli help # Outputs a list of RPC commands when the daemon is running.

Notes
-----

* Tested on OS X 10.10 Yosemite through macOS 10.13 High Sierra on 64-bit Intel processors only.

* Building with downloaded Qt binaries is not officially supported. See the notes in [#7714](https://github.com/bitcoin/bitcoin/issues/7714)
