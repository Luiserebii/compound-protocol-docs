# compound-eureka

## `Transaction timeout`

This can occur if the transaction fails to confirm on time during the script. To prevent this, you can [increase the gas price manually](../deploy-compound/tips.md#increasing-gas-price).

## `yarn eureka apply` breaks partway

Sometimes when deploying, `yarn eureka apply` will break partway through deployment with an error such as `Error: Returned error: nonce too low`, or an assertion breaking, or something else entirely. `compound-eureka` is still buggy, and the errors tend to suggest that Eureka sends a transaction too soon: the previous one has not finished resolving/confirming, so the call to web3.js produces a transaction with a nonce that is too low, or a state which has not finished updating from the last transaction.

Typically when it breaks like this, you are given the option to save to state. You may save the deployment data to state and try to run the deployment again, and observe where it picks up; sometimes, it may skip some steps when starting again, so it is often safer to clean the state and start again. 

This is unfortunately a bug in Eureka that has to be worked around. [Setting the gas price manually](../deploy-compound/tips.md#increasing-gas-price) is one way to attempt to avoid this error. 

If the recurrent breaking is too difficult to tolerate, you can also attempt to write a deployment script yourself. It may help to look at the [Under the Hood](../under-the-hood) section to learn about the low-level executions ran by Eureka.

## General Failsafe

Any other issues may be fixed by [cleaning the repo of the state file](../deploy-compound/tips.md#cleaning-deployment-state), which will allow Eureka to run fresh.
