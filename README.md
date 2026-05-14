# Adv_Analytics_Capstone
Course Demand Forecasting for Academic Scheduling

Develop predictive models to forecast course enrollment demand for the Registrar’s Office at Mercy University. Using regression and machine learning techniques, the project analyzes academic scheduling data to improve scheduling efficiency, room utilization, and strategic planning across Spring, Summer, and Fall terms.

Deliverables

Executive summary for stakeholders.
Technical report with methodology and findings.
Predictive model evaluation and interpretation.
Data visualizations and trend analysis.
Ethical and data governance considerations.
Documentation of tools and processes used.

PACE Stages
(imagen01)

Pace: Plan

Business Problem

Uncertain student enrollment leads to inefficient room assignments, under/over-capacity classrooms, and frequent last-minute scheduling changes, impacting operational efficiency and student experience.

Project Goal

Develop a predictive scheduling system to:

Forecast course-level enrollment demand

While considering:

Campus and room availability constraints
Capacity requirements

Key Questions

Can historical data accurately predict future enrollment?
Can ML improve room assignment decisions?
Can inefficient room usage be reduced?

Success Metrics
Recommended vs. actual room match rate
Capacity utilization improvement
Reduction of under/over-capacity assignments
Conflict avoidance rate

Familiarize yourself with the key variables from the Registrar Report dataset required for model development and testing.
## Feature Definitions

| Variable | Description |
|---|---|
| `COURSE_CODE` | Unique course identifier created by concatenating the `SUBJECT` and `CRSNUMBER` fields. |
| `SECTION_LABEL` | Label or identifier assigned to a specific course section. |
| `SECTION_SEQ` | Sequential counter representing the order of sections within the same `TERM`, `COURSE_CODE`, and `INST_METHOD_GROUPED`. |
| `INST_METHOD_GROUPED` | Instructional delivery method grouped into broader categories, where all distance learning modalities are consolidated as `DL`. |
| `CAMPUS` | Campus location where the course is offered. |
| `PARTTERM` | Academic part-of-term designation associated with the course offering. |
| `MTGDAYS` | Scheduled meeting days represented using weekday letter codes (`U`, `M`, `T`, `W`, `R`, `F`, `S`). |
| `MTGTIME` | Scheduled meeting time range for the course section. |
| `TIME_BLOCK` | Generalized meeting time category derived from `MTGTIME` (e.g., Morning, Afternoon, Evening). |
| `NUM_MEETING_DAYS` | Total number of scheduled meeting days per week based on the `MTGDAYS` field. |
| `MAXENROLL` | Maximum enrollment capacity established by the academic department for the course section. |
| `XLST_FLAG` | Binary indicator identifying whether the course section belongs to a cross-listed group. |
| `hist_avg_enroll` | Historical average enrollment for the same `COURSE_CODE` and `INST_METHOD_GROUPED`. |
| `hist_std_enroll` | Historical standard deviation of enrollment values for the same `COURSE_CODE` and `INST_METHOD_GROUPED`. |
| `hist_max_enroll` | Highest historical enrollment recorded for the same `COURSE_CODE` and `INST_METHOD_GROUPED`. |
| `hist_section_count` | Total number of historical sections (`CRN`) offered for the same `COURSE_CODE` and `INST_METHOD_GROUPED`. |
| `last_hist_enroll` | Enrollment value from the most recent historical offering of the same `COURSE_CODE` and `INST_METHOD_GROUPED`. |
| `last_same_term_enroll` | Enrollment value from the most recent historical offering within the same seasonal term for the same `COURSE_CODE` and `INST_METHOD_GROUPED`. |
