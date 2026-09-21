# Student Retention & Early Intervention Analytics System
---
## 📌 Project Overview

- **Role:** Data Analyst / BI Developer
- **Domain:** Higher Education Analytics & Student Retention
- **Tech Stack:** Python (pandas, numpy, VS Code), SQL (SQLite), Power BI, DAX
- **Data set:** 4,424 records (Polytechnic Institute of Portalegre / UCI Repository)
- **Key Deliverables:**
  - End-to-End ETL Pipeline ('student_retention_pipeline.ipynb')
  - SQLite Database ('retention.db')
  - Star Schema Data Model ('dim_student.csv', 'fact_student_semester.csv')
  - Power BI Dashboard ('student_retention_pipeline.pbix')
  - Strategic Advisory Recommendations

## 🎯 Executive Summary & Business Problem

- **Context**: The institution is facing an alarming student attrition rate of up to 32.12% (1,421 dropouts), resulting in severe tuition revenue loss and negatively impacting academic accreditation metrics.
- **Core Business Problem**: The institution operates in a passive reporting mode - identifying student dropouts only after exit paperwork is finalized. Academic advisors lack operational screening tools to detect early warning signs and intervene before students disengage.
- **Strategic Objectives:**
    1. **Diagnose Critical Tipping Points:** Identify the exact academic performance thresholds and financial friction points that accelerate student attrition.
    2. **Deploy an Actionable Intervention Workbench:** Provide advisors with a decision-support dashboard to proactively target and support at-risk enrolled students during the active academic year.

## 🛠️ Data Architecture & Pipeline

### 1. Data Engineering & Transformation (Python)
- **Surrogate Primary Key Generation:** Because the raw dataset contained no unique student identifiers, a primary key ('student_id' ranging from 1001 to 5424) was generated to establish entity relational integrity.
- **Feature Engineering:** Derived academic trajectory metrics including 'sem1_pass_rate', 'sem2_pass_rate', and 'pass_rate_delta' ('Sem 2 - Sem 1') to capture early academic momentum.
- **Star Schema Normalization:**
    - dim_student (4,424 rows): Demographics, socio-economic attributes, tuition debtor status, and final outcomes.
    - fact_student_semester (8,848 rows): Unpivoted longitudinal semester data (2 records per student) capturing credits enrolled, approved, evaluations, and grade points.

### 2. SQL Integrity & Risk Triage Verification (SQLite)
Ingested tables into SQLite and ran validation queries to test hypothesis correlation prior to BI ingestion.
- **Referential Integrity Audit:** Verified primary-to-foreign key relationships between 'dim_student' and 'fact_student_semester', confirming zero orphaned records across all 8,848 semester rows.
- **Momentum Triage Analysis:** Built CTE to analyze students suffering a severe semester-over-semester velocity decline (-30%)
  - **Empirical Insight:** Flagged **409 critical-drop students**, discovering that **55.7% of this group dropped out**.
  - **Dashboard Translation:** This validation confirmed that performance decay is a critical leading indicator, directly shaping the **Academic Momentum** visualization on Page 2 and the **Risk Tier** categorization on Page 3.

## 📊 Data Modeling & DAX Strategy
- Set up the relationship model: Link dim_student with fact_student_semester via student_id (unique 1-N relationship, single direction), dim_course with dim_student via course_id (unique 1_N relationship, single direction)
- Fix small-sample bias: Implemented thresholding (N ≥ 50) to prevent statistical distortion from low-enrolled courses in macro rankings
- Risk Tier logic: Create a calculated column that combines both academic factors
    - High Risk: Severe academic failure (Pass Rate Sem 2 < 50%) OR dual-vulnerability factor (Debtor = 1 combined with Pass Rate Sem 2 < 75%).
    - Medium Risk: Steep performance decline (Pass Rate Delta ≤ -25%) OR financial factor (Debtor = 1)
    - Low Risk: Financially clear with stable or positive academic momentum.

## 💡 Dashboard Architecture & Key Insights

### Page 1: Executive Overview
<img width="907" height="513" alt="image" src="https://github.com/user-attachments/assets/9a34b4ca-7408-4f03-b2a0-4f1230015842" />

- **Institutional Snapshot:** Overall graduation rate stands at **49.93%**, offset by a **32.12% dropout rate**.
- **The Financial Penalty**: Students carrying tuition debt face a **62% dropout rate** (compared to 28.3% for non-debtors). Conversely, scholarship holders achieve a **76% graduate rate**, identifying financial aid as the most potent retention lever.
- **Non-Traditional Age Risk**: Attrition exceeds **58%** for students aged 25 or older, indicating a need for adult-learning schedule flexibility.

### Page 2: Academic Progression
<img width="912" height="517" alt="image" src="https://github.com/user-attachments/assets/e90ffc52-7f2e-4a48-9bb8-13969378d415" />

- **The First-Semester Tipping Point:**
    - Passing all semester 1 units yields an **80.7% graduation rate**.
    - Failing 1 unit: Dropout risk doubles to **16.2%**.
    - Failing 2 units: Dropout rate jumps to **41.4%**.
    - **Failing 3 or more units**: Attrition hits **79.2%**.
- **Curricular Cliff:** The Tourism curriculum experiences the steepest academic decline (-12.4% pass rate drop from Sem 1 to Sem 2), followed by Basic Education (-6.9%) and Informatics Engineering (-5.3%), pointing to structural friction when transitioning into specialization coursework.

### Page 3: At-Risk Intervention Workbench
<img width="1452" height="818" alt="image" src="https://github.com/user-attachments/assets/41195cb4-e461-46e4-a82c-e0cdba776f97" />

- **Target Cohort:** 794 currently enrolled students.
- **Actionable Triage:** 174 High-Risk students and 90 active tuition debtors.
- **Operational Action Grid:** Equips academic advisors with student ID, scholarship status, debtor, and grade trends so academic advisors can reach out for 1-on-1 counseling.

## 🚀 Strategic Recommendations
1. **Early Warning After Semester 1:** Set up an automated alert system immediately after Semester 1 results for any student failing 2 or more units, triggering mandatory advising prior to the Semester 2 census date.
2. **Emergency Financial Stabilization:** Provide flexible tuition installment schedules for the 90 currently enrolled debtors to eliminate immediate financial administrative barriers to study continuation.
3. **Targeted Curricular Intervention:** Partner with the Academic Deans of Tourism and Informatics Engineering to establish peer-assisted study sessions (PASS) for high-friction Semester 2 gatekeeper subjects.
