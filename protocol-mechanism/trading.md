---
description: Learn the mechanism of trading on Buffer
---

# Trading

Trading on Buffer can currently be executed against the USDC Based liquidity pool. Trades can be placed in a non-custodial manner, allowing traders to remain in custody of their assets even a trade is open.&#x20;

* Available Timeframes: 5 mins, 15 mins, 1 hr, 4 hrs
* Asset Classes: Crypto, forex
* Trading Pairs: Find all pairs available for Up/Down trading on the [Pair list](../products/up-down.md#pair-list) **** page

### Trading Parameters - Definitions

* Max Amount: The option size a user buys depends on the pool’s available liquidity. Currently, the maximum liquidity utilization rate is capped at 40%. There is also a maximum utilization rate per asset per direction up/down, which is currently capped at 2% to 3% (depending on the asset pair).
* Max trade size In a single trade, a user can trade at most 0.25% of the pool’s available liquidity. In case the user tries trading for a greater amount of their partial\*\* fill is allowed, then the trade is opened for the max possible values; otherwise, the user is refunded.

\*\* Note: Partial fill is an advanced setting that opens a trade using the max possible value within the implemented max trade size limit. If the user tries to buy trade for more than what the pool allows then the contract will place a trade for the maximum possible value and refunds the rest of the fees back to the user.

### Pricing

The platform uses price feeds from an off-chain oracle for faster and more accurate pricing. On Buffer, slippage is used as the accepted price movement between when the user initiates the trade and when the trade is actually opened.

Price feed fetched from Price APIs(which take a median over multiple exchanges), this price form the API is used. When the [keeper ](../developer-docs/keepers.md)opens the trade, it provides the price at the block number where the transaction was confirmed on-chain.&#x20;

Meaning there can be a difference between the price provided on the frontend and that used by the keeper, which is why we check for slippage. The trade will be opened at a price provided by the keeper if it's within the slippage with the price provided by the user.

### Fees

The cost to open a position is 15% of the position size. Whenever a user opens a trade, Buffer contract mints an NFT token for the user and burns it at the time of closing. 55% of the fee collected is distributed to BLP holders, and 40% is accrued to BFR stakers, while the remaining 5% is distributed to the winners of the [weekly trading competitions](https://app.buffer.finance/#/leaderboard/weekly). Additional discounts on the settlement fee are given of up to 12% are given via [Optopi NFTs](https://optopi.buffer.finance/) and [referrals](https://app.buffer.finance/#/referral?tab=Use+a+Referral).
