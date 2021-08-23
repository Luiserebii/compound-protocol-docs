# Introduction

![Compound Logo](./img/compound-comp-logo.png)

This is a thorough guide to everything related to deploying Compound protocol to an Ethereum network, such as a public testnet (e.g. Rinkeby, Ropsten). Compiling the Compound protocol contracts, deploying the Compound protocol contracts, and any information that is not obvious to at first glance to a newcomer to the Compound protocol repositories will be addressed in this guide.

This guide is written for the [`compound-protocol`](https://github.com/compound-finance/compound-protocol) and the [`compound-eureka`](https://github.com/compound-finance/compound-eureka) GitHub repositories' master branches as of writing. Therefore, we will be referring to the following commits for each repository:
  * `compound-protocol`: [a6251ecc30adb8dfa904e2231e9a9fdb4eea78be](https://github.com/compound-finance/compound-protocol/tree/a6251ecc30adb8dfa904e2231e9a9fdb4eea78be)
  * `compound-eureka`: [4c58f3bfdd08c74926f0c0e0c0f2eabac78b8b83](https://github.com/compound-finance/compound-eureka/tree/4c58f3bfdd08c74926f0c0e0c0f2eabac78b8b83)

## On the Guide

This guide assumes an installation of a modern Debian distribution: specifically, this guide has been written testing Ubuntu Linux 20.04 LTS. This guide will work similarly for other UNIX-like operating systems (e.g. Arch, macOS), but will require some knowledge of the system's package manager and other specifics.

## Compound Protocol

Compound is a decentralized protocol which establishes pools of assets with algorithmically-calculated interest rates, allowing for the supplying and borrowing of assets. If you do not know very much about Compound, check out the [Compound Whitepaper](https://compound.finance/documents/Compound.Whitepaper.pdf) and the [Compound Protocol Specification](https://github.com/compound-finance/compound-protocol/tree/master/docs/CompoundProtocol.pdf) to learn more.



