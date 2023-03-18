---
description: BLP is the platform’s liquidity provider token.
---

# Buffer For LPs

Holders of the BLP token earn escrowed BFR (esBFR) rewards and 55% of the platform fees distributed in USDC. Users can mint BLP by providing liquidity to the USDC vault on the [Earn](https://app.buffer.finance/#/earn) page. The BLP token is designed to supply the liquidity required for options trading. As such, BLP holders can earn from the PnL of the pool, whenever a trader makes a positive PnL, the pool loses. On the contrary, the pool makes a profit whenever a trader makes a negative PnL. Past PnL data and other stats can be viewed on the official [dashboard](https://app.buffer.finance/#/dashboard) and [stats](https://stats.buffer.finance/) page.

## Minting/Redeeming BLP

Anyone can mint the BLP tokens by providing liquidity to the Buffer USDC vault. The price for minting and redeeming tokens is calculated based on the following parameters:

* Total volume of assets in the pool, including profits and losses of all previous trades
* BLP token supply

Once the BLP token, is minted, it automatically gets staked and begins to accrue esBFR rewards and trading fees in USDC.

## Probabilistic edge

The pool has a probabilistic edge that can generate a positive yield consistently over the long term. The probability of the price of an asset going up or down from the current price within a short period of time is typically 50%, but the payout offered to the trader is not 1:1 to the pay-in amount.

Here’s an example — Let’s suppose the payout ratio offered by the vault is 1:0.7 (70%), and there are 100 traders, 50 of them bet 100 USDC each that the BTC/USDC would go up in an hour. And the other 50 bets, 100 USDC each, that BTC/USDC will go down within the same time. If BTC/USD ends up after the hour, those who bet that BTC/USD will rise will receive 170 USDC each, while those who bet otherwise end up with zero returns. Thus, the total payout is 9,000 USDC, and the vaults generate 1000 USDC as yield.

## How does the liquidity pool manage risk?

These are some other features to manage risk associated with any long-tail event for the liquidity providers.

* **Max utilization** — Max vault utilization at a time is capped at 40% of the total available liquidity. (Would be increased gradually)
* **Max utilization per asset** — 2–3% of the total available liquidity (Depends on the asset)
* **Separate Payout (%)** - for Up and Down (will be kept the same for all assets initially but can be changed in case of unidirectional moves in the market)
* **Max utilization per trade** — 0.25% of the available liquidity
* **Max limit** — the maximum amount of liquidity that can be provided to the USDC vault at any given time is currently capped at 2M USDC (would be increased gradually)

#### Quick links:

* To provide liquidity, go to [https://app.buffer.finance/#/earn](https://app.buffer.finance/#/earn)
* To learn how to provide liquidity, go to the [how to stake on Buffer](../quick-start-guides/how-to-provide-liqudity.md) guide
