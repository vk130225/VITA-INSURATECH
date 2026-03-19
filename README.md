# 🚀 VITA-INSURATECH  
### AI-Powered Autonomous Micro-Insurance for Gig Workers

---

## 🧠 Problem Statement

Gig workers in **10-minute delivery platforms (Blinkit, Zepto, Instamart)** operate in highly time-sensitive, hyperlocal environments where even small disruptions can significantly impact income.

External factors such as:
- 🌧️ Heavy rain / flooding  
- ☀️ Extreme heat  
- 🌫️ Pollution  
- 🚫 Curfews / strikes  
- 🚧 Road closures  

can immediately halt operations, causing **20–30% income loss**.

Existing systems:
- ❌ Do not provide income protection  
- ❌ Rely on manual claims  
- ❌ Are vulnerable to fraud (GPS spoofing, fake claims, fraud rings)

---

## 💡 Our Solution

We present **VITA-INSURATECH**, a **zero-touch parametric micro-insurance platform** that:

- Automatically detects disruptions  
- Triggers claims without user input  
- Validates authenticity using multi-layer intelligence  
- Dynamically adapts pricing based on risk  

> 💀 *We don’t trust a single signal — we validate reality.*

---

## 🛵 Target Persona: Quick-Commerce Delivery Workers

We specifically focus on **10-minute delivery ecosystems**, where:

- Workers operate within **2–4 km radius**
- Income depends on **continuous activity**
- Disruptions have **immediate impact**
- Operations are **hyperlocal**

👉 This makes our **micro-geofencing + behavioral validation highly effective**

---

## 🌍 System Architecture Overview

Our system follows a **6-layer verification architecture**

---

### 🟢 Layer 1: Dynamic Risk Zoning

- City divided using **OpenWeather API + disruption signals**
- Zones classified:

| Zone | Meaning |
|------|--------|
| 🔴 Red | Extreme disruption |
| 🟠 Orange | High disruption |
| 🟡 Yellow | Moderate |
| 🔵 Blue | Low |
| 🟢 Green | Minimal |

---

### 🟡 Layer 2: Sub-Zone Geofencing

- Each zone divided into **50–200m micro-grids**
- Ensures:
  - Precise location validation  
  - Hyperlocal disruption detection  

---

### 🔵 Layer 3: Hybrid Location Validation

- Combines:
  - GPS 📍  
  - Network-based location 📡  

- Detects mismatch using thresholds  
- Adaptive fallback in low-signal environments  

---

### 🟣 Layer 4: Motion Intelligence

- Uses:
  - Accelerometer  
  - Gyroscope  

- Validates:
  - Real movement  
  - Delivery-like patterns  

---

### ⚫ Layer 5: Behavioral & Fraud Detection

- Claim frequency analysis  
- Time consistency  
- Regional anomaly detection  

---

### 🟤 Layer 6: Work Activity Validation (Key Innovation 🔥)

Ensures:
> User is **actually working**, not just present

Includes:
- Structured route detection  
- Minimum session duration  
- Pre-event activity validation  

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
