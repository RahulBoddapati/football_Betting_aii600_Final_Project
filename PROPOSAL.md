# Proposal

## Hypothesis

A probability built only from information available before the close can clear the Bet365 closing price often enough to beat a pass, in mean profit per match, on the 2024/25 Premier League.

The quantity is \(P(Q > 2.5)\), the probability that one match’s full-time total exceeds 2.5 goals. \(Q\) is home goals plus away goals, in goals. The probability is scored on each match from 16 August 2024 through 25 May 2025. Seasons used to build the forecast run from 9 August 2019 through 19 May 2024.

## Data

The source is [Football-Data.co.uk](https://www.football-data.co.uk/data.php), Premier League files coded `E0`, one CSV per season. The unit of observation is one completed match. The files contain 2,280 matches from 9 August 2019 through 25 May 2025 (seasons `1920` through `2425`). One row is dropped: Newcastle v Manchester City on 19 December 2021, whose closing 2.5 prices are stored as 0. That leaves 2,279 matches. The 380 matches in `2425` are held out and are not used to choose the rule.

Football-Data compiles full-time scores from public results sites and the prices from bookmakers, Betbrain, and Oddsportal. Since 2019/20 the site stores an opening set and a closing set; weekend prices are taken on Friday afternoons and midweek prices on Tuesday afternoons. We use the closing Bet365 columns `B365C>2.5` and `B365C<2.5`.

The file records the score after the whistle. The stake is chosen before kickoff, so the decision needs \(P(Q > 2.5)\), and the closing quote is a decimal price rather than that probability. Shots, cards, and the other in-match statistics in the same file are recorded during the match, so they stay out of the forecast. The column key is [notes.txt](https://www.football-data.co.uk/notes.txt).

The [disclaimer](https://www.football-data.co.uk/disclaimer.php) does not grant a licence to republish the CSVs. A release asset would be a republished copy, so the files stay off GitHub. `data/README.md` gives the command that downloads them from the source.

## Decision affected by the model

We act before kickoff. The prediction changes a one-unit stake: bet the over when \(p > 1/o_{\text{over}}\), bet the under when \(1 - p > 1/o_{\text{under}}\), and pass otherwise. Passing earns 0. A winning unit stake pays \(o - 1\); a losing one costs 1.

Betting before the probability clears the price costs more. The two prices sum to more than a fair book (mean overround about 1.05 on these matches), so a toss-up pays that margin on every bet. Passing when an edge was real only misses profit.
