# Deploying via Eureka

## Initial Setup

First, let's clone the GitHub repository and install all needed dependencies:

```sh
git clone https://github.com/compound-finance/compound-eureka/
cd compound-eureka
yarn install
```

## Deploying Compound Protocol

To deploy our Compound protocol smart contracts, we will be using the script `yarn eureka apply`, specifying the network, build folder, config files, and eureka spec files. In order to verify deployed contracts via Etherscan, we specify the Etherscan API key through a passed `etherscan` environment variable. In this section, we will be writing the commands assuming the `ETHERSCAN_API_KEY` environment variable has been set up the way described in [Getting Started](./getting-started.md#etherscan-api-key).

We will be using the Ropsten test network, although we will later discuss how to get deployment working on other popular testnets, like Rinkeby.

### Initial Clean

A freshly cloned repository will already have deployment state saved, which cause Eureka to not execute a fresh deployment, processing any deployments under the pre-existing deployment data instead. Therefore, first clean the deployment state by running:

```sh
yarn eureka clean -n ropsten -c config/*.js
```

For more on `yarn eureka clean`, see the section on [Cleaning Deployment State below](#cleaning-deployment-state).

### Running the Deployment

Through specifying the Etherscan API key, the network as Ropsten, and the appropriate config, build, and eureka filepaths, we create a command that deploys core Compound protocol smart contracts to Ropsten:
```sh
etherscan=$ETHERSCAN_API_KEY yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet}.eureka
```

Deploying governance contracts with the core is as simple as adding `testnet-gov.eureka` to our list:
```sh
etherscan=$ETHERSCAN_API_KEY yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet,testnet-gov}.eureka
```
