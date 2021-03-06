# KsfSwap V2 Subgraph

[KsfSwap](https://KsfSwap.finance/) is a decentralized protocol for automated token exchange on Kucoin Community Chain.

This subgraph dynamically tracks any pair created by the uniswap factory. It tracks of the current state of KsfSwap contracts, and contains derived stats for things like historical data and USD prices.

- aggregated data across pairs and tokens,
- data on individual pairs and tokens,
- data on transactions
- data on liquidity providers
- historical data on KsfSwap, pairs or tokens, aggregated by day

## Running Locally

Make sure to update package.json settings to point to your own graph account.

## Queries

Below are a few ways to show how to query the uniswap-subgraph for data. The queries show most of the information that is queryable, but there are many other filtering options that can be used, just check out the [querying api](info.kucoinswap.finance/subgraphs/name/ksfswap/ksfswap). These queries can be used locally or in The Graph Explorer playground.

## Key Entity Overviews

#### UniswapFactory

Contains data across all of KsfSwap v2. This entity tracks important things like total liquidity (in KCS and USD, see below), all time volume, transaction count, number of pairs and more.

#### Token

Contains data on a specific token. This token specific data is aggregated across all pairs, and is updated whenever there is a transaction involving that token.

#### Pair

Contains data on a specific pair.

#### Transaction

Every transaction on KsfSwap is stored. Each transaction contains an array of mints, burns, and swaps that occured within it.

#### Mint, Burn, Swap

These contain specifc information about a transaction. Things like which pair triggered the transaction, amounts, sender, recipient, and more. Each is linked to a parent Transaction entity.

## Example Queries

### Querying Aggregated v Data

This query fetches aggregated data from all Pancakeswap pairs and tokens, to capture activity throughout the entire protocol.

```graphql
{
  ksfSwapFactories(first: 1) {
    pairCount
    totalVolumeUSD
    totalLiquidityUSD
  }
}
```
```price
{
  ksfSwapFactories(first: 1) {
    id
    pairCount
    totalVolumeUSD
    totalVolumeKCS
  }
  
  bundles(first: 1) {
   id
    kcsPrice
  }
}

```
```token
{
  ksfSwapFactories(first: 1) {
    id
    pairCount
    totalVolumeUSD
    totalVolumeKCS
  }
  tokens(first: 1) {
    id
    symbol
    name
    decimals
  }
}

```
```Admin queries

{
  indexingStatuses(subgraphs: ["QmXKNYHpgYYfdpR6DztvXymnjTLWqNLf39cNxF9GiRx8mH"]) {
    subgraph
    synced
    health
    entityCount
    fatalError {
      handler
      message
      deterministic
      block {
        hash
        number
      }
    }
    chains {
      chainHeadBlock {
        number
      }
      earliestBlock {
        number
      }
      latestBlock {
        number
      }
    }
  }
}
```
