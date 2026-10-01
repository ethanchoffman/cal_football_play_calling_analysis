CAL FOOTBALL PLAY-CALLING ANALYSIS
====================================

OVERVIEW
--------
This project investigates whether 1st-down play-calling (run vs. pass) meaningfully affects
drive outcomes for the California Golden Bears, using 19 seasons (2005-2024, excluding 2020)
of play-by-play data. The analysis evolved from a straightforward efficiency comparison into
a deeper methodological question: can long-run historical splits be trusted at all without
controlling for changes in roster, scheme, and coaching staff over time?

DATA SOURCE
-----------
Play-by-play data pulled via the CollegeFootballData.com API (collegefootballdata.com)
using the official cfbd Python package.

METHOD
------
- Pulled and cleaned ~20,000 plays, classifying them as run or pass plays
- Grouped plays into individual "sets of downs" (1st down through conversion, score, or
  failure) within each drive
- Defined a set as "converted" if total yards gained met or exceeded the yards needed for a
  new 1st down
- Tested run vs. pass conversion rates using independent t-tests, across multiple situational
  cuts: field position, distance-to-go, game state, and lead size
- Cross-validated findings using a second, more sensitive outcome measure (PPA - predicted
  points added) and by testing within a single coaching era (2017-2023)

KEY FINDINGS
------------

1. A statistically significant full-sample effect did not survive rigorous testing

Across the full 19-year sample, 1st-down passing plays converted sets of downs at a higher
rate than rushing (77.5% vs. 73.8%, p < 0.001, n = ~6,800). However, this result failed to
replicate under four separate checks:

- Era-scoping: Isolated to the Wilcox era (2017-2023) alone, the gap nearly disappeared
    (76.3% vs. 75.5%) and was not statistically significant.
- Alternative outcome measure: Using PPA instead of a binary conversion threshold, the
    effect was also not significant within the Wilcox era.
- Season-by-season testing: Only 2 of 19 individual seasons reached significance in
    OPPOSITE directions (2005 favored run, 2024 favored pass)
- Situational re-test: A promising secondary finding (the pass-over-run gap was largest
    specifically in two-score-lead situations, p < 0.001 in the full sample) also failed to
    replicate within a single era, using either conversion rate or PPA.

Conclusion: The most likely explanation is that the full-sample result reflects two decades 
of changing rosters and coaching philosophies blending together, not a real, exploitable 
pattern in how Cal calls plays. The core takeaway of this project is methodological: 
aggregating too many years of historical data can manufacture significant-looking findings 
that don't hold up once you account for how much a team actually changes over time. 
Any future efficiency claim should be tested within a single era or season before being trusted.

2. Cal's play-calling identity shifted substantially over time, independent of efficiency

Separate from the (debunked) efficiency question, Cal's actual 1st-down tendency shifted
meaningfully over the study period: from run-majority in earlier seasons (61-63% run in 2005,
2008, 2012) to a more balanced or pass-leaning approach in later years (55-60% pass in 2013,
2016, 2022), broadly tracking the sport's national shift toward pass-oriented offense.
Notably, this shift occurred without a corresponding, era-consistent efficiency advantage for
passing, suggesting it reflects broader schematic trends and staff philosophy rather than a
data-driven optimization.

3. A tentative lead for future work

Using PPA specifically within two-score-lead situations, passing showed a significant
advantage over running in the full sample (0.248 vs. -0.001 mean PPA, p = 0.005, n = 641).
This did not replicate within a single era and should be treated as a direction for further
investigation, ideally with larger within-era samples or additional seasons, rather than a
confirmed finding.

TAKEAWAY
--------
The takeway isn't "Cal should pass more" it's that naive, long-run splits in 
play-calling data can produce statistically significant but misleading patterns.
Four independent checks (era-scoping, an alternate outcome measure, season-by-season testing,
and a situational re-test) all pointed the same direction: the apparent effect doesn't hold up
once roster and era differences are controlled for. A genuinely actionable recommendation
would require season- or roster-specific analysis, ideally combined with opponent-strength
adjustment, rather than two-decade aggregation.

FILES
-----
cal_football_analysis.ipynb            - full analysis notebook
cal_plays_raw_with_year.csv            - pulled play-by-play data

SETUP
-----
    pip install cfbd pandas numpy matplotlib scipy certifi

Requires a free API key from collegefootballdata.com, set as an environment variable:

    export CFBD_API_KEY='your_key_here'

LIMITATIONS
-----------
- No control for opponent strength/quality in the current analysis
- "Run" and "pass" are broad categories; play design, personnel grouping, and defensive
  look are not captured
- Season-level samples are modest (~150-250 sets per season), limiting the power to detect
  real effects within any single year