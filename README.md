# 🚀 VITA-INSURATECH  
### The Future of Autonomous Micro-Insurance Systems

---

## 🧠 Problem Statement
Gig workers in **10-minute delivery platforms (Blinkit, Zepto, Instamart)** operate in highly time-sensitive, hyperlocal environments. External disruptions such as heavy rain, extreme heat, pollution, curfews, and sudden zone closures can immediately halt operations, leading to significant income loss.

Currently, there is **no system to protect their income** against such uncontrollable disruptions. Additionally, existing parametric insurance systems are vulnerable to fraud such as GPS spoofing, fake claims, and coordinated fraud attacks.

---

## 💡 Our Solution
We propose **VITA-INSURATECH**, an AI-powered micro-insurance platform that:

- Detects real-world disruptions dynamically  
- Automatically triggers payouts (zero manual claims)  
- Prevents fraud using multi-layer validation  
- Adapts pricing based on hyperlocal risk  

> “We ensure that only genuinely affected and actively working delivery partners receive compensation.”

---

## 🛵 Target Persona: Quick-Commerce Delivery Partners

- Operate within **2–4 km radius**
- Highly dependent on weather and road conditions  
- Income tied directly to active working time  
- Frequent short-distance deliveries  

---

## 🌍 System Architecture

### 🟢 Layer 1: Dynamic Risk Zoning
- City divided into zones using environmental data (OpenWeather API + signals)
- Zones classified as:
  - 🔴 Red (Extreme Risk)
  - 🟠 Orange
  - 🟡 Yellow
  - 🔵 Blue
  - 🟢 Green  

---

### 🟡 Layer 2: Sub-Zone Geofencing
- Zones further divided into **50–200m micro-geofences**

---

### 🔵 Layer 3: Hybrid Location Validation
- GPS 📍 + Network-based location 📡  
- Threshold-based mismatch detection  
- Adaptive fallback  

---

### 🟣 Layer 4: Motion Intelligence
- Accelerometer + Gyroscope  
- Validates real movement  

---

### ⚫ Layer 5: Behavioral & Fraud Detection
- Claim frequency  
- Time patterns  
- Regional anomalies  

---

### 🟤 Layer 6: Work Activity Validation 🔥
- Detect delivery-like movement  
- Minimum active session  
- Pre-event presence  

---

## 🔄 System Workflow

```mermaid
flowchart TD

A[User Onboarding] --> B[Location + Activity Tracking]
B --> C[Zone Classification]
C --> D[Disruption Detection]

D --> E[Auto Claim Trigger]

E --> F[Multi-Layer Validation]

F --> G1[Location Check]
F --> G2[Motion Check]
F --> G3[Behavior Check]
F --> G4[Work Activity Check]

G1 --> H[Risk Scoring]
G2 --> H
G3 --> H
G4 --> H

H --> I{Risk Level}

I -->|Low| J[Instant Payout]
I -->|Medium| K[Verification]
I -->|High| L[Reject / Investigate]
