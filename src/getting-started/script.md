# Script

If you would like to automate the installation of required packages, configuration, and initial setup of both Compound protocol repositories, you may use the script below.

```sh
#!/bin/bash

#
# Script to automate the setup of Compound protocol building and deployment
# repositories on an Ubuntu Linux 20.04 LTS machine.
#
# This script takes `-p` and `-e` as required flags:
#   -p: Ethereum private key
#   -e: Etherscan API key
#
# The script configures the system for Compound using the keys passed.
#

while getopts 'p:e:' flag
do
	case "${flag}" in
		p) PRIVATE_KEY=${OPTARG};;
		e) ETHERSCAN_API_KEY=${OPTARG};;
	esac
done

if [ -z "$PRIVATE_KEY" ]; then
	echo "Error: Private key must be set with -p"
	exit 1
fi
if [ -z "$ETHERSCAN_API_KEY" ]; then
	echo "Error: Etherscan API key must be set with -e"
	exit 1
fi

# Update package list
sudo apt-get update

# Add Node.js 13.x repo
curl -fsSL https://deb.nodesource.com/setup_13.x | sudo -E bash -

# Install make, node, libusb, libudev
sudo apt-get install -y make nodejs libusb-1.0-0-dev libudev-dev

# Install yarn
sudo npm install --global yarn

# Install solc
sudo wget https://github.com/ethereum/solidity/releases/download/v0.5.16/solc-static-linux -O /usr/local/bin/solc
sudo chmod +x /usr/local/bin/solc

# Setup secret keys, and Etherscan API key
mkdir -p ~/.ethereum
echo $PRIVATE_KEY > ~/.ethereum/rinkeby
echo $PRIVATE_KEY > ~/.ethereum/ropsten
echo -e "\nETHERSCAN_API_KEY=$ETHERSCAN_API_KEY" >> ~/.bashrc

# Setup compound-protocol and compound-eureka and install dependencies
git clone https://github.com/compound-finance/compound-protocol
cd compound-protocol
yarn install --lock-file
cd ..

git clone https://github.com/compound-finance/compound-eureka/
cd compound-eureka
yarn install
yarn eureka clean -n ropsten -c config/*.js
```
