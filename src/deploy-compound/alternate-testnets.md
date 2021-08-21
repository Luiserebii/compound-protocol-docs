# Deploying to Alternate Testnets (e.g. Rinkeby)

To deploy to an alternate testnet, like Rinkeby, we have to be a little creative. Attempting to run `yarn eureka apply -n rinkeby ...` produces a deployment that appears to break because the emitted transactions are run out of order, suggesting it is undocumented for a reason.

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
etherscan=$ETHERSCAN_API_KEY provider=https://rinkeby-eth.compound.finance yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet}.eureka
```

And with governance included:
```sh
etherscan=$ETHERSCAN_API_KEY provider=https://rinkeby-eth.compound.finance yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet,testnet-gov}.eureka
```

It is important to note, of course, that the private key saved at `~/.ethereum/ropsten` will be used, not the one at `~/.ethereum/rinkeby`!
