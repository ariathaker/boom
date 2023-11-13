---
title: Overview
layout: home
---

# OpenSSF Scorecard

[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/ossf/scorecard/badge)](https://securityscorecards.dev/viewer/?uri=github.com/ossf/scorecard)
[![OpenSSF Best Practices](https://www.bestpractices.dev/projects/5621/badge)](https://www.bestpractices.dev/projects/5621)
![build](https://github.com/ossf/scorecard/workflows/build/badge.svg?branch=main)
![CodeQL](https://github.com/ossf/scorecard/workflows/CodeQL/badge.svg?branch=main)
[![Go Reference](https://pkg.go.dev/badge/github.com/ossf/scorecard/v4.svg)](https://pkg.go.dev/github.com/ossf/scorecard/v4)
[![Go Report Card](https://goreportcard.com/badge/github.com/ossf/scorecard/v4)](https://goreportcard.com/report/github.com/ossf/scorecard/v4)
[![codecov](https://codecov.io/gh/ossf/scorecard/branch/main/graph/badge.svg?token=PMJ6NAN9J3)](https://codecov.io/gh/ossf/scorecard)
[![SLSA 3](https://slsa.dev/images/gh-badge-level3.svg)](https://slsa.dev)
[![Slack](https://img.shields.io/badge/slack-openssf/security_scorecards-white.svg?logo=slack)](https://slack.openssf.org/#security_scorecards)


### What is Scorecard?
We created Scorecard to help open source maintainers improve their security
best practices and to help open source consumers judge whether their dependencies
are safe.

Scorecard is an automated tool that assesses a number of important heuristics
[("checks")](#scorecard-checks) associated with software security and assigns
each check a score of 0-10. You can use these scores to understand specific
areas to improve in order to strengthen the security posture of your project.
You can also assess the risks that dependencies introduce, and make informed
decisions about accepting these risks, evaluating alternative solutions, or
working with the maintainers to make improvements.

The inspiration for Scorecard’s logo:
["You passed! All D's ... and an A!"](https://youtu.be/rDMMYT3vkTk)

#### Project Goals

1.  Automate analysis and trust decisions on the security posture of open source
    projects.

1.  Use this data to proactively improve the security posture of the critical
    projects the world depends on.

### Prominent Scorecard Users

Scorecard has been run on thousands of projects to monitor and track security
metrics. Prominent projects that use Scorecard include:

-   [Tensorflow](https://github.com/tensorflow/tensorflow)
-   [Angular](https://github.com/angular/angular)
-   [Flutter](https://github.com/flutter/flutter)
-   [sos.dev](https://sos.dev)
-   [deps.dev](https://deps.dev)

### View a Project's Score

To see scores for projects regularly scanned by Scorecard, navigate to the webviewer, replacing the placeholder text with the platform, user/org, and repository name: 
https://securityscorecards.dev/viewer/?uri=<github_or_gitlab>.com/<user_name_or_org>/<repository_name>.

For example: 
 - [https://securityscorecards.dev/viewer/?uri=github.com/ossf/scorecard](https://securityscorecards.dev/viewer/?uri=github.com/ossf/scorecard)
 - [https://securityscorecards.dev/viewer/?uri=gitlab.com/fdroid/fdroidclient](https://securityscorecards.dev/viewer/?uri=gitlab.com/fdroid/fdroidclient)

To view scores for projects not included in the webviewer, use the [Scorecard CLI](#scorecard-command-line-interface). 

### Public Data

We run a weekly Scorecard scan of the 1 million most critical open source
projects judged by their direct dependencies and publish the results in a
[BigQuery public dataset](https://cloud.google.com/bigquery/public-data).

This data is available in the public BigQuery dataset
`openssf:scorecardcron.scorecard-v2`. The latest results are available in the
BigQuery view `openssf:scorecardcron.scorecard-v2_latest`.

You can query the data using [BigQuery Explorer](http://console.cloud.google.com/bigquery) by navigating to Add Data > Star a project by name > 'openssf'.
For example, you may be interested in how a project's score has changed over time:

```sql
SELECT date, score FROM `openssf.scorecardcron.scorecard-v2` WHERE repo.name="github.com/ossf/scorecard" ORDER BY date ASC
```

You can extract the latest results to Google Cloud storage in JSON format using
the [`bq`](https://cloud.google.com/bigquery/docs/bq-command-line-tool) tool:

```
# Get the latest PARTITION_ID
bq query --nouse_legacy_sql 'SELECT partition_id FROM
openssf.scorecardcron.INFORMATION_SCHEMA.PARTITIONS WHERE table_name="scorecard-v2"
AND partition_id!="__NULL__" ORDER BY partition_id DESC
LIMIT 1'

# Extract to GCS
bq extract --destination_format=NEWLINE_DELIMITED_JSON
'openssf:scorecardcron.scorecard-v2$<partition_id>' gs://bucket-name/filename-*.json

```

The list of projects that are checked is available in the
[`cron/internal/data/projects.csv`](https://github.com/ossf/scorecard/blob/main/cron/internal/data/projects.csv)
file in this repository. If you would like us to track more, please feel free to
send a Pull Request with others. Currently, this list is derived from **projects
hosted on GitHub ONLY**. We do plan to expand them in near future to account for
projects hosted on other source control systems.
