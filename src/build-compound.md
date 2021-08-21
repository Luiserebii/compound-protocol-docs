# Building Compound Protocol

If you are looking to deploy a specific or modified version of the Compound protocol, this section will address the building process of the official Compound protocol repository! Note that Compound Eureka (the Compound deployment scripts) already have built versions of Compound committed into the repository, so this section can be skipped.

## Initial setup

First, let's clone the GitHub repository and install all needed dependencies:

```sh
git clone https://github.com/compound-finance/compound-protocol
cd compound-protocol
yarn install --lock-file
```

## Compiling

Compiling is simple:
```sh
yarn compile
```
<script id="asciicast-KKD6834A608k84sb0aqyHILm9" src="https://asciinema.org/a/KKD6834A608k84sb0aqyHILm9.js" async data-autoplay="true"></script>

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
