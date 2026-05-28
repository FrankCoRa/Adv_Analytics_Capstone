# Advanced Analytics Capstone  
# Course Demand Forecasting for Academic Scheduling

A machine learning project focused on forecasting **section-level course enrollment demand** using historical academic scheduling data from the Registrar’s Office at Mercy University.

The project applies predictive analytics, feature engineering, and seasonal machine learning models to support proactive academic planning, enrollment forecasting, and data-driven Scheduling operations.

---

# Project Overview

Academic scheduling requires institutions to make operational decisions before final enrollment demand is known. Variability in student enrollment patterns can create challenges for:

- Section planning
- Course demand estimation
- Modality balancing
- Academic resource allocation
- Long-term scheduling strategy

This project develops a predictive enrollment forecasting workflow capable of estimating future section demand across:

- Spring
- Summer
- Fall

using historical Registrar scheduling data.

---

# Business Problem

The Scheduling team often experience uncertainty around future course demand before student registration is finalized.

This can lead to:

- Overfilled sections
- Underutilized course offerings
- Last-minute scheduling adjustments
- Increased administrative workload
- Reactive planning decisions
- Difficulty identifying high-demand courses early

The objective of this project is to help academic scheduling teams anticipate section demand earlier through predictive analytics.

---

# Project Goals

The project focuses on:

- Forecasting section-level enrollment demand
- Identifying seasonal enrollment trends
- Understanding modality-based enrollment behavior
- Supporting proactive academic scheduling decisions
- Improving data-driven planning within Scheduling operations
- Providing interpretable forecasting insights for stakeholders

---

# Project Objectives

The primary objectives include:

- Predict future section enrollment
- Analyze historical enrollment patterns
- Understand demand behavior across academic terms
- Evaluate modality-based demand trends
- Improve operational visibility before registration periods
- Support institutional planning through predictive analytics

---

# Deliverables

The project includes:

- Executive summary for stakeholders
- Technical report with methodology and findings
- Predictive model evaluation
- Enrollment trend visualizations
- Feature engineering documentation
- Ethical and governance considerations
- Forecasting workflow documentation

---

# PACE Framework

This project follows Google's PACE Framework:


![PACE Framework](pace.jpg)

| Stage | Description |
|---|---|
| **Plan** | Define business problem, stakeholders, objectives, and success metrics |
| **Analyze** | Perform exploratory data analysis and feature engineering |
| **Construct** | Build and validate machine learning forecasting models |
| **Execute** | Evaluate results, communicate insights, and identify operational impact |

---

# 🟨 PACE: Plan

## Initial Data Exploration

Initial analysis identified several important scheduling and enrollment behaviors:

- Enrollment demand varies significantly by:
  - term
  - modality
  - course
  - school
  - campus

- Historical enrollment patterns show recurring seasonal behavior across:
  - Spring
  - Summer
  - Fall

- In-person (`TD`) sections consistently demonstrate stronger enrollment demand during primary academic terms.

- Some sections repeatedly operate near or above enrollment capacity, indicating recurring demand pressure.

---

# Dataset Characteristics

The dataset contains academic scheduling and enrollment data including:

- Course identifiers
- Section information
- Campus locations
- Instructional modalities
- Meeting days and times
- Enrollment counts
- Maximum enrollment capacity
- Historical scheduling behavior
- Cross-listed course information

---

# Feature Engineering

Several engineered variables were created to improve forecasting accuracy and capture enrollment behavior patterns.

| Variable | Description |
|---|---|
| `COURSE_CODE` | Combined `SUBJECT + CRSNUMBER` identifier |
| `SECTION_SEQ` | Sequential section count |
| `INST_METHOD_GROUPED` | Grouped instructional modality |
| `TIME_BLOCK` | Morning / Afternoon / Evening |
| `NUM_MEETING_DAYS` | Number of weekly meeting days |
| `XLST_FLAG` | Cross-listed section indicator |
| `hist_avg_enroll` | Historical average enrollment |
| `hist_std_enroll` | Historical enrollment variability |
| `hist_max_enroll` | Historical peak enrollment |
| `hist_section_count` | Historical offering frequency |
| `last_hist_enroll` | Most recent historical enrollment |
| `last_same_term_enroll` | Most recent same-season enrollment |

