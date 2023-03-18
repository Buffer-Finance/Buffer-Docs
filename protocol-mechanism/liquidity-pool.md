---
description: Learn the basic mechanism of liquidity provisioning on Buffer
---

# Liquidity Pool

The USDC-based liquidity pool acts as a counterparty to all trades on Buffer Finance. Liquidity providers to the BLP can deposit in the [USDC vault](https://app.buffer.finance/#/earn) to earn protocol-earned revenue and esBFR rewards. BLP volume, fees collected, BLP/USDC exchange rate, and other pool metrics can be viewed on the [stats](https://stats.buffer.finance/).

### How does it work?

The Buffer liquidity pool acts as a counterparty to the exotic options market on Buffer. It works by locking up a certain amount of liquidity at the time of option buying, which is then used to pay out profits for in-the-money (ITM) options at the time of expiry.'

When an option is bought, the settlement fee (15%) charged is sent to the pool, and the pool locks up the corresponding liquidity. If the option is ITM at the time of expiry, the locked liquidity is sent as a reward to the option holder. This results in a loss for the pool, as the reward payout is higher than the premium received. However, if the option is at-the-money (ATM) or out-of-the-money (OTM) at the time of expiry, the locked liquidity is released, and nothing is sent to the user. The pool then makes a profit from the premium collected at the time of option buying.

It's important to note that the locked liquidity is greater than the premium received, which helps to ensure that the pool has enough liquidity to cover potential reward payouts for ITM options. The profit earned by the pool is distributed among the liquidity providers in proportion to the liquidity they have added.

### Risks

* Smart contract risks: audits, bug bounties, and internal testing mitigate systemic risks, it is still important to always DYOR and be cautious of risks with any smart contract or decentralized application or complex systems;
* Counterparty risk: since the BLP pool acts counterparty to all options trades, whenever a trader makes a profitable trade, the profit is delivered from the BLP pool.

### Risk Management

Parameters implemented to manage risk associated with long tails events for liquidity providers:

1. Max utilization — Max vault utilization at a time is capped at 40% of the total available liquidity (will be increased gradually as protocol volume grows);
2. Max utilization per asset — 2–3% of the total available liquidity (Depends on the asset);
3. Separate Payout (%) for Up and Down (will be kept the same for all assets initially but can be changed in case of unidirectional moves in the market);
4. Max utilization per trade — 0.25% of the available liquidity;
5. Max limit — the maximum amount of liquidity that can be provided to the USDC vault at any given time is currently capped at 2M USDC (will be increased gradually as protocol volume grows)
