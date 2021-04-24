# animated-potato

First of all, we need install the [truffle framework](https://www.trufflesuite.com/truffle)

```
npm install -g truffle
```

After it, we create the working sets on the current directory

```
truffle init
```

Also, you'll need to install the [svm](https://github.com/web3j/svm) command, to facilitate the management of multiple solidity compilers on your environment:

```
curl -L https://github.com/web3j/svm/raw/master/install.sh | bash && source ~/.svm/svm.sh
```

This smart contract [contracts/animated-potato.sol](https://github.com/richizo/animated-potato/contracts/animated-potato.sol) use the solidity **0.7.5**:

```
svm install 0.7.5 && svm use 0.7.5
```

Adjust the **truffle-config.js** to point to correctness version of solidity

```
  compilers: {
    solc: {
      version: "0.7.5",
      docker: false,
      settings: {
        optimizer: {
          enabled: false,
          runs: 200
        }
      }
    }
  },
```

Now, it's time to deploy, but first you may want to adjust the **truffle-config.js** as follow:

```
const HDWalletProvider = require('@truffle/hdwallet-provider');
const fs = require('fs');
const mnemonic = fs.readFileSync(".secret").toString().trim();

module.exports = {
  networks: {
    testnet: {
      provider: () => new HDWalletProvider(mnemonic, `https://data-seed-prebsc-1-s1.binance.org:8545`),
      network_id: 97,
      confirmations: 10,
      timeoutBlocks: 200,
      skipDryRun: true
    },
  }
...
```

And then:

```
truffle migrate --network testnet
```