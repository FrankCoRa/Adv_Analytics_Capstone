# Advanced Analytics Capstone  
## Course Demand Forecasting for Academic Scheduling

This project focuses on developing predictive analytics and machine learning models to forecast course enrollment demand for the Registrar’s Office at Mercy University.

Using historical academic scheduling data, the project aims to improve:

- Academic scheduling efficiency
- Classroom utilization
- Enrollment forecasting accuracy
- Strategic resource allocation
- Operational decision-making across Spring, Summer, and Fall terms

The project combines data analytics, feature engineering, machine learning, and scheduling optimization techniques to support data-driven academic planning.

---

# Project Objectives

The primary objectives of this project are to:

- Forecast course-level enrollment demand
- Improve room assignment decision-making
- Reduce underutilized and over-capacity classrooms
- Minimize last-minute scheduling changes
- Support strategic scheduling and institutional planning

---

# Business Problem

Uncertain student enrollment patterns often result in:

- Inefficient room assignments
- Underutilized classrooms
- Over-capacity classrooms
- Scheduling conflicts
- Increased administrative workload
- Reduced operational efficiency

These challenges directly impact both institutional operations and student experience.

---

# Project Goal

Develop a predictive scheduling system capable of forecasting course enrollment demand while considering operational scheduling constraints such as:

- Campus location
- Room availability
- Capacity limitations
- Instructional methodologies
- Cross-listed sections
- Historical enrollment behavior

---

# Deliverables

The project includes the following deliverables:

- Executive summary for stakeholders
- Technical report with methodology and findings
- Predictive model evaluation and interpretation
- Data visualizations and trend analysis
- Ethical and data governance considerations
- Documentation of tools, workflows, and processes used

---

# PACE Framework

This project follows Google's PACE framework:

| Stage | Description |
|---|---|
| **Plan** | Define the business problem, project goals, stakeholders, and success metrics |
| **Analyze** | Perform exploratory data analysis, feature engineering, and statistical analysis |
| **Construct** | Build and evaluate predictive machine learning models |
| **Execute** | Present findings, visualizations, recommendations, and deployment considerations |

---

# PACE: Plan

## Initial Data Exploration

Initial observations from the exploratory data analysis include:

- The dataset contains detailed scheduling and enrollment information across multiple academic terms.
- Key variables include campus, CRN, instructional methodology, cross-listed groups, meeting days/times, room assignments, enrollment counts, and maximum room capacities.
- Some rooms are heavily utilized while others remain underutilized during specific terms.
- Enrollment demand varies significantly across courses and instructional methods.
- Multiple instructional delivery methods are represented, including in-person, hybrid, and distance learning formats.
- Historical enrollment trends suggest recurring seasonal demand patterns across terms.

---

# Dataset Characteristics

The dataset includes scheduling and enrollment-related attributes such as:

- Course identifiers
- Section information
- Campus locations
- Meeting days and times
- Room assignments
- Instructional methodologies
- Cross-listed course groups
- Enrollment counts
- Maximum enrollment capacities
- Historical scheduling behavior

---

# Feature Engineering

To improve predictive performance and capture scheduling behavior patterns, several engineered variables were created from the original Registrar dataset.

These features help the model better understand enrollment trends, instructional methodologies, scheduling intensity, and historical course behavior.

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

The historical variables shown above were generated through grouping and aggregation processes to provide stronger predictive signals for the target variable: **student enrollment (`SECTENROLL`)**.

These engineered features allow the model to capture:

- Historical enrollment behavior
- Seasonal demand patterns
- Instructional methodology trends
- Scheduling frequency patterns
- Cross-listed course relationships

---

# Key Research Questions

Throughout the planning and analysis stages, the project focuses on addressing the following questions:

- Can historical academic scheduling data accurately predict future course enrollment?
- Can machine learning improve room assignment decision-making?
- Can inefficient classroom usage be reduced through predictive analytics?
- How accurately can enrollment demand be forecasted across different academic terms?
- Can predictive scheduling reduce last-minute scheduling adjustments?

---

# Success Metrics

Project success is evaluated using both predictive performance metrics and operational scheduling outcomes.

## Predictive Metrics

- MAE (Mean Absolute Error)
- RMSE (Root Mean Squared Error)
- R² Score

## Scheduling Optimization Metrics

- Recommended vs. actual room match rate
- Capacity utilization improvement
- Reduction of under-capacity assignments
- Reduction of over-capacity assignments
- Conflict avoidance rate

---

# PACE: Analyze

During the Analyze phase, exploratory data analysis and feature engineering were performed to better understand enrollment patterns, scheduling behavior, and operational constraints.

## Analysis Activities

- Exploratory Data Analysis (EDA)
- Enrollment distribution analysis
- Room utilization analysis
- Cross-listed section analysis
- Time block analysis
- Historical trend analysis
- Correlation analysis
- Feature engineering and selection

Several visualizations were developed to identify:

- Enrollment trends across terms
- Instructional methodology behavior
- Room utilization efficiency
- Over-capacity and underutilized classrooms
- Historical seasonal demand patterns

---

# Machine Learning and Modeling

The project evaluates multiple machine learning models for enrollment forecasting and scheduling optimization.

## Models Evaluated

- Linear Regression
- Random Forest Regressor
- Gradient Boosting Regressor
- XGBoost Regressor

## Model Optimization

Model tuning and validation techniques include:

- GridSearchCV
- Cross-validation
- Hyperparameter tuning
- Feature importance analysis

## Evaluation Metrics

The following evaluation metrics were used to assess model performance:

- Accuracy
- Precision
- Recall
- F1-Score
- AUC-ROC
- MAE
- RMSE
- R² Score

---

# Tools and Technologies

## Programming Languages

- Python

## Libraries and Frameworks

- Pandas
- NumPy
- Scikit-learn
- XGBoost
- Matplotlib
- Seaborn

## Development Environment

- Jupyter Notebook

---

# Ethical Considerations

This project considers ethical and operational concerns including:

- Data privacy and governance
- Fair resource allocation
- Transparency in predictive modeling
- Bias reduction in scheduling recommendations
- Responsible use of institutional data

---

# Expected Impact

The proposed predictive scheduling system aims to:

- Improve classroom utilization
- Increase forecasting accuracy
- Reduce scheduling conflicts
- Enhance institutional planning
- Support data-driven decision-making within the Registrar’s Office

---

# Future Improvements

Potential future enhancements for the project include:

- Real-time room recommendation systems
- Automated scheduling optimization
- Integration with institutional scheduling platforms
- Predictive waitlist analysis
- Deep learning forecasting models
- Interactive scheduling dashboards
- Real-time room conflict detection

---
