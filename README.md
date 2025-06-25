# Loan & Financial Risk Analysis Dashboard

## 📊 Overview
This project is a **Financial Analytics Dashboard** created in **Power BI** to analyze customer demographics, loan performance, and financial risk for a **loan service provider**.

Through rich interactive visuals, this dashboard enables stakeholders to:
- Understand borrower profiles
- Monitor loan issuance and status
- Identify high-risk segments and defaults
- Make informed, data-driven decisions

---

## 📁 Dataset Description

### **Customer Details**
| Column           | Description                        |
|------------------|------------------------------------|
| Customer ID      | Unique customer identifier         |
| Name             | Customer name                      |
| Age              | Age in years                       |
| Gender           | Gender (Male/Female/Other)         |
| Income           | Annual income                      |
| Employment Status| Job status (Full-time, Part-time)  |
| Education Level  | Highest qualification              |
| Credit Score     | Credit rating                      |

### **Loan Details**
| Column             | Description                              |
|--------------------|------------------------------------------|
| Loan ID            | Unique loan ID                           |
| Customer ID        | Reference to Customer                    |
| Loan Amount        | Amount of loan issued                    |
| Interest Rate      | Rate of interest (%)                     |
| Term               | Loan duration                            |
| Issue Date         | Date of loan issuance                    |
| Status             | Current status (Active/Defaulted/Closed) |
| Monthly Installment| EMI amount                               |

---

## ⚙️ Data Processing Workflow

### ✅ Data Cleaning
- Removed duplicates and blanks
- Verified and standardized data types
- Handled missing values

### ✅ Data Categorization
- **Age Group**: Young, Middle-aged, Senior, Elder
- **Credit Score Bucket**: Poor to Excellent
- **Income Group**: Low, Medium, High
- **Risk Category** (New Column):
  - `<580` → High Risk
  - `<670` → Moderate Risk
  - `<740` → Low Risk
  - `≥740` → Very Low Risk

### ✅ Data Modeling
- **One-to-Many**:  
  - `Customer_ID` → Customer to Loan  
  - `Issue_Date` → Loan to Date Table  
- Used DAX: `CALENDARAUTO()` for Date Table  
- Merged customer and loan details for reporting

---

## 📊 Key Measures

- **Total Loan Amount**
- **Average Interest Rate**
- **Average Monthly Installment**
- **Total Customers**
- **Average Age**
- **Average Income**
- **Defaulted Loans**
- **Default Loan Amount**
- **High Risk Loans**
- **High Risk Loan Amount**

---

## 🧠 Dashboard Pages

### 📌 Page 1 – Customer Demographics
- **Cards**: Total Customers, Avg Age, Avg Income  
- **Slicers**: Income Group, Credit Score Bucket  
- **Visuals**:  
  - Pie Chart (Gender)  
  - Pie Chart (Education Level)  
  - Clustered Bar (Avg Credit Score by Gender & Education Level)

![Customer Demographics]

---

### 📌 Page 2 – Loan Portfolio & Performance
- **Cards**: Total Loan Amount, Avg Interest Rate, Avg EMI  
- **Slicers**: Credit Score Bucket, Income Group  
- **Visuals**:  
  - Pie Chart (Loan Types)  
  - Stacked Column (Loan Type by Status)  
  - Top 10 Loans (Active & Defaulted Tables)

![Loan Portfolio]

---

### 📌 Page 3 – Financial Risk Analysis
- **Cards**: Defaulted Loans, Default Amount, High Risk Loans, High Risk Amount  
- **Slicers**: Credit Score Bucket, Income Group  
- **Visuals**:  
  - Donut Charts (Defaulted & High Risk Loan Amount by Employment Type)  
  - Matrix Tables (Income vs Education Level)  
  - Column Chart (Credit Score vs Customers by Employment)

![Financial Risk Analysis]

---

## 🏆 Insights Uncovered

- Most **defaults** are in **low-income and poor credit score** groups  
- **High-risk loans** are more common among **part-time and self-employed** segments  
- Graduate and postgraduate customers generally have **higher credit scores**  
- Loan types like **Student and Personal loans** show higher default rates  
- **EMI and interest trends** vary by income and education level  

---

## 🚀 Business Value

- Helps track **loan portfolio health**
- Enables **risk-based targeting** of customers
- Assists in setting **interest and term policies**
- Supports credit teams with **segmentation and filtering tools**

---

## 💻 How to Use

### Requirements
- [Power BI Desktop](https://powerbi.microsoft.com/en-us/downloads/) (Free)
