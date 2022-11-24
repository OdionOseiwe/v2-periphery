# Uniswap V2

[![Actions Status](https://github.com/Uniswap/uniswap-v2-periphery/workflows/CI/badge.svg)](https://github.com/Uniswap/uniswap-v2-periphery/actions)
[![npm](https://img.shields.io/npm/v/@uniswap/v2-periphery?style=flat-square)](https://npmjs.com/package/@uniswap/v2-periphery)

In-depth documentation on Uniswap V2 is available at [uniswap.org](https://uniswap.org/docs).

The built contract artifacts can be browsed via [unpkg.com](https://unpkg.com/browse/@uniswap/v2-periphery@latest/).

# Local Development

The following assumes the use of `node@>=10`.

## Install Dependencies

`yarn`

## Compile Contracts

`yarn compile`

## Run Tests
`yarn test`


```solidity
function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    )public virtual override ensure(deadline) returns (uint amountA, uint amountB) {
        address pair = UniswapV2Library.pairFor(factory, tokenA, tokenB);
        IUniswapV2Pair(pair).transferFrom(msg.sender, pair, liquidity); // send liquidity to pair
        /// when removing liquidity the following happens
        /// -first the tokens LP tokens are transferred into the contract
        /// -secondly when the burn function is called it gets the amount to tokens that should
        /// be transferred back to the LP
        (uint amount0, uint amount1) = IUniswapV2Pair(pair).burn(to);
        (address token0,) = UniswapV2Library.sortTokens(tokenA, tokenB);
        (amountA, amountB) = tokenA == token0 ? (amount0, amount1) : (amount1, amount0);
        require(amountA >= amountAMin, 'UniswapV2Router: INSUFFICIENT_A_AMOUNT');
        require(amountB >= amountBMin, 'UniswapV2Router: INSUFFICIENT_B_AMOUNT');
    }

```