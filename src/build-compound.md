# Building Compound Protocol

If you are looking to deploy a specific or modified version of the Compound protocol, this section will address the building process of the official Compound protocol repository! Note that Compound Eureka (the Compound deployment scripts) already have built versions of Compound committed into the repository, so this section can be skipped.

## Initial setup

First, let's clone the GitHub repository:

```sh
git clone https://github.com/compound-finance/compound-protocol
cd compound-protocol
yarn install --lock-file
```

## Configuration

`compound-protocol` requires a private key for any deployment-related scripts, and an Etherscan API key, if you'd like to verify those deployed contracts on Etherscan. 

### Private Key

For setting a private key to run transactions from, you have two options:
1. Save the private key in a file named after the network within `~/ethereum`
2. Pass the `ACCOUNT` environment variable to the script

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

## Compiling

Compiling is simple:
```sh
yarn test
```

## Testing

If you would like to test the contracts, you may run `yarn test`, but note that it may take a while to run all of them. The following output was received after running all tests, taking a little over an hour:
```
Test Suites: 2 skipped, 99 passed, 99 of 101 total
Tests:       46 skipped, 15 todo, 1074 passed, 1135 total
Snapshots:   0 total
Time:        3927.308s
Ran all test suites matching /test/i.
Teardown in 0 ms
Done in 3951.43s.
```
It may be useful to know that the [`compound-protocol` CircleCI](https://app.circleci.com/pipelines/github/compound-finance/compound-protocol/498/workflows/32845109-0390-4d92-b8c8-b8492a3adaff/jobs/2190) only runs a portion of these tests, which expands to:
```sh
yarn test tests/CompilerTest.js tests/Comptroller/assetsListTest.js tests/Comptroller/pauseGuardianTest.js tests/Flywheel/FlywheelTest.js tests/Governance/CompScenarioTest.js tests/Governance/GovernorAlpha/ProposeTest.js tests/Governance/GovernorBravo/CastVoteTest.js tests/Governance/GovernorBravo/StateTest.js tests/Models/DAIInterestRateModelTest.js tests/SpinaramaTest.js tests/Tokens/adminTest.js tests/Tokens/cTokenTest.js tests/Tokens/mintAndRedeemCEtherTest.js tests/Tokens/safeTokenTest.js tests/Tokens/transferTest.js
```
This may be a shorter subset that allows for easier testing. 
