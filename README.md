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
flowchart TD
    %% Data Sources Layer
    A[Customer Pipeline<br/>Leads & Opportunities]
    B[Customer Journey<br/>Lifecycle & Touchpoints]
    C[Call Log Detail<br/>Calls & Follow-ups]
    
    %% Scoring Engine
    D[SCV Scoring Engine<br/>Business Rules & Intelligence Layer]
    
    %% Dealer & Supply Data
    E[Stock<br/>Dealer Inventory]
    F[Retail<br/>Vehicle Sales Data]
    
    %% Output Layer
    G[scv_customer_score<br/>Delta Table<br/>Single Source of Truth]
    
    %% Application Layer
    H[SCV Lead Intelligence<br/>Mobile App<br/>Gradio Interface]
    I[Analytics Dashboard<br/>Databricks Dashboard]
    
    %% Data Flow
    A -- opty_id --> D
    B -- opty_id --> D
    C -- opty_id --> D
    E -- Dealer_Code --> D
    F -- Dealer_Code --> D
    
    %% Processing Flow
    D -- Enriched scoring --> G
    
    %% Consumption Flow
    G --> H
    G --> I
    
    %% Scoring Components
    subgraph D_sub [Scoring Components]
        D1[Lead-level Scores]
        D2[Dealer & Supply Signals]
        
        subgraph D1_sub [Lead Scoring]
            D1a[lead_age_score]
            D1b[expected_closure_score]
            D1c[opty_category_score]
            D1d[lead_classification_score]
            D1e[customer_type_score]
            D1f[followup_score]
            D1g[avg_call_duration_score]
        end
        
        subgraph D2_sub [Dealer Signals]
            D2a[stock_availability_score]
            D2b[market_competition_score]
            D2c[dealer_performance_score]
        end
    end
    
    %% App Features
    subgraph H_sub [Mobile App Features]
        H1[Top 20 Leads]
        H2[Next Best Customer]
        H3[Dealer Prioritization]
        H4[Daily Action Items]
    end
    
    subgraph I_sub [Dashboard Insights]
        I1[Enquiry-wise Top 5]
        I2[Leads by PPL]
        I3[Score Distribution]
        I4[Regional Performance]
    end
    
    %% Catalog Info
    G_details[<b>Catalog:</b> tmdai_hackathon<br/><b>Schema:</b> tmdai_hackathon_team_4<br/><b>Table:</b> scv_customer_score]
    G --> G_details
    
    %% Styling
    classDef dataSource fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef engine fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef output fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef app fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef dashboard fill:#fce4ec,stroke:#880e4f,stroke-width:2px
    
    class A,B,C,E,F dataSource
    class D engine
    class G output
    class H app
    class I dashboard


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

