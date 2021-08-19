# Deploying Compound Protocol

`compound-eureka` uses Eureka ([@compound-finance/eureka](https://www.npmjs.com/package/@compound-finance/eureka/v/1.0.3)) to author the smart contract deployment description, logic, and flow. Through passing `.eureka` files stored in the `./eureka` directory to `yarn eureka apply`, we can specify which parts of the protocol we'd like to deploy. For more on Eureka, see the README.md documenation included with the npm package.

## On `.eureka`

One peering over the `.eureka` files (such as `./eureka/compound.eureka`) will notice that some contracts have a string within square brackets (`[]`). Although this is undocumented, I believe it refers to the name of the `.json` file blob to refer to within the `.build` folder, which appears to have the contract ABI and pre-compiled bytecode built in. For example, `[b73e68c]` looks for the named contract within `./.build/b73e68c.json`.

If you are looking to use a custom build of Compound, you will want to note that you will likely have to make a copy of the appropriate `.eureka` file and modify them to refer to your built `.json`.