These features allow the model to better understand:

- Historical demand patterns
- Seasonal enrollment behavior
- Modality trends
- Section frequency
- Cross-listed section dynamics

---

# Key Research Questions

This project addresses several operational and analytical questions:

- Can historical scheduling data accurately predict future enrollment?
- Which features are most useful for forecasting demand?
- How does enrollment behavior vary across academic terms?
- Do instructional modalities influence section demand?
- Can predictive analytics support earlier Scheduling decision-making?

---

# Success Metrics

## Forecasting Metrics

| Metric | Purpose |
|---|---|
| `MAE` | Average prediction error |
| `RMSE` | Penalizes larger prediction errors |
| `R² Score` | Measures explained enrollment variance |

---

# 🟨 PACE: Analyze

The Analyze phase focused on exploratory data analysis, cleaning, standardization, and enrollment behavior analysis.

---

# Data Sources

The analysis used:

- Historical course scheduling data (Spring 2024 – Spring 2026)
- Fall 2026 validation data
- Scheduling enrollment records

---

# Exploratory Data Analysis

## Key Observations

### Instructional Modality Demand

- In-person (`TD`) sections consistently show stronger enrollment demand.
- Spring and Fall terms have the highest enrollment activity.
- Summer enrollment patterns are lower and more concentrated.
  
![Distribution Analysis](enrollment.jpg)
---

## Course Cancellation Patterns

Canceled sections occur more frequently during:

- Spring
- Fall

This may reflect:
- enrollment balancing
- low-demand section consolidation
- operational scheduling adjustments

---

## Trimester & Quarter Term Exclusion

Trimester and Quarter terms were excluded because they:
- contain lower-volume specialized sections
- introduce statistical noise
- reduce model generalization

---

## Capacity Demand Analysis

Sections with maximum enrollment values of:

- `20`
- `25`
  
![Disperse Analysis](max_enrollment.jpg)

frequently operate near or above capacity during Spring and Fall terms.

This suggests recurring enrollment pressure and consistent demand concentration.

---

## Enrollment Ratio Trends

Enrollment ratio was calculated using:

```python
enrollment_ratio = SECTENROLL / MAXENROLL
```

### Seasonal Enrollment Behavior

| Term | Enrollment Trend |
|---|---|
| Spring | Higher |
| Summer | Lower |
| Fall | Higher |


![Boxplot Analysis](ratio.jpg)
---

## Demand by School & Modality

Analysis revealed:

- SLA courses dominate Spring and Fall demand.
- Nursing and HNS courses drive Summer demand.
- In-person sections generally outperform distance learning demand during primary academic terms.

![Bar-graph Analysis](top_30.jpg)
---

# Data Challenges

| Challenge | Impact |
|---|---|
| Variability in `MTGTIME` and `MTGDAYS` | Required standardization |
| Cross-listed structures | Increased feature engineering complexity |
| Enrollment outliers | Required outlier analysis |
| Low-volume special terms | Reduced model stability |
| High granularity | Reduced generalization across small groups |

---

# 🟦 PACE: Construct

## Final Enrollment Forecasting Model

The final model forecasts section-level enrollment using historical scheduling data.

The forecasting workflow focuses entirely on:

- course demand forecasting
- enrollment prediction
- academic scheduling analytics

---

# Step 1 — Data Cleaning & Filtering

The dataset was standardized and filtered to include only:

- Active sections
- In-person (`TD`)
- Hybrid (`BLD`)

Distance learning modalities were consolidated into a single `DL` category.

```python
df['INST_METHOD_GROUPED'] = df['INST_METHOD'].replace({
    'WB': 'DL',
    'WH': 'DL',
    'WS': 'DL'
})
```

---

# Step 2 — Historical Enrollment Features

Historical enrollment behavior was aggregated using:

```python
course_history = (
    historical_df
    .groupby(['COURSE_CODE', 'INST_METHOD_GROUPED'])
)
```

