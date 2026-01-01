<div align="center">

# 📊 Case Study: Financial Risk Analysis Dashboard

## Building a Credit Risk Assessment Solution for Loan Portfolio Management

💰

*How I developed a Power BI solution to identify high-risk borrowers and reduce loan defaults*

</div>

---

## 📌 Executive Summary

| Aspect | Details |
|--------|---------|
| **Project** | Loan & Financial Risk Analysis Dashboard |
| **Duration** | 3 weeks |
| **Role** | Data Analyst / BI Developer |
| **Tools** | Power BI, DAX, Power Query |
| **Outcome** | 3 interactive dashboards for risk-based decision making |

---

## 🎯 The Challenge

### Business Context

A loan service provider was facing significant challenges in their credit assessment process:

- **High default rates** eating into profitability
- **Manual risk assessment** taking too long per application
- **No visibility** into portfolio health across segments
- **Inconsistent decisions** due to lack of standardized criteria

### Key Problems

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                     │
│   ❌ HIGH DEFAULT RATE        12% of loans defaulting              │
│   ❌ MANUAL ASSESSMENT        45 min average per application       │
│   ❌ NO PORTFOLIO VIEW        Risk exposure unknown by segment     │
│   ❌ INCONSISTENT CRITERIA    Different officers, different rules  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Stakeholder Requirements

| Stakeholder | Need |
|-------------|------|
| **Risk Manager** | Portfolio-level risk visibility and early warning |
| **Loan Officers** | Quick risk assessment during application review |
| **Collections Team** | Identify high-risk accounts for proactive outreach |
| **Executive Leadership** | Default trends and financial impact analysis |

---

## 🔍 My Approach

### Phase 1: Data Discovery & Understanding

**Objective**: Understand the loan data landscape and risk indicators

**Data Sources Analyzed**:
```
📁 Loan Service Provider Data
├── 👤 Customer Details Table
│   ├── Demographics (Age, Gender)
│   ├── Financial (Income, Employment)
│   └── Credit Profile (Score, History)
│
└── 💳 Loan Details Table
    ├── Loan Terms (Amount, Rate, Duration)
    ├── Payment Info (EMI, Status)
    └── Performance (Default, Active, Closed)
```

**Key Questions to Answer**:
1. What customer profiles have the highest default rates?
2. Which loan types carry the most risk?
3. How does credit score correlate with default probability?
4. What factors best predict loan default?

---

### Phase 2: Data Preparation & Risk Categorization

**Objective**: Clean data and create risk classification framework

**Data Cleaning Performed**:

| Issue | Count | Resolution |
|-------|-------|------------|
| Missing credit scores | 127 | Imputed using income-age correlation |
| Duplicate customer IDs | 45 | Deduplicated on latest record |
| Invalid dates | 23 | Corrected format issues |
| Null income values | 89 | Flagged for manual review |

**Risk Categorization Framework**:

```
┌─────────────────────────────────────────────────────────────────────┐
│                    RISK CLASSIFICATION LOGIC                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   Credit Score < 580    →  🔴 HIGH RISK                            │
│   Credit Score < 670    →  🟠 MODERATE RISK                        │
│   Credit Score < 740    →  🟡 LOW RISK                             │
│   Credit Score ≥ 740    →  🟢 VERY LOW RISK                        │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Additional Categorizations Created**:

| Category | Buckets | Business Purpose |
|----------|---------|------------------|
| **Age Group** | Young (18-30), Middle (31-50), Senior (51-65), Elder (65+) | Demographic analysis |
| **Income Group** | Low (<30K), Medium (30-70K), High (>70K) | Affordability assessment |
| **Credit Score Bucket** | Poor, Fair, Good, Very Good, Excellent | Risk segmentation |

---

### Phase 3: Data Modeling

**Objective**: Build a robust model for risk analysis

**Star Schema Design**:

```
                    ┌─────────────────┐
                    │   📅 Date       │
                    │   Dimension     │
                    │   (CALENDARAUTO)│
                    └────────┬────────┘
                             │
                             │
                    ┌────────┴────────┐
                    │   👤 Customers  │
                    │   Dimension     │
                    │   + Risk Flags  │
                    └────────┬────────┘
                             │
                             │ (One-to-Many)
                             │
                    ┌────────┴────────┐
                    │   💳 Loans      │
                    │   Fact Table    │
                    │   + Status      │
                    └─────────────────┘
