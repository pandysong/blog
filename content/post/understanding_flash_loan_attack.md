---
date: 2017-04-12
title: understanding flash loanx attack (an example)
weight: 10
---


# Introduction

This post tries to understand how the transaction is done on Etherum for a real
flash loan attack.


# A real Trasaction


```

https://etherscan.io/tx/0x762881b07feb63c436dee38edd4ff1f7a74c33091e534af56c9f7d49b5ecac15

```

This trasaction has one top level contract:

https://etherscan.io/address/0x77f973fcaf871459aa58cd81881ce453759281bc

## `From` address

`From` address is the address who execute function in this contract.

```
0xb8c6ad5fe7cb6cc72f2c4196dca11fbb272a8cbf
```

## Contract internal trasactions

Seeing from https://etherscan.io/tx/0x762881b07feb63c436dee38edd4ff1f7a74c33091e534af56c9f7d49b5ecac15#internal

```
The contract call From 0xb8c6ad5fe7cb6cc72f2c4196dca11fbb272a8cbf To 0x77f973fcaf871459aa58cd81881ce453759281bc produced 65 Internal Transactions
```

### First Trasaction

This internal trasaction get 7500 ETH via the contract 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2.

```
Address
0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2
Name
Transfer (index_topic_1 address src, index_topic_2 address dst, uint256 wad)View Source

Topics

0 0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef
1  0x77f973fcaf871459aa58cd81881ce453759281bc
2  0x360f85f0b74326cddff33a812b05353bc537747b
Data
    wad : 7500000000000000000000

```

Above event 'Transfer' is part of contact 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2's
function call transfer or transferFrom.

https://etherscan.io/address/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2#code

As we could see from the contract code from above link:

```
/**
 *Submitted for verification at Etherscan.io on 2017-12-12
*/

// Copyright (C) 2015, 2016, 2017 Dapphub

// This program is free software: you can redistribute it and/or modify
// it under the terms of the GNU General Public License as published by
// the Free Software Foundation, either version 3 of the License, or
// (at your option) any later version.

// This program is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.

// You should have received a copy of the GNU General Public License
// along with this program.  If not, see <http://www.gnu.org/licenses/>.

pragma solidity ^0.4.18;

contract WETH9 {
    string public name     = "Wrapped Ether";
    string public symbol   = "WETH";
    uint8  public decimals = 18;

    event  Approval(address indexed src, address indexed guy, uint wad);
    event  Transfer(address indexed src, address indexed dst, uint wad);
    event  Deposit(address indexed dst, uint wad);
    event  Withdrawal(address indexed src, uint wad);

    mapping (address => uint)                       public  balanceOf;
    mapping (address => mapping (address => uint))  public  allowance;

    function() public payable {
        deposit();
    }
    function deposit() public payable {
        balanceOf[msg.sender] += msg.value;
        Deposit(msg.sender, msg.value);
    }
    function withdraw(uint wad) public {
        require(balanceOf[msg.sender] >= wad);
        balanceOf[msg.sender] -= wad;
        msg.sender.transfer(wad);
        Withdrawal(msg.sender, wad);
    }

    function totalSupply() public view returns (uint) {
        return this.balance;
    }

    function approve(address guy, uint wad) public returns (bool) {
        allowance[msg.sender][guy] = wad;
        Approval(msg.sender, guy, wad);
        return true;
    }

    function transfer(address dst, uint wad) public returns (bool) {
        return transferFrom(msg.sender, dst, wad);
    }

    function transferFrom(address src, address dst, uint wad)
        public
        returns (bool)
    {
        require(balanceOf[src] >= wad);

        if (src != msg.sender && allowance[src][msg.sender] != uint(-1)) {
            require(allowance[src][msg.sender] >= wad);
            allowance[src][msg.sender] -= wad;
        }

        balanceOf[src] -= wad;
        balanceOf[dst] += wad;

        Transfer(src, dst, wad);

        return true;
    }
}

```
