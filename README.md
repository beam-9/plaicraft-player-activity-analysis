# PLAIcraft: Player Activity Analysis

An R-based exploratory analysis and baseline regression study of Minecraft player activity, completed as a **UBC DSCI 100 group project (Group 10, Section 003)**.

Using player profiles and session logs from UBC’s Pacific Laboratory for Artificial Intelligence (PLAI), the project explores engagement patterns and whether temporal features and aggregated player characteristics can help explain hourly activity. This is a student course project, not an official PLAI publication or deployed planning tool.

## Full report

[Open the analysis notebook](dsci_100_final_report_grp10_section3.ipynb) for code, visualisations, model outputs and discussion.

## Data

| Dataset | Size | Contents |
| --- | --- | --- |
| [players.csv](data/players.csv) | 196 rows, 7 columns | Player experience, subscription, playtime, age and profile attributes |
| [sessions.csv](data/sessions.csv) | 1,535 rows, 5 original columns | Session start/end times and a player identifier |

The datasets are joined using `hashedEmail`. Avoid reproducing individual records unnecessarily; hashing alone does not guarantee anonymity.

## Analysis workflow

- Inspect data types, missing values, session durations and uneven engagement.
- Visualise player age, playtime, session frequency and hourly patterns with ggplot2.
- Join profiles and sessions; derive temporal features and an experience indicator.
- Construct hourly groups and aggregate player characteristics.
- Fit multivariable linear regression with tidymodels using an 80/20 random split and seed 123.
- Evaluate predictions and interpret coefficients and limitations.

## Findings

Playtime and session frequency are highly uneven: most participants have little recorded activity, while a small minority contribute substantially more. Missing end times and zero-duration sessions complicate interpretation. The baseline model shows weak predictive performance on the constructed hourly outcome.

| Metric | Recorded test result |
| --- | ---: |
| RMSE | 1.072 |
| MAE | 0.813 |
| R-squared (`yardstick::rsq`) | 0.0765 |

These are outputs from the submitted report, not independently revalidated benchmarks. The reported R-squared is squared correlation, not necessarily an out-of-sample variance-explained calculation.

## Scope and limitations

The original question concerns simultaneous players, but the code groups sessions by their **start hour** and counts distinct players in each group. This measures session-start activity, not true concurrency from overlapping session intervals. The results should therefore not be used to establish a safe capacity limit or a required number of licences.

Further improvements include:

- Explicitly parse timestamps and document their timezone instead of comparing date strings.
- Reconstruct session overlap and include hours without activity.
- Produce one validated row per hour; the existing `reframe()` can retain repeated hourly rows.
- Use chronological validation and prevent repeated hourly observations crossing the train/test boundary.
- Check whether aggregate playtime features would be available at prediction time.
- Correct the hourly plot interpretation: it counts session starts rather than average concurrent sessions.
- Compare against a simple baseline before claiming forecasting value.

The original coursework notebook is preserved; this documentation update does not alter the analysis.

## Tools and setup

**R · Jupyter · tidyverse · dplyr · ggplot2 · tidymodels · repr · GGally**

Install R and Jupyter with an R kernel, then install the packages:

```r
install.packages(c("tidyverse", "tidymodels", "repr", "GGally", "IRkernel"))
IRkernel::installspec()
```

Open the notebook using the R kernel. Load `library(GGally)` before running the `ggpairs()` cell if it is not already loaded. The notebook reads CSVs from the original GitHub repository URL; for local execution, replace those reads with `data/players.csv` and `data/sessions.csv`.

Package versions are not pinned, and clean-session execution has not been verified for this README update. The pipeline issues listed above may require corrections before rerunning or extending the model.

## Attribution and licence

This is a collaborative course project; individual contributions should be stated separately when describing it in a portfolio or resume. Data provenance and references are described in the notebook.

See [LICENSE](LICENSE) for the existing repository licence. Do not assume the code licence independently grants unrestricted rights to the research data.
