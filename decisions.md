# Decisions

This file is the history of the project. Append an entry when a choice changes the work: the quantity, the data, the split, the model, the loss, or the action. Leave old entries in place. A later entry can reverse an earlier one. If a result depends on a random draw, record the seed under Why. The split entry records the cutoff date, or the seed and the fraction. The pull request that merges the code names this entry.

## 2026-10-29: Pass unless the forecast clears the closing 2.5 price

- Decision. Forecast \(P(Q > 2.5)\) for each Premier League match, with \(Q\) the full-time total in goals, and stake one unit only when that probability clears the Bet365 closing price: the over when \(p > 1/o_{\text{over}}\), the under when \(1 - p > 1/o_{\text{under}}\), and a pass otherwise. Score the rule by mean profit per match on the 2024/25 season, 16 August 2024 through 25 May 2025.
- Why. The same Football-Data `E0` file has the final score and both closing prices. The line is fixed at 2.5, so a push does not occur. On the 2,279 matches with a usable close, the two prices sum to more than a fair book (mean overround about 1.05), so a stake placed before the probability clears \(1/o\) pays that margin. A pass earns 0. A winning unit pays \(o - 1\); a losing unit costs 1. Passing when an edge was real only misses profit.
- Alternative. Bet a side on every match, including betting the shorter closing price, without requiring the forecast to clear \(1/o\).
- Revisit. Change the rule if, on the 380 held-out matches, a stake that does not clear the price earns a higher mean profit per match than passing, or if the model never clears the price and the holdout cannot separate it from a pass.

## 2026-11-18: Hold out the 2024/25 season

- Decision. The cutoff is 19 May 2024, the last match of 2023/24. Training is every usable match from 9 August 2019 through that date (1,899 matches). The holdout is the 2024/25 season, 16 August 2024 through 25 May 2025 (380 matches). The split is a date, not a draw, so there is no seed. Attack and defence are estimated on the training matches only and are not updated with 2024/25 results. A club with no training match is given the average strength.
- Why. The stake is placed before kickoff on a later season. A random fraction of the 2,279 matches would let a 2025 result train the forecast of a 2019 match. The proposal already keeps the 380 matches in `2425` out of the choice of rule.
- Alternative. Draw a random 380/2,279 holdout, or refit after each 2024/25 matchday and forecast only the matches still ahead.
- Revisit. Move the cutoff if the stake is allowed to use earlier matches from the same season. Record the new cutoff in this file.

## 2026-11-18: Independent Poisson strengths

- Decision. Home goals and away goals are independent Poisson counts. For clubs \(h\) and \(a\), \(\log\lambda^H = \mu + \eta + \alpha_h + \delta_a\) and \(\log\lambda^A = \mu + \alpha_a + \delta_h\). \(\mu\) is the log away-goal rate of an average club against an average defence, \(\eta\) is home advantage, \(\alpha\) is attack, and \(\delta\) is defence, with a larger \(\delta\) meaning more goals conceded. The attacks sum to 0 and the defences sum to 0 over the training clubs. \(p = P(Q > 2.5)\) is the probability under that pair of Poissons. The likelihood uses training goals only. The closing prices are not features.
- Why. \(Q\) is a sum of two counts, and the quantity the stake needs is a probability for that sum. A club's scoring and conceding are the mechanism that moves a total across 2.5. The fit starts at \(\mu = \log 1.25\) and zeros elsewhere, so the maximum likelihood estimate does not depend on a seed.
- Alternative. A logistic regression on recent goals, or the de-vigged closing price as \(p\). After the overround that price is a convex combination of \(1/o_{\text{over}}\) and \(1 - 1/o_{\text{under}}\) and does not clear either price, so it always passes.
- Revisit. Replace the pooled strengths if a fit that lets attack and defence change by season moves the held-out mean profit by more than the paired standard error of the difference. Constant strengths across 2019–2024, including the closed-door part of 2019/20, are the assumption trusted least.

## 2026-11-18: Loss is minus mean profit per match

- Decision. On each holdout match the profit is \(0\) on a pass, \(o - 1\) on a winning unit, and \(-1\) on a losing unit. The over wins when \(Q > 2.5\); the under wins when \(Q \le 2\). The loss is minus the mean of that profit over the 380 matches. The notebook reports the mean profit, in stake units per match. The two scores are that mean for the training over-rate and for the Poisson model.
- Why. The action is one unit or nothing. A better Brier score or log loss can still pay the overround on every stake. Mean profit per match counts a pass as 0, which is an action the rule is allowed to take. The standard error beside each score treats the 380 match profits as independent.
- Alternative. Mean profit per stake, which drops the passes, or the Brier score on the indicator \(Q > 2.5\).
- Revisit. Change the loss if the stake is no longer one unit, or if the line is no longer 2.5 and a push can occur.

## 2026-11-18: Simulation check uses seed 20241118

- Decision. The simulation check draws from NumPy seed 20241118. The generator is split into two streams. The first redraws the training totals from the fitted rates. The second draws a league of eight clubs with known attack and defence, 2,240 matches, and the fit is run again.
- Why. The draws check the model. They are not used to choose the stake or to score the 380 holdout matches. The maximum likelihood estimate starts from a fixed point and does not use the seed.
- Alternative. Leave the check unseeded.
- Revisit. Change the seed only if the check is replaced. The held-out profit does not depend on it.

## 2026-11-25: Keep the rule after the holdout

- Decision. The 29 October rule stays. Stake one unit on the over or the under only when the probability clears the Bet365 closing price, and pass otherwise. The talk states that rule. On the 380 held-out matches, a stake that does not clear the price does not beat a pass.
- Why. Forcing a stake on the 78 passes earns +0.0035 stake units per holdout match. Betting the preferred side on every match earns −0.0004. The Poisson rule earns −0.0039 and the training over-rate earns +0.0126. None of these is separated from 0 by its standard error.
- Alternative. Bet a side on every match, or pass every match and drop the price test.
- Revisit. Change the rule if a later season shows a stake that does not clear the price earning a higher mean profit per match than passing, by more than the standard error.
