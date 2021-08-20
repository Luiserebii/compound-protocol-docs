# Compound Core + Governance

The transactions outlined are the ones executed when specifying the `eureka/compound.eureka`, ,`eureka/testnet.eureka`, and `eureka/testnet-gov.eureka` files. They thus correspond to the command:

```sh
yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet,testnet-gov}.eureka
```

## Order of Deployment

1. Compound Lens
2. Jump Rate Model
3. 3x Whitepaper Interest Rate Models
4. ComptrollerG2
5. ComptrollerG1
6. CErc20Delegate
7. ERC-20 KNC
8. ERC-20 LINK
9. ERC-20 ZRX
10. ERC-20 WBTC [FaucetToken]
11. ERC-20 USDT
12. ERC-20 USDC [FaucetToken]
13. ERC-20 SAI [FaucetToken]
14. ERC-20 REP [FaucetToken]
15. ERC-20 DAI [FaucetToken]
16. ERC-20 BAT
17. Fauceteer
18. Simple Price Oracle
19. ERC-20 Comp [Comp]
20. TimelockTest
21. Unitroller [Unitroller ---> Comptroller, tagged as Comptroller]
22. CToken cZRX [CErc20Immutable]
23. CToken cWBTC [CErc20Immutable]
24. CToken cUSDT [CErc20Delegator]
25. CToken cUSDC [CErc20Immutable]
26. CToken cETH [CEther]
27. CToken cSAI [CErc20Immutable]
28. CToken cREP [CErc20Immutable]
29. CToken cDAI [CErc20Delegator]
30. CToken cBAT [CErc20Immutable]
31. Reservoir
32. GovernorAlphaHarness
33. Maximillion

## Constructors

1. The `Comp` constructor receives an account which to grant the inital supply to, in which case is set to the sender (i.e. the deployer).

2. The `TimelockTest` constructor is passed the sender (i.e. deployer) address as the admin, and `60e0` (i.e. 60) as the delay.

3. Each `CToken` uses the underlying ERC-20 address, the Unitroller address, Interest Rate Model address, and an initial exchange rate. The `CErc20Delegator` tokens in particular set an implementation/delegate address to the address of the CErc20Delegate contract from earlier.

4. The `Reservoir` constructor receives the number of tokens to drip per block (in this case, being `Number<5e17>`), the token to drip (Comp), and the address to drip to (the Unitroller address).

5. The `GovernorAlphaHarness` constructor is passed the TimelockTest contract address, the Comp contract address, and the sender (i.e. the deployer) address as the guardian.

6. The `Maximillion` constructor simply receives the cETH address.

## Transaction Executions

* After the USDT TetherToken is deployed, `TetherToken.issue()` is called with `Number<5737970410e6>` being the argument (issuing that many tokens to the owner).

* After the `SimplePriceOracle` is deployed, each of the ERC-20 tokens are set to a proper price: for example, BAT is set to "base: 67126, exp: 10", translating to:
  * SimplePriceOracle.setDirectPrice(address of BAT, 671260000000000)
  * Note that the contract is first deployed, before sending a transaction to set each underlying price

* After the `Comp` token is deployed, the delegator for the deployer is set to the deployer address, translating to:
  * Comp.delegate(address of deployer)

* After the `Unitroller` is deployed, the following transactions are executed:
  * Unitroller.\_setPendingImplementation(address of ComptrollerG1)
  * ComptrollerG1.\_become(Unitroller, SimplePriceOracle, Number<5e17>, Number<20e0>, false)
  * Unitroller.\_setPriceOracle(address of SimplePriceOracle)
  * Unitroller.\_setMaxAssets(Number<20e0>)
  * Unitroller.\_setCloseFactor(Number<5e17>)

* After the CTokens have been deployed, a transaction is run, for each ERC-20, to transfer a high number of tokens from the current deployer (i.e. `msg.sender`) to the Fauceteer contract address
  * Each are generally [ERC-20].transfer(address of Fauceteer, amount)

* After the `Reservoir` is deployed, two transfers are executed on the Comp ERC-20: 
  * `1e24` to the Fauceteer as Comp.transfer(address of Fauceteer, 1e24)
  * `4e24` to the Reservoir as Comp.transfer(address of Reservoir, 4e24)

* After the `GovernorAlphaHarness` is deployed, the admin of the TimeLockTest is set to the address of the `GovernorAlphaHarness`:
  * TimelockTest.harnessSetAdmin(address of GovernorAlphaHarness)

* After the TimelockTest.harnessSetAdmin() send transaction, the Unitroller is called to support each cToken market and to set each collateral factor. An example with cBAT:
  * Unitroller.\_supportMarket(address of cBAT)
  * Unitroller.\_setCollateralFactor(address of cBAT, 600000000000000000)
