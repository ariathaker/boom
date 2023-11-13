---
layout: default
title: Scoring
nav_order: 5
---

## Scoring

### Aggregate Score
Each individual check returns a score of 0 to 10, with 10 representing the best
possible score. Scorecard also produces an aggregate score, which is a
weight-based average of the individual checks weighted by risk.

*   “Critical” risk checks are weighted at 10
*   “High” risk checks are weighted at 7.5
*   “Medium” risk checks are weighted at 5
*   “Low” risk checks are weighted at 2.5

See the [list of current Scorecard checks](#scorecard-checks) for each check's
risk level.
