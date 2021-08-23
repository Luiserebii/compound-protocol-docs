# Automate Setup via Script

If you would like to automate the installation of required packages, configuration, and initial setup of both Compound protocol repositories, you may use the `compound-setup.sh` script located [here](https://raw.githubusercontent.com/Luiserebii/compound-protocol-docs/master/files/compound-setup.sh). This may especially be useful if you are installing to a fresh system, like a newly installed Ubuntu 20.04 LTS VPS.

This script will perform the following:
  * Install all dependencies as listed in [Installation](./installation.md)
  * Save the given private key in both `~/.ethereum/ropsten` and `~/.ethereum/rinkeby`
  * Save the Etherscan API key in `~/.bashrc` as `ETHERSCAN_API_KEY`
  * Clone `compound-protocol` to the home directory `~/` and install all dependencies
  * Clone `compound-eureka` to the home directory `~/` and install all dependencies

If you use this script, don't forget to reload the shell environment with `. ~/.bashrc`!

## Usage

Simply download the script, give it the proper permissions, and run it, using `-p` to pass the private key for deployment, and `-e` to pass the Etherscan API key. For example, if `80bbfb503fb22fc4de19bac1e42cbd5983d8ed26b5c04884b142e6ddf6140eef` is the private key you'd like to setup, and `7C0WA5ES3176894W6APLTH0K9T6S0M1V3F` is the Etherscan API key you'd like to use, you would run:
```sh
wget https://raw.githubusercontent.com/Luiserebii/compound-protocol-docs/master/files/compound-setup.sh
chmod 755 compound-setup.sh
./compound-setup.sh -p 80bbfb503fb22fc4de19bac1e42cbd5983d8ed26b5c04884b142e6ddf6140eef -e 7C0WA5ES3176894W6APLTH0K9T6S0M1V3F
. ~/.bashrc
```
<script id="asciicast-oyiLT6TaHzXWpoFGPAR7Q18lA" src="https://asciinema.org/a/oyiLT6TaHzXWpoFGPAR7Q18lA.js" async data-autoplay="true"></script>
##### Downloading and running setup script on a fresh Ubuntu Linux 20.04 LTS machine
