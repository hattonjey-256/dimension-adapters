# FAZE review update — September 30, 2026

The candidate reconstructs launch fees from cached LaunchFeeSet history and the verified constructor value, preserving block/log order. It no longer calls historical launchFee state.

## Revenue attribution

- Gross fees, total retained revenue, other-project creator allocations and locked-liquidity allocations are unchanged in all three pinned replay windows.
- Holder revenue now measures successful native fee payouts from the hook to the designated FAZE burner, less failed-payment recredits and actual paid keeper rewards. Unrelated burner deposits do not enter this metric.
- Protocol revenue is retained revenue less net buyback funding. This is attribution, not a treasury cash-balance estimate. Funding of prior earnings and later keeper expenses can produce negative individual periods; totals are not clamped.
- Executed buybacks remain a separate reconciliation measure, not additional fees or revenue.

This follows the fee-source measurement and reclassification guidance in fees/AGENTS.md. It deliberately does not copy all retained revenue into protocol revenue while also counting the same funding for holders.

## Pinned native-USDC figures

Through block 23,418,923 (September 29, 2026 19:33:17 UTC), native USDC at nominal USD 1:

| Metric | Tracked total |
|---|---:|
| Gross fees | 245,702.00 |
| Supplier allocations | 42,908.18 |
| Total retained revenue | 202,793.82 |
| Protocol revenue after net buyback funding | 134,831.58 |
| Net fee-funded buyback allocation | 67,962.24 |
| Executed buybacks | 67,921.22 |
| Unspent burner balance | 41.02 |

Additional FAZE-denominated fee/volume amounts remain in SDK balances and are not included in this native-USDC table. SDK USD pricing can therefore differ.

The reconstructed creator routing identifies only the FAZE token pool as the burner's hook fee source. Successful source payouts total 68,166.612355475646668885 native USDC; rewards total 204.376783859557057521. At the earlier burn snapshot, payouts were 68,149.431161144132030123 and the balance was 23.836541291779501719. Fee receipts minus burns and rewards match native balances at BOTH cutoffs exactly. No burns occurred in the gap between cutoffs. This proves net reconciliation, not a complete internal-transfer trace audit; the RPC does not expose debug_traceTransaction.

## Validation

Passed: ts-check; ts-check-cli; launch-fee constructor/prior-window/same-block tests; source-funding failed payout/retry/unrelated recipient/delayed reward tests; all three raw-token replay windows; fees=retained+suppliers and retained=protocol+holders identities; both historical burner balance checks; merged TVL snapshot replay (9 locked NFTs and 21 compound bands).

Merged TVL replay uses the maintainer's getLogs2 revision and real ethers Result event arguments. Curve reserve fixtures are reconstructed from events, so this is not an independent live read of every curve.

Still failing: official live fees test for 2026-09-29, including with DISABLE_PULL_HOURLY=true to reduce fan-out. Public RPC getLogs returns 429, pruned history unavailable, and drpc 400. No successful live fee or merged TVL production run is asserted. The adapter uses SDK providers, not hardcoded endpoints.

## Maintainer assistance needed

Please validate with an Arc historical-log/archive endpoint or the internal indexer, and confirm the source-funding holder-revenue presentation. We have preserved the September 13 start date and failure propagation rather than dropping old data or returning zero on RPC failures. TVL PR #21323 is merged; runtime verification is separate from acceptance of its code.
