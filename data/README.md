# Data

The notebook reads six Premier League files from this folder. They are not committed. The [Football-Data disclaimer](https://www.football-data.co.uk/disclaimer.php) does not grant a licence to republish the CSVs, so there is no GitHub release tagged `data`.

The source is [Football-Data.co.uk](https://www.football-data.co.uk/mmz4281/), the Premier League file `E0.csv` for each season code below. No account and no key. The site returns 403 unless the request sends a user agent. From the project root:

```bash
mkdir -p data
for season in 1920 2021 2122 2223 2324 2425; do
  curl -fsSL -A "Mozilla/5.0" -o "data/E0_${season}.csv" \
    "https://www.football-data.co.uk/mmz4281/${season}/E0.csv"
done
```

Each source file is named `E0.csv`. The download renames it by season so the six seasons can sit in one folder. The notebook checks these SHA-256 values before it fits anything.

| File | SHA-256 |
| --- | --- |
| E0_1920.csv | 100037618b94f94057400bb02bf6bac4ef74ddaa58cde4b38370839c39caee61 |
| E0_2021.csv | 5afe63f69401457b8354eaacee24f9a3e520b3c3af6329564a9783e20d789c62 |
| E0_2122.csv | 335afcbabeb2939fa10ab39ba3e8215072d0b577cb8d0705c1e44c56e934e703 |
| E0_2223.csv | 8442792d3b614c94ea3cf381bd2736805889cc1713169035368fff19c3d02380 |
| E0_2324.csv | b2e057b0ed959f198b0f63d2391c01239f3608e6de5db68edab3f88e04d07ff3 |
| E0_2425.csv | d0c8ce4a96d886cf60cf101f570f4a3893844226f91c7bd769eb568c49edbfa4 |

The unit of observation is one completed Premier League match. The six files contain 2,280 matches from 9 August 2019 through 25 May 2025 (season codes `1920` through `2425`, 380 matches each). The notebook drops Newcastle v Manchester City on 19 December 2021, whose closing 2.5 prices are stored as 0. That leaves 2,279 matches. Columns used are `Date`, `HomeTeam`, `AwayTeam`, `FTHG`, `FTAG`, `B365C>2.5`, and `B365C<2.5`. The column key is [notes.txt](https://www.football-data.co.uk/notes.txt).
