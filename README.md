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

Install necessary dependencies (the correct version is [1.2.3](https://stackoverflow.com/questions/66735307/truffle-contract-deployment-failed-invalid-sender)):

```
npm install @truffle/hdwallet-provider@1.2.3
```

Notice, it requires mnemonic to be passed in for Provider, this is the seed phrase for the account you'd like to deploy from. Create a new **.secret** file in root directory and enter your private key to get started. If you don't know how to do that, consult the official [Metamask guide](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key).

And then:

```
truffle migrate --network testnet
```

To deploy to mainnet:

```
truffle migrate --network mainnet
```
