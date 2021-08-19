# Deploying Compound Protocol

`compound-eureka` uses Eureka ([@compound-finance/eureka](https://www.npmjs.com/package/@compound-finance/eureka/v/1.0.3)) to author the smart contract deployment description, logic, and flow. Through passing `.eureka` files stored in the `./eureka` directory to `yarn eureka apply`, we can specify which parts of the protocol we'd like to deploy. For more on Eureka, see the README.md documenation included with the npm package.

## On `.eureka`

One peering over the `.eureka` files (such as `./eureka/compound.eureka`) will notice that some contracts have a string within square brackets (`[]`). Although this is undocumented, I believe it refers to the name of the `.json` file blob to refer to within the `.build` folder, which appears to have the contract ABI and pre-compiled bytecode built in. For example, `[b73e68c]` looks for the named contract within `./.build/b73e68c.json`.

If you are looking to use a custom build of Compound, you will want to note that you will likely have to make a copy of the appropriate `.eureka` file and modify them to refer to your built `.json`.

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

### Using Alternate Testnets (e.g. Rinkeby)

To use an alternate testnet, like Rinkeby, we have to be a little creative. Attempting to run `yarn eureka apply -n rinkeby ...` produces a deployment that appears to break because the emitted transactions are run out of order, suggesting it is undocumented for a reason.

However, we have a working deployment setup for the `-n ropsten` switch! So, by modifying the switch to run all transactions and verify contracts on the Rinkeby network instead, we can accomplish a successful deployment to Rinkeby. Thus, our job changes to ensuring that transactions running off of the `-n ropsten` switch pass through a Rinkeby provider, and that transactions are verified off of the Rinkeby network when interfacing with Etherscan.

Thankfully, `yarn eureka apply` checks for a `provider` environment variable, which will override the provider set! Thus, for Rinkeby, we may pass `provider=https://rinkeby-eth.compound.finance`. 

What about verifying contracts on Etherscan under the right network? Our solution for this is a little dirtier, and involves modifying Eureka itself. Open the file `./node_modules/@compound-finance/eureka/src/actor.mjs` for editing, browse to lines 264-298, and look at the function `doVerify`:
```js
  async function doVerify(contract, address, args, contractJson, constructorData) {
    if (!ethereum.verificationOpts.etherscanApiKey) {
      throw new Error(`Cannot verify contract without Etherscan API Key`);
    }

    let funcInfo = showFunc(contract, 'constructor', args);

    if (dryRun) {
      log(`[Dry run] Verifying contract ${contract} at ${address} with ${funcInfo}`);
      return;
    }

    try {
      log(`Verifying contract ${contract} at ${address} with ${funcInfo}`);

      let {
        verified,
        url,
        alreadyVerified
      } = await Etherscan.verifyContract(contractJson, ethereum.network, ethereum.verificationOpts.etherscanApiKey, address, constructorData, verbose);

      if (!verified) {
        throw new Error(`Failed to verify contract ${contract}`);
      }

      if (alreadyVerified) {
        log(`Contract ${contract} already verified at ${url}`);
      } else {
        log(`Contract ${contract} verified on Etherscan at ${url}`);
      }
    } catch (e) {
      logError(`Error verifying \`${funcInfo}\`: ${e.toString()}`);
      throw e;
    }
  }
```

What we want to target is the argument being passed `ethereum.network` to the `await Etherscan.verifyContract()` call, and hardcode it to our target testnet (in this case, Rinkeby). Therefore, to hardcode it to Rinkeby, we might modify line 283 like so:
```js
  async function doVerify(contract, address, args, contractJson, constructorData) {
    if (!ethereum.verificationOpts.etherscanApiKey) {
      throw new Error(`Cannot verify contract without Etherscan API Key`);
    }

    let funcInfo = showFunc(contract, 'constructor', args);

    if (dryRun) {
      log(`[Dry run] Verifying contract ${contract} at ${address} with ${funcInfo}`);
      return;
    }

    try {
      log(`Verifying contract ${contract} at ${address} with ${funcInfo}`);

      let {
        verified,
        url,
        alreadyVerified
      } = await Etherscan.verifyContract(contractJson, 'rinkeby', ethereum.verificationOpts.etherscanApiKey, address, constructorData, verbose); // Hardcoding to 'rinkeby'

      if (!verified) {
        throw new Error(`Failed to verify contract ${contract}`);
      }

      if (alreadyVerified) {
        log(`Contract ${contract} already verified at ${url}`);
      } else {
        log(`Contract ${contract} verified on Etherscan at ${url}`);
      }
    } catch (e) {
      logError(`Error verifying \`${funcInfo}\`: ${e.toString()}`);
      throw e;
    }
  }
```

Save the file, and all verifications should now run for Rinkeby. Finally, we run the deployment commands in much the same way as both, just with the `provider` included. Deploying just the core Compound contracts is thus:
```sh
etherscan=$ETHERSCAN_API_KEY provider=https://rinkeby-eth.compound.finance yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet,testnet-gov}.eureka
```

And with governance included:
```sh
etherscan=$ETHERSCAN_API_KEY provider=https://rinkeby-eth.compound.finance yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet,testnet-gov}.eureka
```

## Tips

### Cleaning Deployment State

Eureka saves deployment state data on newly deployed contracts to a file. Running `yarn eureka clean` is useful for wiping this state and ensuring that a new deployment is clean. If you are looking to deploy all of Compound protocol again, you will need to clear the state!

The general syntax of the command is:
```sh
yarn eureka clean -n network -c config/*.js
```
where network is substituted for the name of the network. For example, to clean the deployment state for Ropsten, run:
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

### Increasing Gas Price

By default, Eureka will run all transactions with a gas price of 1 Gwei. This might be useful to increase if you'd like to speed up the deployment process, either for convenience, or to resolve an error.

We can accomplish this in a dirty way by directly modifying Eureka. Open the file located at `./node_modules/@compound-finance/eureka/src/ethereum.mjs` for editing, and within lines 207-218, look at the function there:
```js
export async function dispatch({ eth, provider, from, gas, gasPrice }, contract, data) {
  return await eth.sendTransaction({
    from,
    to: contract,
    value: 0,
    gas,
    gasPrice,
    data: data
  }).on('transactionHash', (hash) => {
    console.log(`Running Ethereum Trx: ${hash}`);
  });
}
```

We want to hardcode the `gasPrice` variable. For example, if we were to hardcode it to 7 Gwei, we might modify the `gasPrice` line (line 213) like so:
```js
export async function dispatch({ eth, provider, from, gas, gasPrice }, contract, data) {
  return await eth.sendTransaction({
    from,
    to: contract,
    value: 0,
    gas,
    gasPrice: '7000000000', // Hardcoding to 7 Gwei
    data: data
  }).on('transactionHash', (hash) => {
    console.log(`Running Ethereum Trx: ${hash}`);
  });
}
```

Save the file, and all Eureka transactions should now execute with the new gas price value!
