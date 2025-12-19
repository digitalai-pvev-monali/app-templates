# 🏆 SCV Sales Intelligence Platform  

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

## 🧱 Architecture  

(GitHub will render Mermaid diagrams in README if your viewer supports it; otherwise use a renderer that supports Mermaid.)

Variant 1 — Horizontal (LR) — clean pipeline with grouping and arrow labels
```mermaid
flowchart LR
  subgraph Sources["Source Tables"]
    direction TB
    A[Customer Pipeline]
    B[Customer Journey]
    C[Call Log Detail]
    E[Stock]
    F[Retail]
  end

  subgraph Processing["Scoring Engine"]
    D[SCV Scoring Engine]
  end

  subgraph Storage["Delta Layer"]
    G[(scv_customer_score<br/>Delta Table)]
  end

  subgraph Consumers["Applications & Dashboards"]
    H[SCV Lead App<br/>(Gradio + Databricks)]
    I[Analytics Dashboard<br/>(Databricks)]
  end

  A -->|opty_id| D
  B -->|opty_id| D
  C -->|opty_id| D
  E -->|Dealer_Code| D
  F -->|Dealer_Code| D

  D -->|MERGE / upsert| G
  G --> H
  G --> I

  classDef sources fill:#f3f4f6,stroke:#333,stroke-width:1px;
  classDef engine fill:#fff7cc,stroke:#b58900,stroke-width:1px;
  classDef storage fill:#e6f7ff,stroke:#0077b6,stroke-width:2px;
  classDef consumers fill:#ecfdf5,stroke:#0f5132,stroke-width:1px;

  class A,B,C,E,F sources;
  class D engine;
  class G storage;
  class H,I consumers;

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
`/Workspace/TMDAi_Hackathon_Team_4/svc_score_script.py`  

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

