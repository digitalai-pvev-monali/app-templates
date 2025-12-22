<img width="204" height="114" alt="image" src="https://github.com/user-attachments/assets/607a0081-5d54-49b2-bb68-0894d2490987" /># 🏆 SCV Sales Intelligence Platform  

**TMDAi Hackathon 2025 — Team 4**  
*Mobile-First | AI-Ready | Action-Driven*

---

## 🚀 Overview  
The SCV business generates strong enquiry and sales volumes, but lacks real-time sales intelligence, resulting in delayed actions and missed conversions.Sales teams currently depend on manual daily reviews, verbal updates and delayed regional and zonal reports

---

## 🔗 Live Demo  
👉 **[Access the Live App](https://hello-world-a-2106525843804885.aws.databricksapps.com/)**  

---

## 📖 Problem Statement  
SCV sales teams face critical operational challenges due to fragmented and delayed intelligence:  
- **Late identification** of hot customers and high-potential pockets.  
- **Unclear “Next Best Action”** for frontline teams — effort spent searching, not selling.  
- **Inconsistent messaging** across 600+ touchpoints, diluting strategic intent.  

*Impact:* Missed conversions, delayed reactions, and inefficient field efforts.

---

## 🎯 Solution  
We built an **end-to-end SCV Intelligence Platform** on Databricks that:  
1. **Integrates** multi-source SCV data.  
2. **Computes** a business-driven **SCV Score** for each opportunity.  
3. **Surfaces** prioritized daily actions via a **mobile-first application**.  
4. **Supports** strategic oversight with interactive **Databricks Dashboards**.
5. **Email Notification** for Macro Market Hotspot Alert – Top 3 Talukas

This is a **functional prototype** of the eGuru SCV application, enhanced with intelligent scoring and real-time recommendations.

---

## 🧱 High Level Architecture  
<img width="530" height="401" alt="image" src="https://github.com/user-attachments/assets/61a8c442-a3e7-43eb-8f88-7d0ffdc8c809" />


---

## 📊 Data Foundation  
### Source Tables  
--------------------------------------------------------------------------------------
| Table               | Purpose                                   | Joining Parameter |
|-------------------  |-------------------------------------------|-------------------|
| `Customer Pipeline` | Primary lead & opportunity data           | opty_id           |
| `Customer Journey`  | Customer interactions & lifecycle events  | opty_id           |
| `Call Log Detail`   | Call duration, follow-ups & engagement    | opty_id           |
| `Stock`             | Vehicle stock availability                | Dealer_Code       |
| `Retail`            | Vehicle sales / retail data               | Dealer_Code       | 
---------------------------------------------------------------------------------------
### Output Table (Single Source of Truth)  
`tmdai_hackathon.tmdai_hackathon_team_4.scv_customer_score`  
This table powers both the application and dashboards.

---

## ⚙️ SCV Scoring Framework  
Each opportunity is evaluated across business-relevant dimensions:  

----------------------------------------------------------------------
| Score Component              | Business Meaning                    |
|------------------------------|-------------------------------------|
| `lead_age_score`             | Freshness of enquiry                |
| `expected_closure_score`     | Probability of near-term conversion |
| `opty_category_score`        | Opportunity type importance         |
| `lead_classification_score`  | Quality of lead                     |
| `customer_type_score`        | Retail / Fleet / Institutional      |
| `followup_score`             | Follow-up discipline                |
| `avg_call_duration_score`    | Engagement intensity                |
| `stock_availability_score`   | Product readiness                   |
| `total_score`                | **Final SCV priority score**        |
----------------------------------------------------------------------

> Scores are calculated using **real business logic**, not arbitrary weights.

---

## 🔄 Intelligent Incremental Processing  
The scoring engine uses a **MERGE-based upsert strategy**:  
- ✅ If `opty_id` exists → **Update scores**  
- ➕ If `opty_id` is new → **Insert record**  

This makes the solution **idempotent, scalable, and production-ready**.

---

## 📁 Scoring Engine  
**Script Location:**  
`/Workspace/TMDAi_Hackathon_Team_4/svc_customer_score_script.py`  

**Responsibilities:**  
- Read source tables  
- Apply joins and transformations  
- Compute SCV scores  
- Create/update `scv_customer_score` table  

---

## 📱 Mobile-First SCV Application (Gradio + Databricks)  
**Live App:** [https://hello-world-a-2106525843804885.aws.databricksapps.com/](https://hello-world-a-2106525843804885.aws.databricksapps.com/)  

**Key Capabilities:**  
- Mobile-friendly UI inspired by **eGuru**  
- Displays **Top Leads** based on SCV score  
- Simple, fast, and **action-oriented**  
- Answers one question every morning:  
  > **“What should I focus on today?”**  

---

##  🌍 Macro Market Hotspot Detection & Alerting System (SQL Gold Tables + Python Alert Agent | 5-Day View)  
**What Problem Does This Solve:** 
- Sales opportunities are often missed because rising demand in specific micro-markets (Talukas) is not detected early, or supply is not aligned with demand. 
**Key Capabilities:**  
- 📈 Detects **high-growth Talukas**
- ⚖️ Identifies **demand–supply mismatches**
- 🚨 Sends **automated alerts to leadership**
- ⚡ Enables **fast, data-backed decisions**
  **High Level Architect:**
  ┌─────────────────────────┐ 

│   Source Data Systems   │ 

│-------------------------│ 

│ • Dealer Leads          │ 

│ • Customer Intent       │ 

│ • Retail Sales          │ 

│ • Vehicle Stock         │ 

└───────────┬─────────────┘ 

            │ 

            ▼ 

┌─────────────────────────┐ 

│  SQL Feature Engineering│ 

│-------------------------│ 

│ • Geo Mapping           │ 

│ • 5-Day Metrics         │ 

│ • Growth Calculations   │ 

└───────────┬─────────────┘ 

            │ 

            ▼ 

┌─────────────────────────┐ 

│  Gold Market Tables     │ 

│-------------------------│ 

│ • Dealer Features       │ 

│ • Taluka Hotspots       │ 

└───────────┬─────────────┘ 

            │ 

            ▼ 

┌─────────────────────────┐ 

│ Python Alert Agent      │ 

│-------------------------│ 

│ • Priority Scoring      │ 

│ • Top 3 Ranking         │ 

│ • Email Notification    │ 

└─────────────────────────┘ 

---
## 🧮 SQL Feature Engineering Pipeline (Gold Tables)
### 3.1 Dealer → Latest Geography Mapping
**Purpose:**  
Ensure every dealer is mapped to the **most recent Taluka & District** based on latest activity.

**Logic:**
- Uses latest available pipeline stage (C0 → C3)
- Picks the most recent geography per dealer

**Output View:**  
`dealer_geo`

---

### 3.2 Dealer Lead & Intent Metrics (5-Day Window)
**Purpose:**  
Measure **current demand momentum** and compare it with historical behavior.

**Metrics:**
- Leads in last 5 days
- Leads in previous window
- High-intent leads (Hot / Warm / Live Deal)
- Intent growth %

**Output View:**  
`dealer_leads_5d`

---

### 3.3 Dealer Retail Performance
**Purpose:**  
Track whether **demand is converting into sales**.

**Metrics:**
- Retail volume (current vs previous window)

**Output View:**  
`dealer_retail_5d`

---

### 3.4 Dealer Stock Availability
**Purpose:**  
Assess **supply readiness**.

**Metrics:**
- Total stock
- Saleable stock

**Output View:**  
`dealer_stock`

---

### 3.5 Dealer Market Feature Gold Table
**Purpose:**  
Create a **single dealer-level feature table** combining all demand and supply signals.

**Metrics Included:**
- Leads, intent, retail, stock
- Lead growth
- Intent growth
- Retail growth

**Gold Table:**  
`dealer_market_features_5d`

---

### 3.6 Taluka-Level Hotspot Aggregation
**Purpose:**  
Aggregate dealer signals to detect **micro-market behavior**.

**Metrics:**
- Total leads, intent, retail, stock
- Average growth rates
- Dealer count

**Hotspot Classification Logic:**

| Condition | Status |
|---------|--------|
| < 2 dealers | NOISE |
| High lead + intent + retail growth | HOTSPOT |
| High lead growth | EMERGING |
| Else | STABLE |

**Gold Table:**  
`macro_market_hotspot_taluka_5d`

---

### 3.7 District-wise Top 3 Talukas
**Purpose:**  
Surface the **most actionable Talukas per district**.

**Logic:**
- Rank by hotspot category
- Then by lead volume

**Output View:**  
`district_top3_talukas`

---
## 🚨 Python Alert Agent (Automated Notifications)
Automatically notify leadership about the **Top 3 Taluka Hotspots** across districts.
---

### 4.1 Input
Reads from:
- `macro_market_hotspot_taluka_5d`

Filters:
- HOTSPOT
- EMERGING

---

### 4.2 Business Priority Scoring

Each Taluka is scored using **business-driven logic**:

| Signal | Impact |
|------|--------|
| HOTSPOT | High Priority |
| EMERGING | Medium Priority |
| Lead growth | Demand momentum |
| Intent growth | Buyer seriousness |
| Zero retail | Supply gap |
| Excess stock | Lower urgency |

➡ Ensures **priority ≠ volume-only**, but **true business impact**.

---

### 4.3 Top 3 Taluka Selection
- Ranked by priority score
- Only **Top 3 Talukas** selected per alert

---

### 4.4 Explainable Reason Codes
Each Taluka includes:
- Lead volume & growth %
- High-intent lead count
- Retail gap (if any)
- Available stock

➡ Alerts are **actionable, not just informative**.

---

### 4.5 HTML Email Alert
**Includes:**
- Styled, readable table
- Color-coded hotspot status
- Business interpretation

**Subject Example:**  
🚨 *Macro Market Hotspot Alert | Top 3 Talukas | <Date>*

---

### 4.6 Automated Delivery
- SMTP (Gmail)
- Fully automated
- No manual intervention
---

## 📈 Databricks Dashboard – Strategic View  
**Dashboard Name:** `team4`  

**Insights Provided:**  
- 🥇 **Top 20 Leads** by SCV Score  
- 🔍 **Enquiry-wise Top 5 Leads**  
- 📈 **Leads by PPL**  

Supports **regional and leadership decision-making**.

---

## 🛠 Technology Stack  
- **Data Engineering:** PySpark, Databricks SQL  
- **Storage:** Delta Lake  
- **Scoring Engine:** Python  
- **UI:** Gradio  
- **Analytics:** Databricks Dashboard  
- **Deployment:** Databricks Apps
- **Package:** Pandas
- **EMAIL Protocol:** SMTP


---

## 🏆 Business Impact  
- **Faster reaction** to market changes  
- **Clear daily priorities** for field teams  
- **Consistent messaging** across touchpoints  
- **Foundation for AI-driven** SCV intelligence  

---

## 👥 Team  
**TMDAi Hackathon 2025 — Team 4**  
Tata Motors  

---

## 📄 License  
This project was developed as part of the **TMDAi Hackathon 2025**. All rights reserved by Team 4 and Tata Motors.

