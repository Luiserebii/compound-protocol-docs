# Compound Core

The transactions outlined are the ones executed when specifying the `eureka/compound.eureka` and `eureka/testnet.eureka` files. They thus correspond to the command:

```sh
yarn eureka apply -n ropsten -b ./.build -c config/*.js -e eureka/{compound,testnet}.eureka
```

## Order of Deployment

1. Compound Lens
2. Jump Rate Model
3. 3x Whitepaper Interest Rate Models
4. ComptrollerG1
5. ComptrollerG2
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
19. Unitroller [Unitroller ---> Comptroller, tagged as Comptroller]
20. CToken cZRX [CErc20Immutable]
21. CToken cWBTC [CErc20Immutable]
22. CToken cUSDT [CErc20Delegator]
23. CToken cUSDC [CErc20Immutable]
24. CToken cETH [CEther]
25. CToken cSAI [CErc20Immutable]
26. CToken cREP [CErc20Immutable]
27. CToken cDAI [CErc20Delegator]
28. CToken cBAT [CErc20Immutable]
29. Maximillion

## Constructors

1. Each `CToken` uses the underlying ERC-20 address, the Unitroller address, Interest Rate Model address, and an initial exchange rate. The `CErc20Delegator` tokens in particular set an implementation/delegate address to the address of the CErc20Delegate contract from earlier.

2. The `Maximillion` constructor simply receives the cETH address.

## Transaction Executions

* After the USDT TetherToken is deployed, `TetherToken.issue()` is called with `5737970410e6` being the argument (issuing that many tokens to the owner).

* After the `SimplePriceOracle` is deployed, each of the ERC-20 tokens are set to a proper price: for example, BAT is set to "base: 67126, exp: 10", translating to:
  * SimplePriceOracle.setDirectPrice(address of BAT, 671260000000000)
  * Note that the contract is first deployed, before sending a transaction to set each underlying price

* After the `Unitroller` is deployed, the following transactions are executed:
  * Unitroller.\_setPendingImplementation(address of ComptrollerG1)
  * ComptrollerG1.\_become(Unitroller, SimplePriceOracle, Number<5e17>, Number<20e0>, false)
  * Unitroller.\_setPriceOracle(address of SimplePriceOracle)
  * Unitroller.\_setMaxAssets(Number<20e0>)
  * Unitroller.\_setCloseFactor(Number<5e17>)

* After the CTokens have been deployed, a transaction is run, for each ERC-20, to transfer a high number of tokens from the current deployer (i.e. `msg.sender`) to the Fauceteer contract address

* After the Fauceteer has received all tokens, the Unitroller is called to support each cToken market and to set each collateral factor. An example with cBAT:
  * Unitroller.\_supportMarket(address of cBAT)
  * Unitroller.\_setCollateralFactor(address of cBAT, 600000000000000000)
