# VITA-INSURATECH
### Zero-Touch Parametric Micro-Insurance for India's Q-Commerce Delivery Workers

> "We don't wait for a claim. We already know."

---

## The Problem Nobody Is Solving

Every morning, thousands of Zepto and Blinkit delivery workers across Bengaluru, Mumbai, and Chennai check the sky before they check their phones. Because if it is raining hard enough, they do not work. And if they do not work, they do not eat. There is no backup. No safety net. No system that says — "we know you could not work today, here is what you lost."

That is not a gap in the market. That is a failure of the entire insurance industry to acknowledge that over 15 million platform gig workers exist and operate without any income protection.

The numbers are not abstract. A Q-Commerce delivery worker in a flood-prone urban zone loses an average of 20–30% of monthly income during disruption periods..

Existing insurance products fail this demographic structurally:

- They require manual claim filing. A worker mid-disruption is not opening a form.
- They are priced monthly. Gig workers earn weekly. That mismatch alone kills adoption.
- They cover health, life, and vehicles — not lost working hours caused by external events.
- They have no fraud intelligence designed for hyperlocal delivery patterns.
- They are reactive. VITA is not.

---

## What VITA Does

VITA-INSURATECH is a mobile-first, zero-touch parametric micro-insurance platform built exclusively for Q-Commerce delivery workers.

The worker subscribes once. Pays a small weekly premium. When a verified external disruption hits their active delivery zone while they are working, VITA detects it, validates it across six independent layers, and processes an instant payout — without the worker doing anything at all.

No forms. No calls. No waiting.

The payout is not based on what the worker says happened. It is based on what the data confirms happened — verified independently across location, motion, behaviour, and external signals.

---

## Why Q-Commerce Specifically

This is a deliberate architectural decision, not just a product choice.

Q-Commerce workers operate within a 2–4 km radius. This is the property that makes everything in VITA work. A flood in Koramangala is not a flood in Indiranagar. The 200-metre precision of our zone detection only makes sense at this delivery scale.

Food delivery workers cover 8–15 km. Zone boundaries blur. Fraud becomes harder to catch. We start where the geometry works in our favour, and expand to other delivery personas in later phases.

| Property | Detail |
|----------|--------|
| Operating Radius | 2-4 km per delivery zone (avg distance covered by a gig worker)|
| Income Model | Per-delivery, continuous activity dependent |
| Earning Cycle | Weekly |
| Disruption Impact | Immediate income stop within hours |
| Platform API Dependency | None — VITA works independently |

Zepto and Blinkit API integration is a future partnership goal. Every part of VITA works without it.

---

## System Architecture: How It All Fits Together

VITA runs as one continuous pipeline. Here is the full flow from a disruption happening in the real world to a payout hitting the worker's account:

```
EXTERNAL SIGNALS
Weather API, AQI API, News API, Twitter/X API
        |
        v
LAYER 1: DYNAMIC RISK ZONING
City divided into colour-coded disruption zones
        |
        v
LAYER 2: SUB-ZONE MAPPING
Each zone divided into smaller grids using cellular network data
        |
        v
LAYER 3: ONE-CLASS SVM
Checks if the worker's location data looks real or faked
        |
        v
LAYER 4: DBSCAN FRAUD RING DETECTION
Finds suspicious clusters of claims inside the disruption zone
        |
        v
LAYER 5: SENSOR FUSION + 24HR MOVEMENT CHECK
Checks last 24 hours of phone movement data
        |
        v
LAYER 6: WORK ACTIVITY VALIDATION
Confirms the worker was actively delivering before the disruption
        |
        v
ADAPTIVE RISK CORRELATION ENGINE (ARCE)
Cross-checks the disruption zone against historical claims patterns
Raises the proof bar in zones where fraud has been seen before
        |
        v
GRADIENT BOOSTING — FINAL DECISION
Takes all layer outputs and decides: approve, review, or reject
        |
        v
INSTANT PAYOUT / REVIEW QUEUE / REJECTION
```

