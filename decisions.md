# Decisions

This file is the history of the project. Append an entry when a choice changes the work: the quantity, the data, the split, the model, the loss, or the action. Leave old entries in place. A later entry can reverse an earlier one. If a result depends on a random draw, record the seed under Why. The split entry records the cutoff date, or the seed and the fraction. The pull request that merges the code names this entry.

## 2026-10-29: Pass unless the forecast clears the closing 2.5 price

- Decision. Forecast \(P(Q > 2.5)\) for each Premier League match, with \(Q\) the full-time total in goals, and stake one unit only when that probability clears the Bet365 closing price: the over when \(p > 1/o_{\text{over}}\), the under when \(1 - p > 1/o_{\text{under}}\), and a pass otherwise. Score the rule by mean profit per match on the 2024/25 season, 16 August 2024 through 25 May 2025.
- Why. The same Football-Data `E0` file has the final score and both closing prices. The line is fixed at 2.5, so a push does not occur. On the 2,279 matches with a usable close, the two prices sum to more than a fair book (mean overround about 1.05), so a stake placed before the probability clears \(1/o\) pays that margin. A pass earns 0. A winning unit pays \(o - 1\); a losing unit costs 1. Passing when an edge was real only misses profit.
- Alternative. Bet a side on every match, including betting the shorter closing price, without requiring the forecast to clear \(1/o\).
- Revisit. Change the rule if, on the 380 held-out matches, a stake that does not clear the price earns a higher mean profit per match than passing, or if the model never clears the price and the holdout cannot separate it from a pass.
