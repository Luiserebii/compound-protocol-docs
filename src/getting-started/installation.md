# Installation

## Installing via apt

First, let's refresh our package list:
```sh
sudo apt-get update
```

Next, install each of the following, as needed:

### make
```sh
sudo apt-get install -y make
```

### libusb and libudev
```sh
sudo apt-get install -y libusb-1.0-0-dev libudev-dev
```

### Node 13.x
```sh
curl -fsSL https://deb.nodesource.com/setup_13.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### yarn
```sh
npm install --global yarn
```

### solc v0.5.16
```sh
sudo wget https://github.com/ethereum/solidity/releases/download/v0.5.16/solc-static-linux -O /usr/local/bin/solc
sudo chmod +x /usr/local/bin/solc
```

## Verifying Correct Packages

After installing, it may help to verify that your packages have installed correctly. Try running `node -v` and `solc --version`, and check to see if their versions match.

Here is an example of output for correctly installed Node v13.x:
```sh
root@ubuntu-s-2vcpu-4gb-intel-tor1-01:~# node -v
v13.14.0
```
Note: It's fine if your installation doesn't have matching minor versions or patch versions: as long as the major version is 13, i.e. `v13.x.x`, the installation is correct.

And example output for correctly installed solc v0.6.16:
```sh
root@ubuntu-s-2vcpu-4gb-intel-tor1-01:~# solc --version
solc, the solidity compiler commandline interface
Version: 0.5.16+commit.9c3226ce.Linux.g++
```