Each layer passes its findings to the next. The final Gradient Boosting model does not make a guess — it makes a decision based on every signal the pipeline collected.

---

## Layer 1: Dynamic Risk Zoning

**Data sources used:** OpenWeather API, AQICN, OpenAQ, NewsAPI, Twitter/X API

The city is split into regions and each region is given a colour based on how disrupted it is right now. This colour map updates in real time.

| Zone | Meaning | When It Triggers |
|------|---------|-----------------|
| Red | Extreme disruption | Rainfall above 64mm/hr OR official red alert OR AQI above 400 OR confirmed curfew |
| Orange | High disruption | Rainfall above 35mm/hr OR AQI 301–400 OR confirmed strike |
| Yellow | Moderate disruption | Rainfall above 15mm/hr OR AQI 201–300 OR unverified social signal |
| Blue | Low disruption | Minor weather warning, no income impact expected |
| Green | Normal | Clear conditions |

**One important rule:** For weather signals, two separate APIs must agree before a zone changes colour. For social disruptions like curfews or strikes, both NewsAPI and Twitter/X signals must report the same event in the same area within 20 minutes of each other. One source alone is never enough. This stops bad API data or a single viral tweet from triggering real payouts.

---

## Layer 2: Sub-Zone Mapping via Cellular Triangulation

Each colour zone from Layer 1 is divided further into smaller 50–200 metre grids. This is done using the cellular network — the worker's phone connects to multiple cell towers at once, and the overlap of those signals tells us which small grid they are in.

We use cellular data here — not just GPS — because it is a completely separate signal. If someone is faking their GPS, the cellular tower data still tells us where they actually are.

These small grids do two things:

1. Make sure the worker is actually inside the disrupted area — not just nearby
2. Give Layer 4 (DBSCAN) the exact map to search for suspicious claim clusters

**Privacy note:** We ask workers for permission to use cellular location data when they sign up. This data is only used for claim checks and is deleted every 24 hours as required under India's Digital Personal Data Protection Act 2023. Nothing is kept longer than needed.

---

## Layer 3: One-Class SVM — Is This Location Real?

**Model used:** One-Class SVM with RBF kernel

**What it checks:** GPS coordinates, cellular triangulation coordinates, the gap between them, how consistent the signals are, and the worker's normal location patterns

Here is the idea: we train this model on what a real delivery worker's location data looks like. Normal GPS and cellular signals agree closely. Movement stays within the delivery radius. Signal quality is consistent.

When a claim comes in, the model checks whether the location data matches that normal pattern. If it does not — it flags the claim.

This is a one-class model, not a two-class one. That is an important distinction. A two-class model needs examples of fraud to learn from. A one-class model only needs examples of normal behaviour. Since we pre-train on synthetic data before launch, it works from Day 1 without needing any real fraud history.

**How it catches GPS faking:** Apps that fake GPS can generate convincing coordinates, but they cannot fake consistent cellular tower data and GPS data at the same time. If the GPS says one location and the cellular signal says another location more than 150 metres away, that is a red flag passed to the next layers.

---

## Layer 4: DBSCAN — Finding Fraud Rings

**Model used:** DBSCAN (Density-Based Spatial Clustering of Applications with Noise)

**The map it works on:** The colour zone from Layer 1 is the area. Every worker claiming inside that zone during the disruption window is a point on that map. Their GPS and cellular coordinates are the coordinates of that point.

DBSCAN looks at all those points and finds clusters. Real disruptions look like clusters of workers spread naturally across the disrupted zone, claiming at different times as the event unfolds. Fraud rings look different — either suspiciously uniform spacing, claims hitting at almost exactly the same time, or single isolated points with no neighbours claiming anything nearby.

