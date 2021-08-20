# Under the Hood

This section is intended to outline the Compound protocol deployment executions at a lower level. Instead of discussing the Eureka interface, the Ethereum transactions emitted by Eureka will be explored. This may be useful if you are looking to rewrite the Compound deployment scripts, as it will give you a sense of how to structure your code.

## Terminology

Compound Core refers to the contracts deployed by Eureka when specifying the `compound.eureka` and `testnet.eureka` files in the `eureka` directory, and Compound Core + Governance refers to the contracts deployed by Eureka when specifying, in addition to the two former mentioned, `testnet-gov.eureka`.

## Notation

`Order of Deployment` specifies the contracts deployed in order. Names in brackets refer to the actual contract name, and an arrow `--->` designates the special relationship in which execution is "proxied" to another contract for implementation. In the only spot it is used, it is intended to show that the Unitroller delegates execution to ComptrollerG1.


