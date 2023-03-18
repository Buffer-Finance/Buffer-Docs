---
description: Overview of all Buffer system configurations
---

# All Configs

### Global Parameters

#### Referral Discounts

| Tier   | Payout Boost(additional payout over base payout) | Fee Share(% of trade size) |
| ------ | ------------------------------------------------ | -------------------------- |
| Tier 1 | 2%                                               | 0.25%                      |
| Tier 2 | 5%                                               | 0.50%                      |
| Tier 3 | 8%                                               | 0.75%                      |

#### NFT Discounts

| Tier     | Payout Boost (additional payout over base payout) |
| -------- | ------------------------------------------------- |
| Silver   | 2.5%                                              |
| Gold     | 5%                                                |
| Platinum | 8%                                                |
| Diamond  | 12%                                               |

#### Overall Pool Utilization Limit

Percent of the pool's available balance. This is the maximum trade amount a user can open a trade for. Currently, it is **40%**

#### Max transaction limit

Percent of the pool's available balance above which the user cannot buy a trade(max fee). Currently, it is **0.15%**

### Pair Specific Configs

| Pair name | Category | Fee Up | Fee Down | Asset Utilization Limit |
| --------- | -------- | ------ | -------- | ----------------------- |
| BTCUSD    | Crypto   | 15%    | 15%      | 2.00%                   |
| ETHUSD    | Crypto   | 15%    | 15%      | 2.00%                   |
| EURUSD    | Forex    | 10%    | 10%      | 3.00%                   |
| GBPUSD    | Forex    | 10%    | 10%      | 3.00%                   |

### Admin Controlled Parameters

#### Trading

| Name                                | Description                                                                               | Contract            | Function to Change                  |
| ----------------------------------- | ----------------------------------------------------------------------------------------- | ------------------- | ----------------------------------- |
| baseSettlementFeePercentageForAbove | Max settlement fee percentage a user will be charged for opening an UP trade              | BufferBinaryOptions | `configure`                         |
| baseSettlementFeePercentageForBelow | Max settlement fee percentage a user will be charged for opening an DOWN trade            | BufferBinaryOptions | `configure`                         |
| nftTierStep                         | Mapping of NFT tier to the step reduction in the settlement fee percentage                | BufferBinaryOptions | `configure`                         |
| isPaused                            | <p>Used for pausing/unpausing</p><p>trading</p>                                           | BufferBinaryOptions | `toggleCreation`                    |
| maxLiquidity                        | Max pool capacity                                                                         | BufferBinaryPool    | `setMaxLiquidity`                   |
| contractRegistry                    | List of trading contracts with a flag depicting whether or not trading is allowed on them | BufferRouter        | `setContractRegistry`               |
| isInPrivateKeeperMode               | If true only the keepers can open/close the trades. Else anyone can                       | BufferRouter        | `setInPrivateKeeperMode`            |
| isKeeper                            | List of all the keepers                                                                   | BufferRouter        | `setKeeper`                         |
| referrerTier                        | Mapping of address to its referrer tier                                                   | ReferralStorage     | `setReferrerTier`                   |
| referrerTierStep                    | Mapping of referrer tier to the step reduction in the settlement fee percentage           | ReferralStorage     | `configure`                         |
| referrerTierDiscount                | Mapping of referrer tier to the rebate earned over the fees                               | ReferralStorage     | `configure`                         |
| traderNFTContract                   | NFT middleware contract                                                                   | OptionsConfig       | `settraderNFTContract`              |
| settlementFeeDisbursalContract      | Fee distirbutor contract                                                                  | OptionsConfig       | `setSettlementFeeDisbursalContract` |
| assetUtilizationLimit               | Pair wise pool utilization limit                                                          | OptionsConfig       | `setAssetUtilizationLimit`          |
| overallPoolUtilizationLimit         | overall Pool Utilization Limit                                                            | OptionsConfig       | `setOverallPoolUtilizationLimit`    |
| optionFeePerTxnLimitPercent         | Max Trade size as a percentage of Pools Available Balance                                 | OptionsConfig       | `setOptionFeePerTxnLimitPercent`    |
| minFee                              | Minimum trade size                                                                        | OptionsConfig       | `setMinFee`                         |
| maxPeriod                           | Max period of a trade                                                                     | OptionsConfig       | `setMaxPeriod`                      |
| minPeriod                           | Min period of a trade                                                                     | OptionsConfig       | `setMinPeriod`                      |
| marketTimes                         | Mapping of day to its open close time. Only for forex pairs.                              | OptionsConfig       | `setMarketTime`                     |

#### Staking

| Function                     | Variable affected     | Description                                 | Contract                                                                       |
| ---------------------------- | --------------------- | ------------------------------------------- | ------------------------------------------------------------------------------ |
| `updateLastDistributionTime` | lastDistributionTime  | last Distribution Time                      | RewardDistributor / BonusDistributor                                           |
| `setTokensPerInterval`       | tokensPerInterval     | tokens to be distributed per Interval       | RewardDistributor / BonusDistributor                                           |
| `withdrawToken`              | -                     | Withdraws any ERC20 token from the contract | RewardDistributor / BonusDistributor / RewardRouterV2 / Vester / RewardTracker |
| `batchStakeBfrForAccount`    | -                     | Stake multiple at once                      | RewardRouterV2                                                                 |
| `stakeBfrForAccount`         | -                     | Stake BFR for another account               | RewardRouterV2                                                                 |
| `compoundForAccount`         | -                     | Compounds for another account               | RewardRouterV2                                                                 |
| `batchCompoundForAccounts`   | -                     | Compounds multiple at once                  | RewardRouterV2                                                                 |
| `setInPrivateStakingMode`    | inPrivateStakingMode  | Sets/unsets in private mode                 | RewardTracker                                                                  |
| `setInPrivateTransferMode`   | inPrivateTransferMode | Sets/unsets in private mode                 | RewardTracker                                                                  |
| `setHandler`                 | isHandler             | Sets/unsets handlers                        | RewardRouterV2 / Vester / RewardTracker                                        |
| `setDepositToken`            | isDepositToken        | Sets/unsets deposit tokens                  | RewardTracker                                                                  |
| `setInPrivateClaimingMode`   | inPrivateClaimingMode | Sets/unsets in private mode                 | RewardTracker                                                                  |
| `stakeForAccount`            | -                     | Stake for another account                   | RewardTracker                                                                  |
| un`stakeForAccount`          | -                     | unstake for another account                 | RewardTracker                                                                  |
| `claimForAccount`            | -                     | claim for another account                   | RewardTracker                                                                  |
| `setHasMaxVestableAmount`    | hasMaxVestableAmount  | Sets/unsets hasMaxVestableAmount            | Vester                                                                         |
| `depositForAccount`          | -                     | Deposits for another account                | Vester                                                                         |
| `setGov`                     | -                     | Set gov role                                | RewardDistributor / BonusDistributor / RewardRouterV2 / Vester / RewardTracker |
| `setMinter`                  | -                     | Sets minter role                            | esBfr / bnBfr                                                                  |
| `mint`                       | -                     | Mints for account                           | esBfr / bnBfr                                                                  |
