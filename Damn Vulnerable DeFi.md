Damn Vulnerable DeFi is the wargame to learn offensive security of DeFi smart contracts in Ethereum.

Featuring flash loans, price oracles, governance, NFTs, DEXs, lending pools, smart contract wallets, timelocks, and more!

# Challenges

|#|Name|
|---|---|
|1|[[1 Unstoppable]]|
|2|[[2 Naive receiver]]|
|3|[[3 Truster]]|
|4|[[4 Side Entrance]]|
|5|[[5 The Rewarder]]|
|6|[[6 Selfie]]|
|7|[[7 Compromised]]|
|8|[[8 Puppet]]|
|9|[[9 Puppet V2]]|
|10|[[10 Free Rider]]|
|11|[[11 Backdoor]]|
|12|[[12 Climber]]|
|13|[[13 Wallet Mining]]|
|14|[[14 Puppet V3]]|
|15|[[15 ABI Smuggling]]|

# How to play

1. Clone [the repository](https://github.com/tinchoabbate/damn-vulnerable-defi/tree/v3.0.0)
2. Checkout the latest version with `git checkout v3.0.0`
3. Install dependencies with `yarn`
4. Code your solution in the `*.challenge.js` file (inside each challenge's folder in the `test` folder)
5. Run the challenge with `yarn run challenge-name`. If the test is executed successfully, you've passed!

## Tips

- To code the solutions, you may need to read [Ethers](https://docs.ethers.io/v5/single-page/) and [Hardhat](https://hardhat.org/getting-started/) docs.
- In all challenges you must use the account called `player`. In Ethers, that may translate to using [`.connect(player)`](https://docs.ethers.io/v5/single-page/#/v5/api/contract/contract/-%23-Contract-connect).
- Some challenges require you to code and deploy custom smart contracts.
- Go [here](https://github.com/tinchoabbate/damn-vulnerable-defi/discussions/categories/support-q-a-troubleshooting) for troubleshooting, support and Q&A.