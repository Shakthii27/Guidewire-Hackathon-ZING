# Zing - Parametric Micro-Insurance for Delivery Partners

> **"Weather the Ride."**
> Zing is a mobile-first parametric micro-insurance platform that automatically protects gig delivery workers from income loss caused by extreme weather events - with zero paperwork and instant payouts.

---

## Table of Contents

- [The Problem](#the-problem)
- [Persona & Scenario](#persona--scenario)
- [Application Workflow](#application-workflow)
- [Weekly Premium Model](#weekly-premium-model)
- [Parametric Triggers](#parametric-triggers)
- [Why Mobile](#why-mobile)
- [AI/ML Integration](#aiml-integration)
- [Tech Stack](#tech-stack)
- [Development Plan](#development-plan)
- [Adversarial Defense & Anti-Spoofing Strategy](#adversarial-defense--anti-spoofing-strategy)
- [Why Zing Wins](#why-zing-wins)

---

## The Problem

India has over **15 million gig delivery workers**. They are classified as independent contractors — which means no employer, no sick leave, and no income protection.

Every summer, Delhi hits **46°C+**. Every monsoon, streets flood in minutes. Every winter, AQI crosses 500. On those days, a delivery rider faces an impossible choice:

> **Log off and survive the conditions - or ride through it and survive the month.**

No existing insurance product addresses this. Traditional insurance requires claims, assessors, paperwork, and weeks of waiting. By then, Shakthi has already missed rent.

**Zing solves this with parametric insurance** — payouts triggered automatically by verified environmental data, not by human claims.

---

## Persona & Scenario

### Meet Shakthi - The delivery partner

| Attribute | Detail |
|-----------|--------|
| **Role** | Gold-Tier Zomato delivery partner, New Delhi |
| **Income** | ₹600–800/day, weekly payout cycle |
| **Dependents** | Pays rent + supports family |
| **Device** | Android smartphone, always on the road |
| **Core Fear** | A bad weather week wipes out her monthly margin |

### The Scenario

It is the third week of May 2026. Delhi's Heat Index has been climbing for five days. On Tuesday morning, the Wet-Bulb Globe Temperature in Shakthi's delivery zone crosses **36.2°C** - a level at which outdoor physical labour is medically classified as dangerous.

**Without Zing:** Shakthi rides anyway. She risks heat stroke. Or she logs off and loses ₹240 for the day - money she cannot afford to lose.

**With Zing:** At 11:22 AM, Zing's backend detects the WBGT breach in Shakthi's pin code. It cross-checks her Zomato status (offline), validates her GPS (stationary), runs fraud checks, and at 11:23 AM - **₹240 is credited to her UPI. She never filed a claim.**

---

## Application Workflow
ONBOARDING
- Shakthi downloads Zing → links Zomato Rider ID + UPI
- One-time KYC via Aadhaar OTP

WEEKLY SUBSCRIPTION
- Every Sunday night, ML model calculates next week's premium
- Monday morning: premium auto-deducted from Zing wallet

REAL-TIME MONITORING
- Zing backend continuously polls:
- OpenWeatherMap API (WBGT, rainfall)
- CPCB AQI API (Air Quality Index)
- Google Maps API (zone-level GPS validation)

AUTOMATIC TRIGGER
- If a parametric threshold is breached in Shakthi's pin code:-
- Disruption event is logged
- Shakthi receives push notification: "⚠️ Heatwave detected. Stay safe."
- System begins activity validation

FRAUD VALIDATION (AI Layer)
- Platform Sync Check → Zomato API queried
- GPS Stationarity Check → accelerometer + location
- Ghost Zone Analysis → peer rider behaviour compared
- Earnings Correlation → end-of-week payout cross-checked

ZERO-TOUCH PAYOUT
- All checks pass → instant UPI transfer
- Failed check → claim held, rider notified with reason


---

## Weekly Premium Model

Zing does not charge a fixed monthly premium. Instead, **risk is priced fresh every week** based on the actual forecast for the rider's location.

### How It Works

Every Sunday, Zing's ML model runs a **7-day disruption probability forecast** for each active pin code. The premium for the coming week is calculated as:

Weekly Premium = Base Rate × Risk Multiplier × Tier Adjustment


| Weather Forecast | Risk Multiplier | Example Premium (Gold Tier) |
|---|---|---|
| Clear / Low Risk | 0.75× | ₹15 |
| Moderate Risk | 1.0× | ₹20 |
| Peak Summer / Monsoon | 1.25× | ₹25 |
| Extreme Risk Week | 1.5× | ₹30 |

### Why Weekly?

- Riders' income is **weekly** - their insurance cost should match their cash flow cycle
- A fixed monthly premium is unaffordable in a bad-income month
- Weekly pricing lets the model **respond to actual seasonal risk** rather than averaging it out
- Riders can see next week's premium every Sunday and plan accordingly

### Coverage Cap

Each tier has a weekly payout cap:

| Tier | Weekly Coverage Cap | Hourly Rate |
|---|---|---|
| Silver | ₹800 | ₹60/hr |
| Gold | ₹1,200 | ₹80/hr |
| Platinum | ₹1,800 | ₹100/hr |

---

## Parametric Triggers

A parametric trigger is a **pre-defined, objectively measurable threshold** that automatically initiates the payout process. There is no subjective assessment - if the data says the threshold was crossed, the event is logged.

Zing uses three triggers:

### 1. Thermal Stress (Heatwave)

| Parameter | Threshold |
|---|---|
| Metric | Wet-Bulb Globe Temperature (WBGT) |
| Trigger | WBGT ≥ 35°C sustained for ≥ 3 hours |
| Data Source | OpenWeatherMap API + IMD |
| Granularity | Pin-code level |

> WBGT is the internationally recognised standard for assessing heat stress on outdoor workers (ISO 7243). It accounts for temperature, humidity, wind speed, and solar radiation — far more accurate than dry-bulb temperature alone.

---

### 2. Air Quality (Pollution)

| Parameter | Threshold |
|---|---|
| Metric | Air Quality Index (AQI) |
| Trigger | AQI ≥ 450 ("Severe") sustained for ≥ 4 hours |
| Data Source | CPCB (Central Pollution Control Board) API |
| Granularity | Station-level, mapped to pin codes |

> At AQI 450+, CPCB classifies outdoor exposure as hazardous. Continuous delivery riding at this level causes documented respiratory harm.

---

### 3. Hyper-Local Flooding

| Parameter | Threshold |
|---|---|
| Metric | Rainfall intensity |
| Trigger | ≥ 50mm rainfall in any 2-hour window |
| Data Source | OpenWeatherMap + IMD rainfall API |
| Granularity | Pin-code level |

> 50mm/2hr is the threshold at which urban drainage in Indian metros (Delhi, Mumbai, Chennai) typically fails, causing surface flooding that physically prevents two-wheeler movement.

---

## Why Mobile (Not Web)

| Factor | Mobile | Web |
|---|---|---|
| Rider behaviour | Always on the road, never at a desk | Requires sitting at a computer |
| GPS validation | Native device GPS for real-time zone checks | No reliable geolocation |
| Push notifications | Instant payout alerts while riding | No native push support |
| Platform integration | Background sync with Zomato app | Cannot access device-level data |
| Fraud detection | Accelerometer + GPS cross-validation | No sensor access |
| Accessibility | Works on low-end Android, offline-capable | Requires stable browser + internet |

A web platform would make Zing inaccessible to the exact users it serves. The mobile app is not a preference - it is a product requirement.

---

## AI/ML Integration

### 1. Dynamic Premium Calculation

Model Type: Gradient Boosted Regression (XGBoost)

Training Data:
- 5 years of IMD historical weather data (city + pin code level)
- Historical CPCB AQI records
- Seasonal disruption frequency by geography

Features:
- Rolling 30-day temperature anomaly
- Monsoon onset probability (IMD seasonal forecast)
- Historical disruption frequency for the pin code
- Rider tier and claims history

Output: Probability of ≥1 disruption event in the coming 7 days → converted to a risk multiplier → applied to base premium rate.

---

### 2. Fraud Detection Engine

Model Type: Anomaly Detection (Isolation Forest + Rule-Based Layer)

Zing's fraud engine runs four checks every time a payout is triggered:

- Platform Sync  
- Earnings Correlation  
- Ghost Zone Analysis  
- GPS Spoof Detection  

Trust Score system adjusts premiums based on behaviour.

---

## Tech Stack

| Layer | Technology |
|------|-----------|
| Mobile | Flutter |
| Backend | Node.js, FastAPI |
| Database | MongoDB |
| ML | scikit-learn, XGBoost |
| APIs | OpenWeatherMap, CPCB |
| Maps | Google Maps |
| Payments | Razorpay (sandbox) |
| Notifications | Firebase |
| Hosting | AWS |

---

## Development Plan

- Week 1: Foundation  
- Week 2: Trigger pipeline  
- Week 3: AI/ML layer  
- Week 4: Integration & demo  

---

## Adversarial Defense & Anti-Spoofing Strategy

Zing is designed to operate in adversarial environments where coordinated fraud attempts may occur at scale. A recent simulated threat scenario demonstrated the use of GPS spoofing by organized groups to trigger false payouts. Zing addresses this through a multi-layered defense architecture that goes beyond simple location verification.

### 1. Differentiation: Genuine Rider vs Spoofed Actor

Zing does not rely on GPS alone. Instead, it builds a **behavioral and contextual profile** of each rider during a disruption event.

A genuine stranded rider typically shows:
- Sudden transition from active → inactive state
- Consistent device movement patterns before disruption
- Presence within a disruption zone shared by other riders

A spoofed actor typically shows:
- Static or inconsistent GPS signals without corresponding motion data
- Activity mismatch between Zing and delivery platform (e.g., still completing orders)
- Isolated inactivity while nearby riders continue normal operations

Zing’s AI engine evaluates these patterns in real time to classify events as legitimate or suspicious.


### 2. Data Beyond GPS

To detect coordinated fraud rings, Zing analyzes multiple data layers:

**Device-Level Signals**
- Accelerometer and gyroscope data (movement vs claimed inactivity)
- App foreground/background usage patterns
- Device integrity checks (mock location detection)

**Platform-Level Data**
- Rider activity status from delivery platform APIs (online/offline, active orders)
- Weekly earnings correlation with claimed downtime

**Network & Location Context**
- IP address clustering (multiple riders claiming from same network)
- Cell tower triangulation vs GPS mismatch
- Historical location consistency patterns

**Peer-Based Intelligence**
- Real-time comparison with other riders in the same pin code
- Detection of abnormal clusters of simultaneous claims
- “Ghost zone” detection where few riders claim disruption but majority remain active


### 3. UX Balance: Fairness for Honest Riders

Zing is designed to **minimize false negatives** (rejecting genuine claims) while preventing fraud.

If a claim is flagged:
- Payout is temporarily held, not rejected
- Rider receives a clear notification explaining the reason
- Passive re-validation occurs when:
  - Network stabilizes
  - Device reconnects
  - Additional data confirms legitimacy

For high-trust riders:
- Faster approvals
- Lower friction validation

For flagged patterns:
- Trust Score is adjusted
- Future claims may require additional validation layers

This ensures that honest riders experiencing genuine disruptions (e.g., network drop during storms) are not unfairly penalized.



### Zing’s anti-spoofing strategy combines:
- Behavioral intelligence  
- Multi-source data validation  
- Real-time peer comparison  
- Adaptive trust scoring  

This creates a system that is resilient against coordinated fraud while remaining fair, fast, and frictionless for genuine delivery partners.

--- 

## Why Zing Wins

For the Rider:
- Zero friction  
- Affordable  
- Instant payouts  
- Transparent  

For the Business:
- Fraud-resistant  
- Scalable  
- Data-driven pricing
