# Deploying Compound Protocol

## Initial Setup

First, let's clone the GitHub repository and install all needed dependencies:

```sh
git clone https://github.com/compound-finance/compound-eureka/
cd compound-eureka
yarn install
```

## Deployment

### Compound
```sh
etherscan=$ETHERSCAN_API_KEY yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet}.eureka
```

### Compound (+ Governance)
```sh
etherscan=$ETHERSCAN_API_KEY yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet,testnet-gov}.eureka
```

### Rinkeby

### Compound
```sh
etherscan=$ETHERSCAN_API_KEY provider=https://rinkeby-eth.compound.finance yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet,testnet-gov}.eureka
```

### Compound (+ Governance)
```sh
etherscan=$ETHERSCAN_API_KEY provider=https://rinkeby-eth.compound.finance yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet,testnet-gov}.eureka
```

## Tips

### clean state

Any other issues can be fixed by cleaning the repo of the state file. 

For example, to clean the deployment state for Ropsten, run:
```sh
yarn eureka clean -n ropsten -c config/*.js
```

**NOTE**: Why don't we add the `-b` switch if the `compound-eureka` documentation includes it?

The answer: if we look at what `yarn eureka clean` runs, we see that the `-b` switch is not read at all. Following the chain of execution, within `package.json`, lines 6-9:
```js
  "scripts": {
    "eureka": "node --experimental-vm-modules --unhandled-rejections=strict node_modules/@compound-finance/eureka/src/cli.mjs",
    "import-contract": "node --experimental-vm-modules --unhandled-rejections=strict ./import_contract.mjs"
  },
```
And then, into `node_modules/@compound-finance/eureka/src/cli.mjs`, lines 40-43:
```js
    .command('clean', 'deletes Eureka backend state', (yargs) => {
      }, async (argv) => {
        cleanBackend(argv.network, array(argv.config_file), argv.yes, argv.verbose, argv.jsonrpc);
      })
```
From this, we can see that only the network (`-n`) and config (`-c`) switches are really read.

### increase gas price