DBSCAN is the right tool here for one specific reason: it does not need to be told how many clusters to look for. It figures that out itself. It also naturally labels points that do not fit any cluster as outliers — and in this context, an outlier claim with no corroborating neighbours in the same zone is a strong fraud signal.

Every claim gets a cluster label. Outliers and suspicious cluster patterns get a fraud risk score that goes into the final decision model.

---

## Layer 5: Sensor Fusion + 24-Hour Movement Check

**Sensors used:** Accelerometer, gyroscope, GPS, cellular triangulation

**How long data is kept:** 24 hours. All raw sensor data is deleted after this. Only the final movement score is kept. This is required under India's DPDP Act 2023.

The VITA app quietly tracks movement in the background while it is open. This is not surveillance — it is the minimum data needed to confirm a worker was genuinely out delivering before a disruption hit.

**What gets checked:**

- Does the phone movement match a two-wheeler delivery pattern? The vibration, the stop-start rhythm, the speed — all of it leaves a signature.
- Are there proper delivery stops? A worker picking up and dropping orders stops briefly at multiple points. That pattern is detectable.
- Is the movement path consistent with delivery routes inside the worker's zone?
- Is the speed reasonable for a delivery bike? Not standing still. Not going 80kmph.

A phone sitting on a table with a GPS faker running on it has no movement. The accelerometer is flat. The gyroscope shows nothing. Movement data cannot be faked without actually moving.

Workers are told when data collection is running. Raw data is processed into a single movement score where possible on the device itself, so the raw data never even leaves the phone.

---

## Layer 6: Work Activity Validation

This is the layer that answers the one question that matters most: was this worker actually delivering when the disruption happened?

**What gets checked:**

- **Was the worker active before the disruption?** They must have a strong delivery activity score for at least 45 minutes before the disruption trigger fires. Just being in the zone is not enough. Actively working in the zone is.
- **Does their route look like a real delivery route?** Multiple stop points, movement between them consistent with picking up and dropping orders, session length consistent with an actual work shift.
- **Is the movement-to-rest ratio normal?** Real delivery sessions have a natural rhythm of riding and stopping. Something that looks like continuous movement with no stops, or sitting still the whole time, gets flagged.
- **How quickly did they claim?** The time between the disruption trigger firing and the claim being submitted is measured. Claims that come in suspiciously fast — as if the button was pre-staged — or suspiciously late — as if the person found out after the fact — are scored accordingly.

Everything this layer finds, combined with everything the previous layers found, gets packaged into a single feature set and handed to the final decision model.

---

## Gradient Boosting: The Final Decision

**Model used:** XGBoost

This is where everything comes together. XGBoost takes the output of every layer — all the scores, flags, cluster labels, and risk signals — and makes a single decision on the claim.

**Inputs it receives:**

| Source | What It Sends |
|--------|--------------|
| Layer 1 | Zone colour, disruption severity, whether two sources confirmed it |
| Layer 2 | How much the worker's sub-zone overlaps with the disrupted area |
| Layer 3 | Anomaly score, gap between GPS and cellular location |
| Layer 4 | Cluster label, whether the claim is an outlier |
| Layer 5 | Movement score, session duration, route consistency |
| Layer 6 | Pre-disruption activity score, route pattern score, claim timing score |
| Worker profile | Zone history, how often they claim, how long they have been subscribed, approximate earnings |
| ARCE output | Whether this zone has a history of suspicious claims |

**How the decision works:**

| Score | Decision | What Happens |
|-------|----------|-------------|
| Above 0.72 | Approved | Payout sent instantly |
| 0.55 to 0.72 | Review | Sent to manual review with all reason codes |
| Below 0.55 | Rejected | Rejected with logged reason codes for audit |

XGBoost handles this well because real fraud signals are rarely linear. A slightly suspicious GPS combined with a slightly off movement pattern combined with a borderline activity score — none of those alone crosses a threshold, but together they should. Gradient Boosting catches those combined patterns. It also tells us which features mattered most in each decision, which is important for insurance compliance and audit trails.

