What is Tripi?
-------------

Tripi is a decentralized blockchain project built on Bitcoin's UTXO model, with support for Ethereum Virtual Machine based smart contracts, and secured by a proof of stake consensus model. It achieves this through the revolutionary Account Abstraction Layer which allows the EVM to communicate with Tripi's Bitcoin-like UTXO blockchain. For more general information about Tripi as well as links to join our community, go to https://tripi.io

Welcome to the Tripi Ignition Main Network. This is the main network where the tokens hold value and should be guarded very carefully. If you are testing the network, or developing unstable software on Tripi, we highly recommend using either testnet or regtest mode. 

The major features of the Tripi network include:

1. Compatibility with the Ethereum Virtual Machine, which allows for compatibility with most existing Solidity based smart contracts. No special solidity compiler is required to deploy your smart contract to Tripi. 
2. A Proof of Stake consensus system which is optimized for Tripi's contract model. Any user can stake and help to secure the network. There is no voting, master nodes, or minimum amount required. There have been transactions as small as 2 Tripi that have created blocks in the past. 
3. The Decentralized Governance Protocol is completely implemented and functional, which allows certain network parameters to be modified without a fork or other network disruption. This currently controls parameters like block size, gas prices, etc. 
4. Uses the UTXO transaction model and is compatible with Bitcoin, allowing for existing tooling and workflows to be used with Tripi. This allows for the infamous SPV protocol to be used which is ideal for light wallets on mobile phones and IoT devices.

Note: Tripi Core is considered beta software. We make no warranties or guarantees of its security or stability.

Tripi Documentation and Usage Resources
---------------

These are some resources that might be helpful in understanding Tripi. Note that the unofficial documents are not created by the Tripi team.

Basic usage resources:

* [Block explorer](https://track.tripi.io)


Development resources:

* [Tripi Book - A Developer's Guide To TRIPI](https://github.com/tripiproject/tripibook)

General Info about Tripi:

* [Tripi - TRIPI.io](https://www.tripi.io)


What is Tripi Core?
------------------

Tripi Core is our primary mainnet wallet. It implements a full node and is capable of storing, validating, and distributing all history of the Tripi network. Tripi Core is considered the reference implementation for the Tripi network. 

Tripi Core currently implements the following:

* Sending/Receiving Tripi
* Sending/Receiving TRC20 tokens on the Tripi network
* Staking and creating blocks for the Tripi network
* Creating and interacting with smart contracts
* Running a full node for distributing the blockchain to other users
* "Prune" mode, which minimizes disk usage
* Regtest mode, which enables developers to very quickly build their own private Tripi network for Dapp testing
* Compatibility with the Bitcoin Core set of RPC commands and APIs


Building Tripi Core
----------

### Build on Ubuntu

    This is a quick start script for compiling Tripi on  Ubuntu


    sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils git cmake libboost-all-dev
    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:bitcoin/bitcoin
    sudo apt-get update
    sudo apt-get install libdb4.8-dev libdb4.8++-dev

    # If you want to build the Qt GUI:
    sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler qrencode

    git clone https://github.com/tripiproject/tripi --recursive
    cd tripi

    # Note autogen will prompt to install some more dependencies if needed
    ./autogen.sh
    ./configure 
    make -j2
    
### Build on CentOS

Here is a brief description for compiling Tripi on CentOS, for more details please refer to [the specific document](https://github.com/tripiproject/tripi/blob/master/doc/build-unix.md)

    # Compiling boost manually
    sudo yum install python-devel bzip2-devel
    git clone https://github.com/boostorg/boost.git
    cd boost
    git checkout boost-1.66.0
    git submodule update --init --recursive
    ./bootstrap.sh --prefix=/usr --libdir=/usr/lib64
    ./b2 headers
    sudo ./b2 -j4 install
    
    # Installing Dependencies for Tripi
    sudo yum install epel-release
    sudo yum install libtool libdb4-cxx-devel openssl-devel libevent-devel
    
    # If you want to build the Qt GUI:
    sudo yum install qt5-qttools-devel protobuf-devel qrencode-devel
    
    # Building Tripi
    git clone --recursive https://github.com/tripiproject/tripi.git
    cd tripi
    ./autogen.sh
    ./configure
    make -j4

### Build on OSX

The commands in this guide should be executed in a Terminal application.
The built-in one is located in `/Applications/Utilities/Terminal.app`.

#### Preparation

Install the OS X command line tools:

`xcode-select --install`

When the popup appears, click `Install`.

Then install [Homebrew](https://brew.sh).

#### Dependencies

    brew install cmake automake berkeley-db4 libtool boost --c++11 --without-single --without-static miniupnpc openssl pkg-config protobuf qt5 libevent imagemagick --with-librsvg qrencode

NOTE: Building with Qt4 is still supported, however, could result in a broken UI. Building with Qt5 is recommended.

#### Build Tripi Core

1. Clone the tripi source code and cd into `tripi`

        git clone --recursive https://github.com/tripiproject/tripi.git
        cd tripi

2.  Build tripi-core:

    Configure and build the headless tripi binaries as well as the GUI (if Qt is found).

    You can disable the GUI build by passing `--without-gui` to configure.

        ./autogen.sh
        ./configure
        make

3.  It is recommended to build and run the unit tests:

        make check

### Run

Then you can either run the command-line daemon using `src/tripid` and `src/tripi-cli`, or you can run the Qt GUI using `src/qt/tripi-qt`

For in-depth description of Sparknet and how to use Tripi for interacting with contracts, please see [sparknet-guide](doc/sparknet-guide.md).

License
-------

Tripi is GPLv3 licensed.


Development Process
-------------------

The `master` branch is regularly built and tested, but is not guaranteed to be
completely stable. [Tags](https://github.com/tripiproject/tripi/tags) are created
regularly to indicate new official, stable release versions of Tripi.

The contribution workflow is described in [CONTRIBUTING.md](CONTRIBUTING.md).

Developer IRC can be found on Freenode at #tripi-dev.

Testing
-------

Testing and code review is the bottleneck for development; we get more pull
requests than we can review and test on short notice. Please be patient and help out by testing
other people's pull requests, and remember this is a security-critical project where any mistake might cost people
lots of money.

### Automated Testing

Developers are strongly encouraged to write [unit tests](src/test/README.md) for new code, and to
submit new unit tests for old code. Unit tests can be compiled and run
(assuming they weren't disabled in configure) with: `make check`. Further details on running
and extending unit tests can be found in [/src/test/README.md](/src/test/README.md).

There are also [regression and integration tests](/test), written
in Python, that are run automatically on the build server.
These tests can be run (if the [test dependencies](/test) are installed) with: `test/functional/test_runner.py`

### Manual Quality Assurance (QA) Testing

Changes should be tested by somebody other than the developer who wrote the
code. This is especially important for large or high-risk changes. It is useful
to add a test plan to the pull request description if testing the changes is
not straightforward.