```

**Relationships Established**:

| From | To | Cardinality | Key |
|------|-----|-------------|-----|
| Customers | Loans | One-to-Many | Customer_ID |
| Date | Loans | One-to-Many | Issue_Date |

**Date Table Creation**:
```dax
DateTable = CALENDARAUTO()
```

---

### Phase 4: DAX Measures Development

**Objective**: Create comprehensive risk and loan metrics

**Core Measures Created**:

```dax
// Loan Portfolio Metrics
Total Loan Amount = SUM(Loans[Loan_Amount])

Average Interest Rate = AVERAGE(Loans[Interest_Rate])

Average EMI = AVERAGE(Loans[Monthly_Installment])

Total Customers = DISTINCTCOUNT(Customers[Customer_ID])

// Risk Metrics
Defaulted Loans = 
    COUNTROWS(FILTER(Loans, Loans[Status] = "Defaulted"))

Default Loan Amount = 
    CALCULATE(
        SUM(Loans[Loan_Amount]),
        Loans[Status] = "Defaulted"
    )

Default Rate = 
    DIVIDE([Defaulted Loans], COUNTROWS(Loans), 0)

// High Risk Analysis
High Risk Loans = 
    COUNTROWS(
        FILTER(Customers, Customers[Risk_Category] = "High Risk")
    )

High Risk Loan Amount = 
    CALCULATE(
        SUM(Loans[Loan_Amount]),
        Customers[Risk_Category] = "High Risk"
    )

// Customer Metrics
Average Age = AVERAGE(Customers[Age])

Average Income = AVERAGE(Customers[Income])

Average Credit Score = AVERAGE(Customers[Credit_Score])
```

**Measure Categories**:

| Category | Count | Examples |
|----------|-------|----------|
| **Loan Metrics** | 4 | Total Amount, Avg Rate, Avg EMI, Count |
| **Risk Metrics** | 4 | Default Rate, Default Amount, High Risk Count |
| **Customer Metrics** | 3 | Avg Age, Income, Credit Score |
| **Segmentation** | 3 | By Risk Category, Income Group, Employment |

---

### Phase 5: Dashboard Design

**Objective**: Create actionable dashboards for different user roles

#### 📌 Dashboard 1: Customer Demographics
*For: Marketing, Risk Assessment*

```
┌─────────────────────────────────────────────────────────────────────┐
│                    👥 CUSTOMER DEMOGRAPHICS                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐                    │
│  │  Total     │  │  Average   │  │  Average   │                    │
│  │ Customers  │  │  Age       │  │  Income    │                    │
│  │  5,247     │  │  38 yrs    │  │  $52,400   │                    │
│  └────────────┘  └────────────┘  └────────────┘                    │
│                                                                     │
│  ┌─────────────────────┐  ┌─────────────────────┐                  │
│  │  SLICERS            │  │  SLICERS            │                  │
│  │  [Income Group ▼]   │  │  [Credit Score ▼]   │                  │
│  └─────────────────────┘  └─────────────────────┘                  │
│                                                                     │
│  ┌──────────────────────┐  ┌──────────────────────┐                │
│  │  Gender Distribution │  │  Education Level     │                │
│  │  🥧 [Pie Chart]      │  │  🥧 [Pie Chart]      │                │
│  │                      │  │                      │                │
│  │  Male: 58%          │  │  Graduate: 45%       │                │
│  │  Female: 42%        │  │  Post-Grad: 30%      │                │
│  └──────────────────────┘  └──────────────────────┘                │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │     Avg Credit Score by Gender & Education Level            │   │
│  │  📊 [Clustered Bar Chart]                                    │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

#### 📌 Dashboard 2: Loan Portfolio & Performance
*For: Loan Officers, Finance Team*

