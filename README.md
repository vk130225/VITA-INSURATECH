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
### 🟢 Layer 1: Dynamic Risk Zoning
- City divided into zones using environmental data (OpenWeather API + signals)
- Zones classified as:
  - 🔴 Red (Extreme Risk)
  - 🟠 Orange
  - 🟡 Yellow
  - 🔵 Blue
  - 🟢 Green  

➡️ Used for:
- Claim triggering  
- Pricing decisions  

---

### 🟡 Layer 2: Sub-Zone Geofencing
- Zones further divided into **50–200m micro-geofences**
- Enables hyperlocal disruption detection  

---

### 🔵 Layer 3: Hybrid Location Validation
- Combines:
  - GPS 📍  
  - Network-based location 📡  

- Threshold-based mismatch detection  
- Adaptive fallback in low-signal conditions  

---

### 🟣 Layer 4: Motion Intelligence
- Uses:
  - Accelerometer  
  - Gyroscope  

- Validates real physical movement  

➡️ Prevents:
- Static spoofing  
- Fake movement  

---

### ⚫ Layer 5: Behavioral & Fraud Detection
- Claim frequency analysis  
- Time-based patterns  
- Regional anomaly detection  

➡️ Detects:
- Suspicious individuals  
- Coordinated fraud rings  

---

### 🟤 Layer 6: Work Activity Validation (Key Innovation 🔥)

Ensures users are **actually working**:

- Detect structured delivery-like movement  
- Enforce minimum active session duration  
- Require presence before disruption  

#### 📊 Work Activity Score
| Score | Meaning |
|------|--------|
| High | Active worker |
| Medium | Possibly active |
| Low | Likely inactive/fraud |

---

## 🔐 Adversarial Defense & Anti-Spoofing Strategy

We use a **multi-layer defense system**:

- Location validation (GPS + network)  
- Motion verification (sensor fusion)  
- Behavioral anomaly detection  
- Work session validation  
- Fraud ring detection  

> “Fraud is prevented by combining multiple independent signals, making spoofing infeasible.”

---

## 🤖 AI & Risk Modeling

### 🔹 DBSCAN
- Detects clusters of suspicious users  
- Identifies fraud rings  

---

### 🔹 One-Class SVM
- Learns normal delivery behavior  
- Flags anomalies  

---

### 🔹 Gradient Boosting
- Combines:
  - Zone risk  
  - Location mismatch  
  - Motion consistency  
  - Behavior patterns  
  - Work activity  

➡️ Outputs:
- Low Risk  
- Medium Risk  
- High Risk  

---

## 🌍 Region Consistency Validation

We compare:
- **Environmental severity (zones)**  
- **Claim density**

### Logic:
| Scenario | Result |
|--------|--------|
| High weather + high claims | Low Risk |
| Moderate mismatch | Medium Risk |
| Extreme mismatch | High Risk |

---

## ⚡ Parametric Automation (ZERO-TOUCH SYSTEM)

- Real-time disruption detection  
- Automatic claim triggering  
- Instant payout processing  

> “Users do not need to manually file claims — payouts are triggered automatically.”

---

## 🔄 Risk-Based Decision System

| Risk Level | Action |
|----------|--------|
| Low | Instant payout |
| Medium | Delayed verification |
| High | Strict validation / rejection |

---

## 📸 Adaptive Verification Policy

- Low risk → minimal checks  
- Medium risk → occasional verification  
- High risk → strict validation (selfie/proof)  

---

## 💰 Dynamic Weekly Pricing Model

Premiums are adjusted dynamically based on zone risk:

| Zone | Risk | Weekly Premium | Coverage |
|------|------|---------------|----------|
| Red | Very High | ₹50 | ₹1200 |
| Orange | High | ₹40 | ₹1000 |
| Yellow | Medium | ₹30 | ₹800 |
| Blue | Low | ₹20 | ₹500 |
| Green | Minimal | ₹10 | ₹300 |

> “Pricing adapts dynamically to ensure fairness for workers and sustainability for insurers.”

---

## 🚫 Non-Weather Disruption Handling

Since no direct API exists for curfews or strikes, we use:

- News signals  
- Traffic anomalies  
- User activity patterns  

> “We infer disruptions using multi-signal intelligence.”

---

## 👤 User Workflow

1. User onboarding with consent  
2. System tracks location & activity  
3. Zone classification updates in real-time  
4. Disruption detected  
5. Claim auto-triggered  
6. AI evaluates risk  
7. Payout processed  

---

## 🔒 Privacy & Compliance

- Explicit user consent  
- Minimal data collection  
- Data retention limited to **48 hours**  
- Compliant with **India’s DPDP Act**

---

## 🧪 Prototype Plan (Phase 1)

- Simulated zones (OpenWeather API)  
- Basic geofencing  
- Mock AI risk scoring  
- Simulated fraud detection  

---

## 🚀 Future Scope

- SDK integration with delivery platforms  
- Real-time activity verification  
- Advanced ML models  
- Payment gateway integration  
- Analytics dashboards  

---

## 🎯 Key Innovation

> “Our system ensures that a worker is not just present in a disruption zone, but actively working and behaving consistently with real-world delivery patterns.”

---

## 👥 Team
(Add your team members here)

---

## 📌 Repository
(Add your GitHub link)

---
