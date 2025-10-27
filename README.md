# 🎟️ Venezia Referral App

A **Remix-based Shopify app** built for **Venezia Photo**, designed to automate and manage a complete referral system for workshop purchases.  
It connects directly to the **Venezia Photo Shopify store**, providing the team with a dashboard, reward logic, analytics, and referral tracking.

---

## 🧭 Core Concept

The Venezia Referral App introduces a structured referral flow inside the Venezia Photo e-commerce ecosystem:

1. **Referral creation** → After a purchase, a customer automatically receives a **unique referral code**.  
2. **Referral usage** → A new customer can use this code to receive a **discount** (e.g. 10 % off).  
3. **Reward system** → The referrer earns a **cashback (e.g. €20)** for every new referred purchase.  
4. **Dashboard** → Venezia admins can view referrers, referrals, and transactions in real time.  
5. **Legacy customers** → Admins can manually create one-time promo codes.  
6. **Analytics** → The system tracks usage, ROI, and detects potential abuse.

---

## 🧩 Functional Flow

### 1️⃣ Purchase Event
- Customer buys a workshop on Shopify.  
- The Shopify webhook `ORDER_PAID` triggers the app.  
- If the customer is new → generate a **referral code** and store it.  
- If the order contains a valid referral code → create a reward for the referrer.

### 2️⃣ Referral Code Usage
- Each referral code can be used **once per new customer**, but **many times in total**.  
- Shopify applies the discount through the app (Admin GraphQL API).

### 3️⃣ Cashback Rewards
- Each valid referral → creates a **Reward record**.  
- Cashback amount is configurable (default : €20).  
- Rewards can be **marked as paid** manually or via automation.

### 4️⃣ Admin Dashboard
Admins can:
- See all referrers and rewards.  
- Generate promo codes for legacy users.  
- Adjust cashback / discount settings.  
- View analytics and mark payouts.

---

## 🧱 System Architecture Overview

```mermaid
graph TD
    A[Shopify_Store_Venezia_Photo] -->|Webhook_ORDER_PAID| B[Venezia_Referral_App_Remix]
    B --> C[Referral_Service]
    B --> D[Reward_Service]
    B --> E[Promo_Code_Service]
    B --> F[Analytics_Engine]
    B --> G[PostgreSQL_Railway]
    B --> H[Shopify_Admin_GraphQL_API]
    C --> G
    D --> G
    E --> H
    F --> G
    F --> I[Dashboard_UI]
