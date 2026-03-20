# March Madness Underdog Strategy Research

## Project Overview

This project tests a strategic hypothesis about NCAA March Madness upsets: that underdogs can systematically improve their odds by targeting the opponent's top players for foul trouble, exploiting the assumption that talent density drops off more steeply beyond the starting 5 for higher-seeded teams.

## Core Hypothesis

Even among top-seeded tournament teams, there is greater variance in talent density beyond the starting 5 roster spots. If true, the talent gap between a 3-seed's bench and a 14-seed's bench is significantly smaller than the gap between their starters. This means removing starters via foul trouble disproportionately hurts the higher seed.

## Research Focus

We are NOT focused on the extreme 1 vs 16 matchups. The sweet spot is the 3 vs 14 through 5 vs 12 seed range, where upsets are expected every year and the bench talent convergence hypothesis is most plausible.

## Data Strategy

### Primary dataset
- 2024 March Madness full team rosters (user-provided, format TBD once uploaded)

### Player quality metric
- Barttorvik individual player advanced stats via the `pybart` Python library (https://github.com/avewright/pybart)
- Key method: `Torvik().player_stats(year=2024)` returns 67 columns of player-level data including BPM (Box Plus/Minus), shot locations, and per-game box scores
- BPM is the primary single-number player value metric we'll use
- Use caching: `Torvik(cache_dir=".torvik_cache", cache_ttl=3600)`

### Enrichment workflow
1. Pull Barttorvik player stats for the 2023-24 season
2. Match players from the roster dataset to Barttorvik records (expect name inconsistencies; fuzzy matching will likely be needed)
3. Tag each player with their BPM and other relevant advanced stats
4. Classify players as starter vs bench (by minutes played or roster position)
5. Segment by tournament seed tier

### Analysis goals
- Compare the starter-to-bench quality gap (measured by BPM or composite metric) across seed tiers
- Determine whether the gap narrows as seed numbers get closer (i.e., does a 12-seed's bench look more like a 5-seed's bench than a 16-seed's bench looks like a 1-seed's?)
- Quantify how much removing a top-5 player to foul trouble degrades a team's effective talent level, by seed tier
- Explore whether historical foul trouble data correlates with upset outcomes

### Future extensions (not in scope yet)
- Multi-year analysis (pybart supports historical seasons back to ~2011)
- Recruiting composite data (247Sports, RSCI) as an alternative talent proxy
- EvanMiya BPR as a more sophisticated metric (subscription required)
- Foul economy modeling: possessions-to-fouls-drawn rates by play type

## Technical Notes
- pybart serves data from barttorvik.com flat files and API endpoints
- Per Bart Torvik's request, do not mass-scrape; use caching and bulk flat files
- Player name matching across datasets will be the primary data engineering challenge; consider fuzzy matching libraries like `thefuzz` or `rapidfuzz`
- All data attribution goes to Bart Torvik (barttorvik.com)

## User Preferences
- No dashes (em, en, or otherwise) unless hyphenating words
- Don't prompt for follow-up actions unless explicitly asked
- Ask for better input materials when it would reduce unnecessary compute