---

## Adaptive Risk Correlation Engine (ARCE)

ARCE is how VITA gets smarter over time and stops fraud before it starts.

Here is the core idea. Layer 1 classifies zones by disruption level: Red, Orange, Yellow, Blue, Green. ARCE separately classifies those same zones by their claims history — how many claims came from each zone during past disruptions. Then it cross-checks the two.

If a Red disruption zone produces Red-level claims — that makes sense. Consistent. Low suspicion.

If a Red disruption zone is producing hardly any claims — or a Green zone with no disruption is producing a flood of claims — that is the signal ARCE is designed to catch.

| Disruption Zone | Claims Zone | ARCE Reading | What Changes |
|----------------|-------------|--------------|--------------|
| Red | Red | Low risk — consistent | Standard checks |
| Red | Orange / Yellow | Medium risk — partial mismatch | Extra proof required |
| Red | Blue / Green | High risk — major mismatch | Maximum proof thresholds |
| Green | Red | Critical — no disruption, high claims | Fraud investigation triggered |

When ARCE flags a zone as High or Critical, the proof bar automatically rises for everyone claiming in that zone next time — tighter location checks, stricter movement requirements, lower tolerance for outliers in DBSCAN. The system learns which zones attract fraud and makes itself harder to game there.

ARCE's zone risk score also feeds directly into the Gradient Boosting model as an extra input, so the final claim decision already accounts for whether the zone has a bad history.

---

## Weekly Premium Model

Gig workers earn week to week. The premium model matches that exactly.

### Pricing Tiers

| Risk Zone | Weekly Premium | Maximum Weekly Payout | Coverage Multiplier |
|-----------|--------------|----------------------|---------------------|
| Green | Rs. 29 | Rs. 800 | 27.6x |
| Yellow | Rs. 39 | Rs. 1,200 | 30.8x |
| Orange | Rs. 49 | Rs. 1,800 | 36.7x |
| Red | Rs. 59 | Rs. 2,500 | 42.4x |

A worker's zone is recalculated every 7 days based on where they actually worked that week, how often that area gets disrupted, and what the XGBoost risk model scores them. If the zone improves, the premium drops automatically the next week.

### What Affects the Premium

Four things go into calculating the right weekly premium for each worker:

1. **Zone disruption frequency** — how often the worker's area has been disrupted in the last 90 days
2. **Worker age** — standard actuarial age-based risk adjustment
3. **Approximate earnings** — estimated from their active session data, used to set a payout cap that reflects their actual income exposure
4. **Claim history** — clean record means gradual discounts over time; high fraud scores mean premium adjustments

### Stopping Selective Subscriptions

Workers who only subscribe during monsoon and cancel in summer would drain the pool. Three things prevent this:

1. Monsoon-period premiums in flood-prone zones are priced to reflect the actual risk, making it financially unattractive compared to subscribing year-round
2. Minimum 4-week subscription required — no subscribe-claim-cancel cycles
3. Claim-to-premium ratio is tracked — suspicious ratios are flagged before the next renewal

### What Happens to the Premiums

The money collected as premiums is not just sitting in a bank account. It is put into liquid overnight mutual funds and short-term debt instruments — safe assets that can be converted to cash within 24 hours when claims need to be paid, while still earning stable returns in the meantime.

Those returns do three things: help cover premiums for workers in high-risk zones, keep a reserve buffer of at least 3x the expected weekly claims, and gradually reduce the cost of coverage for everyone as the pool grows.

---

## Platform: Mobile — Not a Choice, a Requirement

Layers 2, 3, and 5 depend on hardware that simply does not exist reliably in a web browser. Mobile is the only platform where VITA's fraud detection can actually run.

| What We Need | Web Browser | Mobile App |
|-------------|------------|-----------|
| Accelerometer for motion detection | Unreliable | Full access |
| Gyroscope | Not available | Full access |
| Precise GPS | Browser dependent | Full access |
| Background data collection | Not possible | Supported |
| Offline data saving | Not possible | Supported |
| Cellular triangulation | Not available | Full access via device APIs |

