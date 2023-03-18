---
description: An overview of Buffer contracts and mechanics.
---

# Architecture

The actors within our software architecture

1. Publisher - Fetches price feeds from some off-chain oracle, signs it, and stores it along with the signature for the keeper to use for opening and closing the trades.
2. Keeper - Takes the signed data from the publisher and runs bots to open and close the trades. The keepers are trustless roles though since whatever data they send in should be signed by the publisher(the admin for now), so they have to send the correct price data.
3. User - Buys options by initiating the trades via router
4. Owner - Can change all the custom attributes in the contracts

### Architecture

A single Router is shared by all the options contracts.&#x20;

Each asset pair has its own options contract linked to a pool(USDC). The pair is independent of Pool's deposit token. In the case of the USDC pool, the user will be paying and winning in USDC.&#x20;

<figure><img src="https://lh6.googleusercontent.com/bPP5mve1niI-LCy2QJSeQyogo3zaXyWGwi20I1XZesvjSXlPiMK5NbMbLWx3xQ5almHQfjgcVO3KHKxN5LEkvKYIw7MhM3o6stfGzMJO-9MPWmPkWthlyYbrDb7DuYM5WPMaSrwiEQVnMzn8fZhUSU4" alt=""><figcaption></figcaption></figure>

#### BufferRouter

This is the main contract, the users and keepers interact with.&#x20;

The users call the initiateTrade() from the router in order to open a trade. This function simply collects the fees from the user for opening his trade and adds the user’s request to the trade queue.

The keeper listens to these requests and calls the resolveQueuedTrades() with one or more trades at a time with the data signed(verified) by the publisher and opens or cancels the trades as per the conditions.

The keeper also keeps track of all the active options. Whenever an option goes from the active to the expired state the keeper runs the unlockOptions() with the data signed(verified) by the publisher and closes it based on the moneyness of the option at the time of expiry.

#### BufferBinaryOptions

This is an ERC721 contract where each option is an NFT.

The router calls this contract to open and close options.

Whenever the router opens a trade,  this contract mints an NFT token for the user and burns it at the time of closing. The fee collected by the router is distributed amongst the BufferBinaryPool and SettlementFeeDisbursal. Additional discounts on the settlement fee are given based on TraderNFTs and referrals.

#### BufferBinaryPool

The BufferBinaryOptions contract sends the premium charged while buying an option to the pool.

Pool locks the liquidity at the time of option buying in order to pay the profit for the ITM options at the time of expiry.

For ITM options at the time of expiry, the locked liquidity is sent as a reward thus resulting in a loss for the pool

For ATM/OTM options at the time of expiry, the locked liquidity is released and nothing is sent to the user. The pool thus makes a profit from the premium collected at the time of option buying

Note: locked liquidity > premium

The admin and other liquidity providers add this liquidity that’s locked in the pool to reward the users for ITM options and make a profit from the OTM and ATM options. This profit is distributed amongst the LP in the ratio of the liquidity added.&#x20;

The pool also has a lockup period of 1 day to prevent LPs from immediate withdrawal after an OTM option.

#### OptionConfig

This contract provides the ability to set/reset all the configurations used by the options contract

### ReferralStorage

This contract stores the referral mapping for who owns which referral code and who referred whom

### SettlementFeeDisbursal

The Settlement Fees charged while buying an option is sent to this contract via BufferBinaryOptions to distribute it amongst the shareholders specified by the admin&#x20;

#### BFR

BFR is the governance token of the platform, it is a regular ERC20 token that can be staked for rewards.

**BLP**

BLP is the liquidity provider token of the platform, it can be minted using any of the tokens within the liquidity pool, currently USDC.

The token’s price is determined by the worth of all tokens within the pool and factoring in the profits and losses of all currently opened positions.

### Technicalities&#x20;

**Signature Validation**

Publisher fetches the prices every second for the required asset pairs from some off-chain oracle and stores it in the DB along with a signature. The keeper picks up this data to open and close the trades. The contract validates this data by generating a signature and checking if the signer is the publisher.

#### Referrals

Referrals allow users to get fee discounts and earn rebates. The referral program has a tier system that helps to ensure that referrers receive the rebates for the users they brought onto the platform.

Anyone can create a Tier 1 code. Admin can upgrade their code to Tier 2 or Tier 3.

#### NFTs and NFT Tiers

Buffer minted 2222 NFTs for the users. Users who own these NFTs can avail of discounts on options trading based on their NFT tiers.

We'll have 4 tiers for the NFTs

Externally we'll name the tiers something else, for eg.

0 --> Silver

1 --> Gold

2 --> Platinum

3 --> Diamond&#x20;

#### Fees

The user pays a fee at the time of option initialization to the router. The router then passes it on to the options contracts if the keeper is able to settle the user’s request.&#x20;

The options contract divides the fee into 2 parts - premium and the SF. The Settlement Fee(SF)(discounted) is calculated based on the NFTs and referrals. The referrer rebate is subtracted from the SF and the remaining SF is sent to the SettlementFeeDisbursal to distribute further amongst the shareholders. The premium is sent to the pool as locked premium.

#### Settlement Fees(Sf)

The contract defines baseSettlementFeePercentageForAbove and baseSettlementFeePercentageForBelow. Sf can be discounted by subtracting (max step reduction \* step size) from the base sf. This max step reduction is defined in the BufferBinaryOptions based on the referral and NFT tiers that the user has.&#x20;

