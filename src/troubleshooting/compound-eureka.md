# compound-eureka

## `Transaction timeout`

This can occur if the transaction fails to confirm on time during the script. To prevent this, you can increase the Gwei manually (see file **do proper linking for this**)

## `yarn eureka apply` breaks partway

Sometimes when deploying, `yarn eureka apply` will break partway through deployment with an error such as `nonce too low`, or an assertion breaking, or something else entirely. `compound-eureka` is still buggy, and the errors tend to suggest that Eureka sends a transaction too soon: the previous one has not finished resolving/confirming, so the call to web3.js produces a transaction with a nonce that is too low, or a state which has not finished updating from the last transaction.

Typically when it breaks like this, you are given the option to save to state. You may save the deployment data to state and try to run the deployment again, and observe where it picks up; sometimes, it may skip some steps when starting again, so it is often safer to clean the state and start again. 

This is unfortunately a bug in Eureka that has to be worked around. Setting the gas price manually (**also do linking for this**) is one way to attempt to avoid this error. 

If the recurrent breaking is too difficult to tolerate, you can also attempt to write a deployment script yourself. **LINK FOR read here to learn more about the deployment, under the hood***

## General Solution

**link to tip about cleaning deployment state**
