# Proposed FAZE methodology

Scope: initial September 13, 2026 release 9 on Arc mainnet. Proposed for maintainer review.

## TVL
Discover launches/pools from events. Read each coin's real ethReserve at the requested block. Exclude virtual reserves, unclaimed fees, treasury balances and minted launch-token inventory. Value quote-side liquidity of graduation NFT positions after verifying dead-address ownership, plus hook-owned compounding bands, using integer Q96 position math. Exclude FAZE-denominated quote assets and FAZE own-token pools from core TVL. Disclose Uniswap v4 overlap with doublecounted=true.

## Volume
Bonding curves only: Bought.ethGross plus Sold.ethOut + Sold.fee. Count one quote leg. Exclude post-graduation Uniswap swaps, transfers and liquidity additions. Preserve quote-token identities and raw units; SDK pricing determines displayed USD. The official runner may use current prices for recent windows and historical prices for older windows, so daily and hourly USD estimates can differ despite identical raw amounts.

## Gross fees
Curve trade fees from Bought/Sold; flat launch fees from the verified initial fee and chronological changes; migration fees from Graduated. Hook fees are FeesSettled plus closing minus opening accrued balances. Claims are not new fees. Native USDC uses 18-decimal gas-token units, not six-decimal ERC20-view units.

## Supply-side costs and retained revenue
Creator fees from other tokens and fees allocated to locked liquidity are supply-side costs. Other tokens' own buybacks remain creator costs, not FAZE holder income. Count settlements and changes in pending entitlements; do not count subsequent claims or compounding again.

Revenue = gross fees minus these supply-side costs. It includes platform treasury allocations and creator fees from FAZE's own pool, identified by the team as the project's own allocation. This does not mean all own-pool allocations were spent on buybacks. ProtocolRevenue is omitted rather than presenting the treasury component as the whole retained revenue split.

Pending entitlements use actual live splits at each boundary. Split changes can reallocate pending amounts and make an individual revenue/cost component negative; the fee/revenue/cost identity remains required.

## Executed FAZE buybacks / holder revenue
Use Burned.ethIn emitted by 0x0379DE4B544c6Ac8927f4BCf71553a5816aF1bd3 in the requested interval. The verified factory's BuybackBurner source measures actual quote spent after refunds and reports keeper bounty separately. Exclude bountyWei. The burned-token amount is supporting evidence, not another dollar revenue entry.

Recognize holder revenue when the buyback executes. Never add it to gross fees or subtract it again from retained revenue: this is a capital allocation that may occur after the underlying fee accrual. The burner can technically receive other funding, so the methodology does not assert every spent unit originated in creator fees. Only the designated FAZE burner is included, not other projects' burner clones.

The creator handover receipt is verified at block 21,224,107. That supports the funding explanation but does not replace execution events with an assumed allocation rate. No claim of a full payout-destination-history audit is made.

## Validation
See evidence/upstream-replay.json, upstream-tvl-replay.json and volume-reconciliation.json. Recorded source snapshot totals retain their original cutoffs. Official live historical fees/TVL tests remain limited by public RPC availability. This proposal does not assert audited net earnings or unverified token-incentive amounts.