* settlementFeePercentage = (assetCategoryBaseStep - max(userNFTTierStep,  userReferralTierStep)) \* settlementFeeStepSize
* settlement fee =  settlementFeePercentage \* trade size
* For a settlement fee of x%, the payout will be 100 - 2\*x
* lower the settlement fee the higher the payout the user can get
* at 20% settlement fee the user can make 1.6x on their bet
* at 12.5% settlement fee the user can make 1.75x on their bet

Example: base\_sf can be 20% for Crypto or 12.5% for Forex

Now each tier in the NFT has a step reduction associated with it -- let's call it stepReductionViaNFT and each tier in the Referral also has a step reduction associated with it -- let's call it stepReductionViaReferral.

Then we have a stepSize, suppose its value is 2.5

eg 1. then the effective settlement fee for a user trading Forex with a tier 3 NFT (with a stepReduction value of 3) will be effective settlement fee = base sf - (stepReductionsViaNFT \* stepSize) = 12.5 -  (3 \*  2.5) = 5 -- >  Which results in a payout of 1.9x

eg 2. if the user is using a tier 2 referral code (with a step reduction of 2) and a tier 3 NFT (with a stepReduction value of 3) both -> then we'll take the max stepReduction b/w these 2. ie. 3 then again effective settlement fee = base sf - (max(stepReductionsViaNFT, stepReductionsViaReferral) \* stepSize) = 12.5 -  (max(3, 2) \*  2.5) = 5 -- >  Which results in a payout of 1.9x

All these values are configurable so that we can precisely control the effective settlement fee for each case and thus incentivize the use of the referrals and NFTs on our platform based on the Market conditions&#x20;

#### How do settlement fees work?

Suppose a user pays 100 USDC to open an "up"  position on ETH-USD

and their effectiveSettlementFee for this trade was 20%

then we'll take 20% from the amount paid by the user as protocol fees --> 20 USDC

Then we'll transfer the rest (80 USDC) to the pool and then lock another 80 USDC in the pool for the user in case the trade ends ITM (in the money).

If their trade ends ITM then we pay out 160 USDC ( 2 x 80) from the pool to the user.

This means with a 20% effectiveSettlementFee the user can make 1.6x (160 USDC on a 100 USDC trade)

#### PrivateKeeperMode

If True, then only the keepers manually set by the admin using "setKeeper" are allowed. Else anyone can act as a keeper

#### Max Amount

The option size a user buys depends on the pool’s available liquidity. The pool doesn’t allow to lock more x% liquidity for an asset and y% overall. This x% is defined as assetUtilizationLimit and y% is the overallPoolUtilizationLimit. &#x20;

#### Max trade fee

In a single trade, a user can trade at most x% of the pool’s available liquidity. This x% is defined in the contract as optionFeePerTxnLimitPercent. In case the user tries trading for a greater amount if his partial fill is allowed then the trade is opened for the max possible values otherwise the user is refunded.

### Advanced Settings

On the FE the users can find an option to configure the slippage and partial fill.&#x20;

#### Partial Fill

This opens a trade of the max possible value and refunds the rest of the fees back to the user. If the user tries to buy trade for more than what the pool allows then the contract will check if the allowPartialFill flag is true.&#x20;

#### Slippage

In our case slippage is used as the accepted price movement b/w when the user initiates the trade and when the trade is actually opened.

The FE(frontend) is fetching the price feed from some Price APIs(which take a median over multiple exchanges) --> when the user opens the trade from the FE )(launching an initiateTrade txn), this price from the API is used --> then when the keeper opens the trade it provides the price at the blockNumber where the initiateTrade txn was accepted. So there can be a difference b/w the price provided on the FE and that used by the keeper, that is why we check for slippage.\
The trade will be opened at the price provided by the keeper if it's within the slippage with the price provided by the user.&#x20;

#### What is the difference between lockedAmount and lockedPremium on BufferBinaryPool.sol?

In our case -- Locking means that the funds will not be used to underwrite new options.&#x20;

When the user pays 100 USDC for the trade, assuming the settlement fee is 20%, the remaining 80 USDC goes to the pool as lockedPremium  -- this fund is locked until the option expires.

Then another 160 USDC from the pool's balance is locked as lockedAmount until the option expires.

The above lockedPremium is given to the pool as a reward for taking the risk over the lockedAmount

Now 2 cases can happen --&#x20;

1\. if the option ends ITM -- in this case the entire lockedAmount is paid to the option's owner, and the lockedPremium is unlocked -- so the Pools net loss is lockedAmount - lockedPremium on this trade.

2\. if the option ends OTM -- in this case nothing is paid to the option's owner and the pool unlocks both lockedAmount and lockedPremium, and the pool's net profit is lockedPremium on this trade.

### How do keepers work?

We have 2 keepers - one for opening the option trades queued by the users and a second to unlock/exercise the options that have expired. These keepers keep running endlessly at an interval specified in WAIT\_TIME.

In every iteration, the keeper calls a datasource to fetch the options which are to be opened or closed at the moment. This datasource is theGraph's subgraph where we store all our blockchain data. For every option that needs to be executed, the subgraph returns its asset pair and timestamp. The keeper sends a list of \[{asset\_pair, timestamp}] to the publisher endpoints which returns the corresponding price with a signature. The keeper sends a transaction to the blockchain with all this data and the signature is what helps in verifying that the price has come from a trusted publisher.\