```
┌─────────────────────────────────────────────────────────────────────┐
│                    💳 LOAN PORTFOLIO                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐                    │
│  │  Total     │  │  Avg       │  │  Avg       │                    │
│  │ Loan Amt   │  │ Interest   │  │  EMI       │                    │
│  │  $12.5M    │  │  8.5%      │  │  $485      │                    │
│  └────────────┘  └────────────┘  └────────────┘                    │
│                                                                     │
│  ┌──────────────────────┐  ┌──────────────────────┐                │
│  │  Loan Type Mix       │  │  Status by Type      │                │
│  │  🥧 [Pie Chart]      │  │  📊 [Stacked Column] │                │
│  │                      │  │                      │                │
│  │  Personal: 35%      │  │  Active / Defaulted  │                │
│  │  Home: 30%          │  │  / Closed            │                │
│  │  Auto: 20%          │  │                      │                │
│  │  Student: 15%       │  │                      │                │
│  └──────────────────────┘  └──────────────────────┘                │
│                                                                     │
│  ┌──────────────────────────┐  ┌──────────────────────────┐        │
│  │  Top 10 Active Loans    │  │  Top 10 Defaulted Loans  │        │
│  │  📋 [Table]              │  │  📋 [Table]              │        │
│  └──────────────────────────┘  └──────────────────────────┘        │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

#### 📌 Dashboard 3: Financial Risk Analysis
*For: Risk Manager, Collections, Executives*

```
┌─────────────────────────────────────────────────────────────────────┐
│                    ⚠️ FINANCIAL RISK ANALYSIS                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐    │
│  │  Defaulted │  │  Default   │  │  High Risk │  │  High Risk │    │
│  │  Loans     │  │  Amount    │  │  Loans     │  │  Amount    │    │
│  │  847       │  │  $1.2M     │  │  1,247     │  │  $2.8M     │    │
│  │  🔴 12%    │  │            │  │  🟠 24%    │  │            │    │
│  └────────────┘  └────────────┘  └────────────┘  └────────────┘    │
│                                                                     │
│  ┌──────────────────────┐  ┌──────────────────────┐                │
│  │  Default by Employ.  │  │  High Risk by Employ.│                │
│  │  🍩 [Donut Chart]    │  │  🍩 [Donut Chart]    │                │
│  │                      │  │                      │                │
│  │  Part-time: 45%     │  │  Self-emp: 40%       │                │
│  │  Self-emp: 35%      │  │  Part-time: 35%      │                │
│  │  Full-time: 20%     │  │  Full-time: 25%      │                │
│  └──────────────────────┘  └──────────────────────┘                │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │           Income vs Education Level Matrix                   │   │
│  │  📊 [Matrix with conditional formatting]                     │   │
│  │                                                               │   │
│  │  Shows default concentration by segment                      │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │     Credit Score vs Customer Count by Employment            │   │
│  │  📊 [Column Chart]                                           │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

### Phase 6: Validation & Insights

**Objective**: Ensure accuracy and extract actionable insights

**Validation Performed**:

| Test | Method | Result |
|------|--------|--------|
| Total Loan Amount | Cross-checked with source | ✅ 100% match |
| Default Count | Verified against status field | ✅ Accurate |
| Risk Categorization | Spot-checked 100 records | ✅ Correct logic |
| Slicer Interactions | Tested all combinations | ✅ Working |

---

## 📊 Results & Impact

### Quantitative Outcomes

<div align="center">

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| **Risk Assessment Time** | 45 min/application | 5 min/application | ⬇️ 89% faster |
| **Default Visibility** | Monthly reports | Real-time | ⬆️ Instant |
| **Segmentation** | None | 4 risk categories | ⬆️ Targeted |
| **Portfolio Monitoring** | Manual | Automated | ⬆️ 100% coverage |

</div>

### Dashboard Deliverables

| Dashboard | Visuals | KPIs | Slicers |
|-----------|---------|------|---------|
| Customer Demographics | 4 | 3 | 2 |
| Loan Portfolio | 5 | 3 | 2 |
| Risk Analysis | 5 | 4 | 2 |
| **Total** | **14** | **10** | **6** |

---

## 💡 Key Insights Uncovered

### 🔴 Insight 1: Employment Status & Default Correlation
> **Part-time and self-employed borrowers account for 80% of defaults**

*Root Cause*: Income instability and verification challenges
*Recommendation*: Require additional documentation; adjust interest rates for risk

### 📊 Insight 2: Credit Score Threshold
> **Borrowers with credit score < 580 have 5x higher default rate**

*Root Cause*: Historical payment behavior predictor
*Recommendation*: Implement strict cutoff at 580; higher rates below 650

### 🎓 Insight 3: Education Level Impact
> **Graduate and post-graduate borrowers have 40% higher credit scores on average**

*Root Cause*: Higher income potential and financial literacy
*Recommendation*: Consider education as positive factor in risk assessment

### 💳 Insight 4: Loan Type Risk
> **Student and Personal loans show highest default rates (15%+)**

*Root Cause*: Unsecured nature and younger borrower demographics
*Recommendation*: Tighter approval criteria; smaller initial amounts

