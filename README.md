# Media & Entertainment Analytics Portal (Power BI)

An interactive Power BI report that analyses **50,000 OTT streaming subscriber records** to track user growth, content engagement, subscription monetization, device ecosystems, and churn behavior.

---

## Project Overview

OTT streaming platforms generate high volumes of viewing, subscription, and user-activity data, making it difficult to pinpoint why subscribers leave and which content actually retains them. This dashboard brings the data into a single unified report so product, content, and retention managers can:
- track overall user base, revenue, ARPU, and churn rate at a glance
- evaluate content engagement using Watch Time, Completion Rate, and the composite **Content Performance Index (CPI)**
- compare subscription tier performance and payment gateway adoption across devices
- identify inactivity thresholds (`days_since_last_login`) driving subscriber churn
- drill down into specific countries, content titles/genres, payment methods, and churned user cohorts

**Author:** Shweta Bharti

---

## Dataset

- **Source:** OTT Media & Entertainment Userbase Dataset *(add dataset/Kaggle link here)*
- **Size:** 50,000 users across 8 genres, 4 primary devices, 4 payment methods, and 3 subscription tiers
- **Structure:** Star-schema data model with user dimension (`Dim_Users`) and engagement/retention fact tables (`Fact_Engagement`, `Fact_Retention_Churn`)
- **Files in this repo:** Raw and cleaned dataset files located in the `data/` folder
- **Groups of columns:** User demographics (`age_group`, `gender`, `country`), subscription & billing (`subscription_type`, `payment_method`, `revenue`), content engagement (`favorite_genre`, `watch_time`, `completion_rate`, `rating`, `recommendation_click_rate`), and retention metrics (`account_age_months`, `days_since_last_login`, `is_churned`)

| Column | Meaning |
| :--- | :--- |
| `subscription_type` | Subscriber plan tier: `Basic`, `Standard`, `Premium` |
| `primary_device` | Main streaming device: `Mobile`, `Smart TV`, `Laptop`, `Tablet` |
| `payment_method` | Billing gateway used: `PayPal`, `Credit Card`, `UPI`, `Debit Card` |
| `is_churned` | Subscriber status: `1` / `Yes` (Churned), `0` / `No` (Active/Retained) |
| `days_since_last_login` | Number of inactive days since the user's last platform session |

---

## Dashboard Pages

| # | Page | What it shows |
| :--- | :--- | :--- |
| 1 | **Global Market Overview** | Executive KPI cards (50.0K Users, $616.17K Revenue, 41 Avg Age, 19.93% Churn), age-group donut, global revenue map, genre & device splits, and synced slicers. |
| 2 | **Content Performance & Audience Engagement** | Avg Rating (3.00), Completion Rate (64.53%), Watch Time (154.95 mins), CPI Score (46.52), and scatter/bubble quadrant analysis by genre and device. |
| 3 | **Device Ecosystem & Subscription Analytics** | ARPU ($12.32), Total Churned (10K), Avg Devices Used (2.00), payment gateway donut, and a tier × device user/revenue matrix. |
| 4 | **Retention, Dormancy & Churn Deep-Dive** | Retained vs. churned split by subscription tier and a combo chart tracking churn volume against `account_age_months` and `days_since_last_login`. |
| 5 | **Drill-Through Deep-Dive Pages** | Dedicated hidden target pages for granular record-level inspection by Country, Content Title/Genre, Subscription/Payment Method, and Churned User Cohorts. |

---

## Key Findings

- **Overall Scale & Revenue:** The platform serves **50,000 users** generating **$616,174.77** in total revenue at an **ARPU of $12.32** and an average user age of **41 years**.
- **Churn & Inactivity Threshold:** Overall churn stands at **19.93% (10,000 users)**, with average inactivity at **29.41 days** and average account age at **29.87 months**. Churn spikes sharply when `days_since_last_login` reaches **28–30 days** (peaking at 203 churned users in month 19), signaling a **21-day inactivity trigger** for re-engagement campaigns.
- **Subscription Tier Monetization:** **Standard** is the largest tier (**19,931 users**, **$245.83K** revenue, 20.09% churn), followed by **Premium** (**15,196 users**, **$187.13K** revenue, 20.39% churn) and **Basic** (**14,873 users**, **$183.22K** revenue, 19.24% churn).
- **Content Engagement:** Average watch time is consistent at **~2.6 hours** per genre (`154.95 mins` overall) with a **64.53%** completion rate and a composite **CPI score of 46.52**. **Documentary** leads both user volume (**6,352 users**) and recommendation click volume (**~315K**).
- **Device & Payment Balance:** Usage is evenly distributed across **Mobile (25.17%)**, **Smart TV (25.12%)**, **Laptop (25.02%)**, and **Tablet (24.69%)**, with an average of **2.00 devices** per user. Payment adoption is similarly balanced across **PayPal (25.08%)**, **Credit Card (25.00%)**, **UPI (24.99%)**, and **Debit Card (24.93%)**.

---

## Tools & Techniques

- **Power BI Desktop:** Multi-page report design, synced slicers, reset-filter bookmarks, combo charts, scatter quadrant analysis, conditional matrix formatting, and cross-page drill-through navigation
- **DAX:** Custom calculated measures for `ARPU`, `Churn Rate %`, `Total Churned`, and `Content Performance Index (CPI)`
- **Microsoft Excel / CSV:** Raw and cleaned data preparation

---

## Sample DAX

```dax
Total Users = DISTINCTCOUNT(Dim_Users[user_id])

Total Revenue = SUM(Fact_Subscriptions[revenue])

ARPU = DIVIDE([Total Revenue], [Total Users], 0)

Total Churned =
CALCULATE(
    COUNTROWS(Dim_Users),
    Fact_Retention_Churn[is_churned] = 1
)

Churn Rate % = DIVIDE([Total Churned], [Total Users], 0)

CPI Score =
AVERAGE(Fact_Engagement[avg_watch_time_hours]) * AVERAGE(Fact_Engagement[rating])v
