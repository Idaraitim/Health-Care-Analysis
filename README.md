#  Patient Healthcare Dashboard

**Analyst:** Idara Itim  
**Tool Used:** Microsoft Power BI  
**Focus:** Patient Demographics, Treatment Effectiveness, and Satisfaction Metrics  

---

##  Project Summary

This Power BI dashboard project analyzes patient healthcare data to uncover trends in demographics, treatment adherence, length of stay (LOS), and care satisfaction. The dashboard integrates custom tooltips and interactivity to deliver insights into patient experience and hospital performance.

---

##  Data Preparation in Power Query

Data was cleaned and transformed using Power BI’s Power Query Editor:

- Removed blank/irrelevant rows
- Standardized column values (e.g., converting follow-up and medication adherence fields to Yes/No)
- Cleaned geographic values (e.g., removed system artifacts like "Microsoft Word")
- Ensured correct data types:
  - **Numeric:** Age, LOS, Care Satisfaction
  - **Text:** Diagnosis, Gender, Discharge Destination

###  New Column Created:

**Age Group (via DAX):**
```DAX
Age Group = 
SWITCH(
    TRUE(),
    'Health Care'[Age] <= 12, "Child (0-12)",
    'Health Care'[Age] <= 19, "Teenager (13-19)",
    'Health Care'[Age] <= 35, "Young Adult (20-35)",
    'Health Care'[Age] <= 55, "Adult (36-55)",
    'Health Care'[Age] <= 75, "Senior (56-75)",
    "Elder (76+)"
)
```

---

##  Measures and KPIs (DAX)

- **Follow-Up Rate**
```DAX
FollowUp Rate = 
DIVIDE(
    CALCULATE(COUNTROWS('Health Care'), 'Health Care'[Follow-up Appointment] = "Yes"),
    COUNTROWS('Health Care')
)
```

- **Adherence Rate**
```DAX
Adherence Rate = 
DIVIDE(
    CALCULATE(COUNTROWS('Health Care'), 'Health Care'[Medication Adherence] = "Yes"),
    COUNTROWS('Health Care')
)
```

- **Average LOS**
```DAX
Avg LOS = AVERAGE('Health Care'[Length of Stay])
```

- **Average Satisfaction**
```DAX
Avg Satisfaction = AVERAGE('Health Care'[Care Satisfaction])
```

- **Comorbidity Count & Rate**
```DAX
Patients with Comorbidities = 
CALCULATE(
    COUNTROWS('Health Care'),
    NOT(ISBLANK('Health Care'[Comorbidities])) &&
    'Health Care'[Comorbidities] <> "None" &&
    'Health Care'[Comorbidities] <> 0 &&
    'Health Care'[Comorbidities] <> "0"
)

Comorbidity Rate = 
DIVIDE([Patients with Comorbidities], COUNTROWS('Health Care'))
```

---

## 📊 Dashboard Structure

### 1. Patient Overview Dashboard

**KPIs:**
- Total Patients
- Average Age
- Total with Comorbidities
- Comorbidity Rate

**Visuals:**
- Column chart: Patient count by Age Group
- Donut chart: Gender distribution
- Bar chart: Comorbidities by Gender
- Map: Patient Discharge Destinations
- Pie chart: LOS distribution by Gender

**Slicers:**
- Diagnosis
- Age Group

---

### 2. Treatment Effectiveness & Satisfaction Dashboard

**KPIs:**
- Adherence Rate
- Follow-Up Rate
- Average LOS
- Average Satisfaction

**Visuals:**
- Column chart: Adherence Rate by Age Group
- Donut chart: Care Satisfaction by Gender
- Column chart: Avg LOS by Primary Diagnosis
- Map: Discharge Destination for satisfied patients
- Area chart: LOS by Medication Adherence

**Slicers:**
- Primary Diagnosis

---

### 3. Tooltip Page

A mini insight page appears on hover, showing:
- Avg Satisfaction
- Avg LOS
- Age Group context

---

##  Insights Derived from Dashboard

- **High LOS Regions:** South East and North Central have the highest average Length of Stay (LOS), indicating resource strain or slower treatment turnover.
- **Patient Satisfaction:** Most patients rate care between 2.8 and 3.2 stars out of 5, with visible room for experience improvement.
- **Adherence & Follow-Up Gaps:** Less than 50% adherence and follow-up rate signals a need for better aftercare and patient engagement strategies.
- **Gender Distribution:** Fairly balanced gender representation; trends are not significantly skewed.
- **Discharge Outcomes:** Home discharges are more common and correlated with higher satisfaction ratings.

---

##  Recommendations

Based on the data analysis and visualizations presented in the dashboard, the following actions are recommended to improve patient outcomes and operational efficiency:

1. **Enhance Follow-Up Programs**  
   With less than 50% follow-up rates observed, the hospital should establish a stronger patient follow-up system—such as SMS/email reminders, automated calls, or digital apps to encourage post-discharge check-ins.

2. **Improve Medication Adherence Initiatives**  
   Launch medication education campaigns or assign case managers for patients with chronic conditions to improve adherence. Incorporating pharmacy check-ins could help reinforce compliance.

3. **Target Support for Seniors and Adults**  
   Since Adults (36–55) and Seniors (56–75) form the largest age group, healthcare resources and interventions should be tailored to the needs of these demographics, including chronic disease management and home care services.

4. **Monitor and Reduce Length of Stay (LOS)**  
   LOS trends across different diagnoses and adherence levels suggest optimization opportunities. The hospital should conduct root-cause analysis where LOS is above average and work on care pathway streamlining.

5. **Leverage High Satisfaction Trends**  
   Patients discharged to **home** reported higher satisfaction. This suggests the need to expand home-care services or rehabilitation-to-home pathways where medically appropriate.

6. **Track Comorbidities Proactively**  
   With a 76% comorbidity rate, healthcare providers should consider implementing **early screening tools** for common co-occurring conditions to reduce complications and re-admission.