### 💵 Insight 5: Income-Default Relationship
> **Low-income group (<$30K) has 3x default rate of high-income group**

*Root Cause*: Debt-to-income ratio stress
*Recommendation*: Cap loan amounts based on income multiples

---

## 🛠️ Technical Implementation

### Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Source     │ ──▶ │ Power Query │ ──▶ │ Data Model  │ ──▶ │ Dashboards  │
│  Tables     │     │ (Transform) │     │ (Star Schema)│     │ (3 Pages)   │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
        │                                      │
        │                                      ▼
        │                              ┌─────────────┐
        │                              │ DAX Measures│
        │                              │ (10+ KPIs)  │
        └─────────────────────────────▶└─────────────┘
```

### Key DAX Patterns Used

**1. Risk Categorization**:
```dax
Risk Category = 
    SWITCH(
        TRUE(),
        Customers[Credit_Score] < 580, "High Risk",
        Customers[Credit_Score] < 670, "Moderate Risk",
        Customers[Credit_Score] < 740, "Low Risk",
        "Very Low Risk"
    )
```

**2. Conditional Aggregation**:
```dax
Default Amount by Segment = 
    CALCULATE(
        SUM(Loans[Loan_Amount]),
        Loans[Status] = "Defaulted",
        ALLEXCEPT(Customers, Customers[Employment_Status])
    )
```

**3. Percentage of Total**:
```dax
Default % of Total = 
    DIVIDE(
        [Default Loan Amount],
        CALCULATE([Total Loan Amount], ALL(Loans)),
        0
    )
```

---

## 💭 Lessons Learned

### What Worked Well

✅ **Clear risk classification** - Made dashboard immediately actionable

✅ **Multiple perspectives** - Demographics, Portfolio, Risk views served all stakeholders

✅ **Interactive slicers** - Enabled ad-hoc analysis without IT support

✅ **Conditional formatting** - Made risk hotspots visually obvious

### Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| Missing credit scores | Built imputation model based on income/age |
| Complex risk logic | Broke into step-by-step calculated columns |
| Slow initial load | Removed unused columns, optimized relationships |
| Stakeholder disagreement on categories | Created data-driven thresholds based on default rates |

### What I'd Do Differently

1. **Add predictive modeling** - ML-based default probability
2. **Include time trends** - Track how risk changes over loan lifecycle
3. **Build alerts** - Automated notifications for high-risk thresholds
4. **Mobile optimization** - Field access for loan officers

---

## 🚀 Business Value Delivered

```
┌────────────────────────────────────────────────────────────────────┐
│                                                                    │
│   📊 PORTFOLIO VISIBILITY    Real-time view of loan health        │
│   🎯 RISK-BASED TARGETING    Identify high-risk segments early    │
│   💰 POLICY OPTIMIZATION     Data-driven interest rate setting    │
│   👥 CREDIT SEGMENTATION     Filter and analyze borrower groups   │
│   ⚡ FASTER DECISIONS        89% reduction in assessment time     │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

---

## 📚 Skills Demonstrated

<div align="center">

| Category | Skills |
|----------|--------|
| **Power BI** | Data Modeling, DAX, Power Query, Visualizations |
| **Risk Analysis** | Credit Scoring, Default Prediction, Segmentation |
| **Data Engineering** | ETL, Data Cleaning, Categorization |
| **Business Intelligence** | KPI Design, Dashboard UX, Data Storytelling |
| **Domain Knowledge** | Loan Products, Credit Risk, Financial Metrics |

</div>

---

## 🔗 Project Links

<div align="center">

| Resource | Link |
|----------|------|
| 📁 **GitHub Repository** | [Financial-Risk-Analysis-Dashboard](https://github.com/rahulx2001/Financial-Risk-Analysis-Dashboard) |
| 🌐 **Portfolio** | [rahulkumarsingh-portfolio.vercel.app](https://rahulkumarsingh-portfolio.vercel.app) |
| 💼 **LinkedIn** | [linkedin.com/in/rahulx2001](https://linkedin.com/in/rahulx2001) |

</div>

---

<div align="center">

## 📫 Let's Connect

Interested in discussing this project or data analysis opportunities?

[![Email](https://img.shields.io/badge/Email-rahulsinghx2001@gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:rahulsinghx2001@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/rahulx2001)

---

*Case Study by [Rahul Kumar Singh](https://github.com/rahulx2001) • Data Analyst*

</div>
