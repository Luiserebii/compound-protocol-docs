# Tips

## Cleaning Deployment State

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

## Increasing Gas Price

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
