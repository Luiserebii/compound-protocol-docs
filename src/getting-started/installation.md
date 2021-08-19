# Installation

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
