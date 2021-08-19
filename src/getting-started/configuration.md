# Configuration

`compound-protocol` and `compound-eureka` require a private key for any deployment-related scripts, and an Etherscan API key, if you'd like to verify those deployed contracts on Etherscan. Luckily, the configuration for both are very similar.

## Private Key

For setting a private key to run transactions from, you have two options:
1. Save the private key in a file named after the network within `~/ethereum`
2. Pass the `ACCOUNT`/`pk` environment variable to the command

I recommend stashing the private key in a file. For example, to save a private key `80bbfb503fb22fc4de19bac1e42cbd5983d8ed26b5c04884b142e6ddf6140eef` for usage for the Rinkeby network, one may run:
```sh
mkdir -p ~/.ethereum
echo '80bbfb503fb22fc4de19bac1e42cbd5983d8ed26b5c04884b142e6ddf6140eef' > ~/.ethereum/rinkeby
```

## Etherscan API Key

To have the script verify a contract on Etherscan after deployment, the API key must be passed through an environment variable. I suggest stashing the API key in your `~/.bashrc` and referring to it from there. For example, to save an API key `7C0WA5ES3176894W6APLTH0K9T6S0M1V3F`, one may run:
```sh
echo -e '\nETHERSCAN_API_KEY=7C0WA5ES3176894W6APLTH0K9T6S0M1V3F' >> ~/.bashrc
```

Don't forget to run `. ~/.bashrc` to reload our new configuration into the current shell environment!
