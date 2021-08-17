# Building Compound Protocol

If you are looking to deploy a specific or modified version of the Compound protocol, this section will address the building process of the official Compound protocol repository! Note that Compound Eureka (the Compound deployment scripts) already have built versions of Compound committed into the repository, so this section can be skipped.

## Prerequesites

Node vXX.X, npm, and yarn!

## Initial setup

First, let's clone the GitHub repository:

```
git clone https://github.com/compound-finance/compound-protocol
cd compound-protocol
yarn install --lock-file
```

## Troubleshooting

### `Unknown option: p`

When running one of the package scripts, like `yarn test`, the output errors and stops, displaying:

```
Unknown option: p
Type shasum -h for help
Unknown option: p
Type shasum -h for help
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

As of the commit written to this guide, this fix has been merged as of [eb7a6c9](https://github.com/compound-finance/compound-protocol/commit/eb7a6c9831198a19736bc4c1f8f66d41b98f4eaf), but as the error appeared during the writing of this guide, it will be mentioned. Chances are, if you have this error, you have checked out an earlier commit of Compound protocol.

To fix this error which is relevant to a newer version of `shasum`, make the changes to the files described in this pull request: [https://github.com/compound-finance/compound-protocol/pull/80/files](https://github.com/compound-finance/compound-protocol/pull/80/files).
