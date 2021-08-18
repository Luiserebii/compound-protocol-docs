# Getting Started

## Prerequisites

Node 13.x, `npm`, `yarn`, `solc` v0.5.16, `git`, `python`, `g++`, and `make` are all needed to be installed on the system before using either repository. With the exception of the Node.js-specific tooling, many of these should be available on a basic Linux installation out of the box.

`libusb` and `libudev` are also required for the `compound-protocol` repo.

Note that if you have a newer version of `solc`, you will need to install the older version to build Compound protocol contracts.

We will be providing instructions on how to install most of these dependencies below, so don't worry!

## Installation

First, let's refresh our package list:
```sh
sudo apt-get update
```

Next, install each of the following, as needed:

### make
```sh
sudo apt-get install -y make
```

### libusb and libudev
```sh
sudo apt-get install -y libusb-1.0-0-dev libudev-dev
```

### Node 13.x
```sh
curl -fsSL https://deb.nodesource.com/setup_13.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### yarn
```sh
npm install --global yarn
```

### solc v0.5.16
```sh
sudo wget https://github.com/ethereum/solidity/releases/download/v0.5.16/solc-static-linux -O /usr/local/bin/solc
sudo chmod +x /usr/local/bin/solc
```

## Configuration

`compound-protocol` and `compound-eureka` require a private key for any deployment-related scripts, and an Etherscan API key, if you'd like to verify those deployed contracts on Etherscan. Luckily, the configuration for both are very similar.

### Private Key

For setting a private key to run transactions from, you have two options:
1. Save the private key in a file named after the network within `~/ethereum`
2. Pass the `ACCOUNT`/`pk` environment variable to the command

I recommend stashing the private key in a file. For example, to save a private key `80bbfb503fb22fc4de19bac1e42cbd5983d8ed26b5c04884b142e6ddf6140eef` for usage for the Rinkeby network, one may run:
```sh
mkdir -p ~/.ethereum
echo '80bbfb503fb22fc4de19bac1e42cbd5983d8ed26b5c04884b142e6ddf6140eef' > ~/.ethereum/rinkeby
```

### Etherscan API Key

To have the script verify a contract on Etherscan after deployment, the API key must be passed through an environment variable. I suggest stashing the API key in your `~/.bashrc` and referring to it from there. For example, to save an API key `7C0WA5ES3176894W6APLTH0K9T6S0M1V3F`, one may run:
```sh
echo -e '\nETHERSCAN_API_KEY=7C0WA5ES3176894W6APLTH0K9T6S0M1V3F' >> ~/.bashrc
```

Don't forget to run `. ~/.bashrc` to reload our new configuration into the current shell environment!