The model captures:

- Average historical enrollment
- Enrollment variability
- Historical peak demand
- Historical section frequency
- Same-season enrollment behavior

---

# Step 3 — Seasonal Forecasting Strategy

Instead of using one global model, the final approach trains separate models for:

- Spring
- Summer
- Fall

This improves forecasting performance because enrollment behavior differs significantly by season.

---

# Step 4 — Final Machine Learning Model

The final forecasting model uses:

```python
XGBRegressor(
    n_estimators=300,
    max_depth=4,
    learning_rate=0.05,
    subsample=0.8,
    colsample_bytree=0.8,
    random_state=42
)
```

XGBoost was selected because it:
- handles structured tabular data effectively
- captures nonlinear enrollment behavior
- performs well with mixed feature types
- improves predictive stability across academic terms

---

# Final Model Features

| Feature Category | Examples |
|---|---|
| Historical Enrollment | Average enrollment, prior enrollment |
| Course Features | Course code, modality |
| Academic Term | Spring, Summer, Fall |
| Schedule Features | Meeting days, time block |
| Section Structure | Cross-listed indicators |

---

# Model Output

```python
predicted_enrollment_xgb
```

The final output predicts expected enrollment for each CRN in the target term.

---

# 🟥 PACE: Execute

## Seasonal Validation Results

The final seasonal XGBoost models achieved the following validation results:

| Season | Validation Term | MAE | RMSE | R² |
|---|---|---|---|---|
| Spring | 202610 | 3.079 | 4.320 | 0.739 |
| Summer | 202620 | 4.299 | 6.629 | 0.826 |
| Fall | 202530 | 2.948 | 4.376 | 0.757 |

---

# Understanding the Evaluation Metrics

The model was evaluated using three primary regression metrics:

| Metric | Meaning |
|---|---|
| `MAE` | Average enrollment prediction error |
| `RMSE` | Measures larger prediction errors more heavily |
| `R²` | Measures how much enrollment variation is explained by the model |

---

# MAE — Mean Absolute Error

## Definition

MAE measures the average difference between:

- predicted enrollment
- actual enrollment

The lower the MAE, the more accurate the model predictions are on average.

---

## Example

If a section actually enrolled:

```python
30 students
```

and the model predicted:

```python
27 students
```

the prediction error is:

```python
3 students
```

---

## Interpretation of Results

| Season | MAE Interpretation |
|---|---|
| Spring | Predictions are off by approximately 3 students on average |
| Summer | Predictions are off by approximately 4.3 students on average |
| Fall | Predictions are off by approximately 2.9 students on average |

---

## Key Insight

The Fall model achieved the lowest average prediction error, meaning it generated the most precise enrollment forecasts overall.

This suggests Fall enrollment behavior is:
- more stable
- more historically consistent
- easier for the model to learn

---

# RMSE — Root Mean Squared Error

## Definition

RMSE measures prediction error similarly to MAE but gives larger penalties to bigger mistakes.

This helps identify whether the model occasionally produces large forecasting errors.

---

## Example

Suppose two sections have prediction errors:

| Section | Error |
|---|---|
| Section A | 2 students |
| Section B | 12 students |

RMSE penalizes the 12-student error more heavily than MAE.

---

## Interpretation of Results

| Season | RMSE Interpretation |
|---|---|
| Spring | Some larger forecasting errors exist but remain controlled |
| Summer | Greater enrollment variability creates larger prediction swings |
| Fall | Forecasting remains relatively stable |

---

## Key Insight

Summer produced the highest RMSE because summer enrollment behavior is:
- less consistent
- more volatile
- lower volume
- heavily dependent on specific programs and modalities

Even though Summer achieved the strongest R² score, its enrollment variability increases large-error sensitivity.

---

# R² — Coefficient of Determination

## Definition

R² measures how much of the enrollment variation can be explained by the model.

Values closer to:

```python
1.0
```

indicate stronger predictive performance.

---

## Interpretation of Results

