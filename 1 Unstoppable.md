There’s a tokenized vault with a million DVT tokens deposited. It’s offering flash loans for free, until the grace period ends.

To pass the challenge, make the vault stop offering flash loans.

You start with 10 DVT tokens in balance.

- [See the contracts](https://github.com/tinchoabbate/damn-vulnerable-defi/tree/v3.0.0/contracts/unstoppable)
- [Complete the challenge](https://github.com/tinchoabbate/damn-vulnerable-defi/blob/v3.0.0/test/unstoppable/unstoppable.challenge.js)
___
# Solution:

This attack can be achieved by exploiting a vulnerability within the `flashLoan` function.

The key vulnerability is found in the following lines of code:

uint256 balanceBefore = totalAssets();  
if (convertToShares(totalSupply) != balanceBefore) revert InvalidBalance();

This code is meant to enforce the [ERC4626 requirement](https://ethereum.org/en/developers/docs/standards/tokens/erc-4626/), which is a standard proposed for tokenized vaults in DeFi. The aim of ERC4626 is to create a standardized approach for handling user deposits and shares within a vault, ultimately determining the rewards for staked tokens.

**Understanding ERC4626 and the Accounting System**

In the context of this challenge, there are two fundamental terms to comprehend: `**assets**` and `**shares**`.

- **The assets** represent the underlying token, in this case, `**DVT**` that users deposit and withdraw from the vault.
- **Shares**, on the other hand, symbolize the vault tokens, denoted as `**oDVT**` minted or burned for users based on the proportion of their deposited assets.

The challenge leverages the `**convertToShares()**` function as per `**ERC4626**`. This function calculates the number of shares the vault should mint, taking the user’s deposited assets into account.

However, two distinct issues come to light:

## Enforcing Equal TotalSupply and TotalAssets

The line `**(convertToShares(totalSupply) != balanceBefore)**` enforces a condition that requires the totalSupply of vault tokens to always match the totalAssets of underlying tokens before any flash loan execution. This condition acts as a safeguard to ensure that the flashLoan function remains inactive if the vault deviates assets to other contracts.

## Tracking Assets and the TotalAssets Function

The `**totalAssets**` function has been overridden to return the vault contract's asset balance (`**asset.balanceOf(address(this))**`). This introduces an alternative accounting system that relies on monitoring the supply of vault tokens.

# The Attack Strategy

The attack strategy revolves around creating a conflict between the two distinct accounting systems. This is achieved by manually transferring `DVT` tokens **directly** to the vault.

This manipulation disrupts the balance and leads to a scenario where the condition `**(convertToShares(totalSupply) != balanceBefore)**` fails.

Consequently, the flashLoan function is deactivated due to the divergence in accounting systems, since the transaction will alwyas be reverted without dependance on the “user” input.

# Executing the Attack

In this challenge we don’t need to deploy any malicious smart contracts to attack the protocol, we simply need to send DVT tokens directly to the pool, we can do so by adding this line of code to the `unstoppable.challange.js` file in the “Execution” section:
```js
// Send 1 (or any non 0 value) wei of DVT from the player directly to the pool  
await token.connect(player).transfer(vault.address, 1)
```

Now we can execute the challenge and see that we were able to pass it:
```js
yarn run unstoppable
```