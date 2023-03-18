# Fee distribution

The fee paid at the time of opening a trade goes to the [Settlement Fee Disbursal(SFD) contract](https://automation.chain.link/arbitrum/109303885389665712720457602338769546314168992214556194478546599960107579106479). SFD contract transfers this fee to the reward distributor contracts of staking.

* 40% goes to the bfrDistributor
* 55% goes to the blpDistributor

The protocol uses the [chainlink keeper](https://automation.chain.link/arbitrum/109303885389665712720457602338769546314168992214556194478546599960107579106479) to distribute the fee accumulated in the SFD contract to the respective distributors every Wednesday at 12:00 UTC.