| Season | R² Interpretation |
|---|---|
| Spring | Model explains ~74% of enrollment variation |
| Summer | Model explains ~83% of enrollment variation |
| Fall | Model explains ~76% of enrollment variation |

---

## Key Insight

The Summer model achieved the highest R² value:

```python
0.826
```

meaning the model successfully captured most of the enrollment behavior during Summer terms.

This may occur because:
- Summer offerings are smaller
- fewer modalities are offered
- scheduling patterns are more concentrated
- fewer sections create more identifiable enrollment behavior

---

# Overall Model Interpretation

The seasonal forecasting strategy demonstrated strong predictive performance across all academic terms.

## Main Findings

- Historical enrollment behavior is highly predictive of future enrollment.
- Same-season historical enrollment is one of the strongest forecasting signals.
- Enrollment behavior differs significantly between:
  - Spring
  - Summer
  - Fall

which validates the decision to train separate seasonal models.

---

# Operational Interpretation for the Scheduling Team

From a Scheduling perspective, forecasting within approximately:

```python
±3 students
```

for most Spring and Fall sections provides meaningful planning value before registration begins.

Examples of operational value include:

- identifying sections likely to exceed capacity
- monitoring low-demand offerings earlier
- anticipating enrollment pressure by modality
- supporting discussions with academic departments
- improving section planning visibility before schedules finalize

---

# Business Impact

The forecasting workflow supports Scheduling operations by:

- Improving visibility into future enrollment demand
- Identifying high-demand sections earlier
- Supporting proactive academic planning
- Reducing uncertainty before registration periods
- Helping departments anticipate enrollment pressure
- Supporting data-driven scheduling conversations

---

# Ethical Considerations

This project considers:

- Responsible institutional data usage
- Privacy and governance concerns
- Transparency in model interpretation
- Avoiding over-reliance on automated predictions
- Bias monitoring across schools and modalities

The model is intended to support decision-making rather than replace human academic planning processes.

---

# Tools & Technologies

## Programming Language

- Python

## Libraries

- Pandas
- NumPy
- Scikit-learn
- XGBoost
- Matplotlib
- Seaborn

## Development Environment

- Jupyter Notebook

---

# Future Improvements

Potential future enhancements include:

- Adding additional historical years
- Incorporating waitlist forecasting
- Adding department-level demand indicators
- Developing SHAP-based explainability dashboards
- Building interactive Tableau or Power BI dashboards
- Automating term-to-term forecasting workflows
- Monitoring long-term model drift

---

# Strategic Outcome

This project demonstrates how predictive analytics can help transition from reactive scheduling support toward proactive academic planning.

By forecasting enrollment demand before final registration is known, the workflow provides earlier visibility into:

- section demand
- modality trends
- seasonal enrollment behavior
- operational planning needs

---

# Scheduling Operational Outcome

One of the proposed operational outcomes of this project is the integration of forecasted enrollment values directly into future academic section rolls.

The forecasted enrollment amount can be added as a reference value during the rolling process for future terms, allowing scheduling teams and academic departments to review predicted demand before schedules are finalized.

This creates several operational advantages:

- Improved visibility into expected section demand
- Earlier identification of high-demand courses
- Better planning for section offerings
- Stronger support for proactive scheduling decisions
- Enhanced communication between the Scheduling team and academic departments

---

# Example Use Case

When rolling a future section into an upcoming term:

| CRN | Course | Historical Avg | Forecasted Enrollment |
|---|---|---|---|
| 24510 | BIOL130 | 24 | 31 |
| 24522 | ENGL110 | 18 | 17 |
| 24531 | NURS320 | 20 | 34 |

Scheduling teams could immediately identify:

- sections likely to experience enrollment pressure
- sections potentially requiring additional offerings
- courses showing declining demand trends

before registration officially begins.

---

# Long-Term Institutional Value

Over time, this forecasting workflow can help institutions:

- strengthen academic planning
- improve forecasting visibility
- reduce reactive scheduling adjustments
- support evidence-based decision-making
- improve operational efficiency across Scheduling workflows

while maintaining human oversight and institutional scheduling expertise.