Without mobile, the fraud detection architecture does not work. That is why we built for mobile.

**Framework:** React Native in TypeScript — one codebase for both iOS and Android, with full access to all the native sensors the fraud detection layers depend on.

---

## Payout System: How Workers Get Paid

The moment a claim is approved by the Gradient Boosting model, the payout pipeline fires automatically. The worker does not tap anything. The money moves on its own.

### How the Payout Flow Works

```
CLAIM APPROVED (score above 0.72)
        |
        v
PAYOUT ENGINE TRIGGERED
Payout amount calculated based on worker's zone tier and session duration
        |
        v
PAYMENT PROCESSOR
Razorpay Test Mode (prototype) / Razorpay Live (production)
        |
        v
UPI TRANSFER
Worker's registered UPI ID receives the payout instantly
        |
        v
PUSH NOTIFICATION
Worker is notified on the app with payout amount and reason
```


### Payment APIs

**Razorpay (Test Mode for prototype, Live for production)**

Razorpay is the primary payment processor. In the prototype, Razorpay Test Mode is used — it simulates the full real-money payment flow including UPI transfers, webhook callbacks, and transaction receipts, without moving actual money. When the platform goes live, switching to Razorpay Live requires only a credential swap, no code changes.

Razorpay is chosen specifically because it has strong UPI support, is widely used in Indian fintech, supports automated payouts via its Payout API (not just collections), and has a well-documented sandbox that lets us demonstrate the full end-to-end flow in the prototype.

**UPI Simulator**

For the prototype demo, a UPI simulator is layered on top of Razorpay's test environment to visually demonstrate the worker receiving money on their UPI ID (e.g., worker@upi). This makes the demo concrete — judges see the notification, the UPI reference number, and the credited amount — exactly as a real worker would experience it.

**Why UPI and not a bank transfer or wallet?**

Q-Commerce delivery workers almost universally have a UPI ID tied to their phone number. They do not always have a bank account with net banking enabled. UPI works with any bank account, works offline via *99#, and settles instantly. For this demographic, UPI is the only payment channel that makes sense.

### Payout Amounts

Payouts are calculated based on the worker's active zone tier and the duration of the disruption window they were working in:

| Zone | Maximum Weekly Payout |
|------|-----------------------|
| Green | Rs. 800 |
| Yellow | Rs. 1,200 |
| Orange | Rs. 1,800 |
| Red | Rs. 2,500 |

If a disruption lasts 3 hours and the worker was active for all 3, they receive the full proportional payout for their zone. Partial disruption windows are prorated. The maximum is capped at the weekly payout limit for their tier regardless of how many disruptions occur in a single week.

### Premium Collection

Workers pay their weekly premium through the same Razorpay integration — UPI autopay or a manual weekly UPI payment. The subscription renewal reminder is sent as a push notification every 7 days. If a worker's premium payment fails, their coverage pauses until payment is received. No coverage, no claims.

---

## Tech Stack

| Layer | Technology | Why We Chose It |
|-------|------------|----------------|
| Mobile app | React Native (TypeScript) | One codebase, iOS and Android, full native sensor access |
| Backend | Node.js + Express | Handles many real-time zone monitoring events at once without slowing down |
| Main database | PostgreSQL | Financial records need to be reliable and auditable — PostgreSQL guarantees that |
| Fast cache | Redis | Zone states and active worker sessions need to be read in milliseconds |
| ML models | Python — scikit-learn, XGBoost | Standard tools for One-Class SVM, DBSCAN, and Gradient Boosting |
| Weather data | OpenWeather API (OneCall 3.0) | Hyperlocal coordinates, minute-level rainfall data |
| AQI data | AQICN + OpenAQ | Two independent AQI sources for cross-checking, good Indian city coverage |
| News and social | NewsAPI + Twitter API v2 | Location-filtered streams for catching curfew and strike signals |
| Payments | Razorpay Test Mode + UPI Sandbox | Realistic payout simulation for the demo, production-ready when live |
| Login | Firebase Auth | Phone number OTP login — no email address needed, which works better for this user base |
| Hosting | AWS EC2 + S3 | Scales up automatically during large disruption events; S3 stores ML model versions |
| Monitoring | AWS CloudWatch | Real-time visibility into pipeline health and claim processing |

---

## Cold Start: How It Works Before We Have Any Real Data

Every new worker has zero history. Fraud models that learn only from real users fail completely on Day 1. VITA solves this with synthetic dataset training of our ML models.

Before a single user signs up, the One-Class SVM, DBSCAN, and Gradient Boosting models are trained on synthetic datasets built to match real Q-Commerce delivery behaviour — the kind of motion a delivery bike makes, how long sessions typically last, what normal GPS and cellular agreement looks like, how routes through Bengaluru or Mumbai delivery zones are shaped.Also, training the models are quite easy as they don't deal with human data rather data from API so rapid prototyping is highly possible and scalable to real-life.

New workers start on this default profile. Over their first four weeks, as real data comes in, their personal profile gradually takes over from the synthetic one. The transition happens automatically.

This approach — training on synthetic data before launch — is how real insurance ML systems handle the cold start problem. It is the only way to have fraud detection working reliably before you have enough real users to learn from.

---

## Deliverable Roadmap

### Phase 1 — Ideation and Foundation (Current)
- [x] Full system architecture designed
- [x] Six-layer fraud detection pipeline defined
- [x] ML model choices made and justified
- [x] ARCE cross-correlation mechanism designed
- [x] Weekly premium model built out with dynamic pricing
- [x] Parametric trigger thresholds set with data sources
- [x] DPDP Act compliance approach defined
- [ ] GitHub repository base setup
- [ ] Core data models and API schema
- [ ] Synthetic dataset generation started
- [ ] 2-minute demo video
      
### Phase 2 — Automation and Protection (March 21 – April 4)
- [ ] Worker onboarding and registration
- [ ] Insurance policy management
- [ ] XGBoost premium calculation engine
- [ ] Four parametric trigger integrations
- [ ] Claims management with six-layer validation pipeline v1

### Phase 3 — Scale and Optimise (April 5 – 17)
- [ ] Full fraud detection stack running (One-Class SVM, DBSCAN, ARCE)
- [ ] Simulated instant payout via Razorpay test mode
- [ ] Worker dashboard: earnings protected, active weekly coverage
- [ ] Admin dashboard: loss ratios, next-week disruption predictions
- [ ] Final pitch deck in PDF
- [ ] 5-minute demo video with live disruption simulation

### Future Plans
- Zepto and Blinkit platform API partnership for richer activity data
- Expansion to food delivery and e-commerce delivery workers
- IRDAI regulatory compliance
- Regional language support: Kannada, Hindi, Telugu, Tamil
- B2B licensing to platform companies

---

## Why VITA Wins

VITA is a fraud-resistant, self-sustaining insurance engine. Six layers of validation make fraud expensive to attempt. Models work from Day 1 without needing historical data. The premium pool is financially self-sustaining through float returns. Zone detection is precise enough to tell the difference between two streets 200 metres apart. And ARCE means the system gets harder to game over time, not easier.

The worker does not need to understand any of this. They pay Rs. 39 a week, put their phone in their pocket, and go to work. When a flood hits their zone, their UPI account gets credited before they have even stopped to take shelter.


---

## Team

VITA-INSURATECH
Amrita Vishwa Vidyapeetham, Bengaluru
Department of Robotics and Artificial Intelligence

*[The tech Incubaters]*
-KS Viswa
-J Anwita Reddy
-Eshaan Sai



