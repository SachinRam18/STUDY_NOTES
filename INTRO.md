# INTERVIEW : SELF-INTRO

Good morning sir/ma’am. Thank you for giving me this opportunity to introduce myself.

I’m Sachin Ram ES, I completed my schooling in Chennai, and currently I’m pursuing my B.Tech in Computer Science and Business Systems at PSG iTech, Coimbatore.

During my college journey, I’ve explored different areas of technology and worked mainly with Python, Java, and Flutter. I’ve also done internships where I worked on an Education Management System and an AI-based Customer Support Agent using RAG and agentic workflows. 

I’m still learning and improving and would consider myself at a growing stage.

Apart from that, I’ve worked on projects like a Smart Geo-Fenced Firearm Safety System, where I explored hardware and software integration, and AIxAI, an AI-powered negotiation system where AI agents negotiate with each other.

Personally, I believe in accepting things as they come, staying positive, and continuing to move forward and improve from every experience.

Going forward, I want to build a good career in the software field, keep learning, and contribute to the organization I work with.

That’s a brief introduction about me. Thank you.



# SMART GEO-FENCED SAFETY SYSTEM FOR LICENSED FIREARMS

## Problem

Today, violence is not caused only by **illegal firearms**. Even **licensed firearms** can be misused in public places.

We got this idea after seeing incidents like the **Charlie Kirk shooting** and the **assassination attempt on Donald Trump**.

So, we proposed a system to **detect firearms in restricted areas and automatically lock unauthorized ones**.

## Production Idea

Our production idea is a **smart geo-fencing system for licensed firearms**.

Each firearm has a unique identity. When it enters a restricted zone, the system checks whether it is authorized.

If it is not authorized, the **safety lock is automatically activated**.

This is the basic working of the setup.

## Hardware

The main components are:

- **Passive UHF Crypto Tag** – gives each firearm a unique ID.
- **Reader and Antenna** – detects the firearm in the restricted zone.
- **Edge Controller** – verifies the ID and makes the authorization decision.
- **Microcontroller** – controls the safety mechanism.
- **Dashboard** – monitors status, logs, and authorized IDs.

The basic flow is:

**Firearm enters restricted zone → RFID Tag responds to Reader → Reader sends Tag ID to Edge Controller → Edge Controller verifies the ID and sends the decision to the Microcontroller → Microcontroller activates or releases the Safety Lock.**

## Challenges and Solutions

**FIRST** : The first challenge was **protecting the electronics** from water, heat, shock, and vibration.

We explored **Parylene C coating**, which protects the components while having minimal effect on RF signals.

**SECOND** : The second challenge was **automatic real-time locking**.

We proposed connecting the existing safety mechanism with an automatic locking system controlled by the microcontroller.

**THIRD** : The third challenge was **identifying police and civilian firearms**.

For this, the edge controller maintains an authorized database, with a fixed prefix helping to identify the category quickly.

**FOURTH** : Another challenge was **security**.

The crypto tag responds only to a valid authorized reader, and the controller verifies the communication before taking action.

We also considered **lost or stolen police firearms**. Their IDs can be removed from the authorized database through the dashboard, so they can be treated as unauthorized.

For compact electronics, we consulted **Makers Hub Limited, Coimbatore**, for suitable custom microcontroller boards. 

For the prototype, we used an **ESP32, RFID reader, two RFID tags, a servo motor, and a 3D-printed firearm replica**. One tag represents an unauthorized firearm and activates the lock, while the other represents an authorized firearm. The servo motor **simulates the locking mechanism**. 

# PROJECT 2 – AIxAI

## Problem Statement

In the future, AI agents may automatically renew subscriptions for customers.

These AI agents mainly compare **price, features, and overall value**, so companies may lose loyal customers even when customers are satisfied.

## Proposed Solution

We developed an **AI-to-AI negotiation system**, where the **Business AI** directly negotiates with the **Customer AI**.

The main goal is to **retain customers while maintaining profitability**.

## Phase 1 – Monitor Phase

The Business AI monitors **product usage, feature adoption, customer activity, and renewal dates** to understand customer behavior.

## Phase 2 – Risk Detection Phase

A **churn prediction model** checks whether the customer is likely to leave.

If the churn probability crosses a set threshold, the negotiation process starts automatically.

## Phase 3 – Evaluation Phase

The Customer AI compares the current provider with competitors based on **price, features, security, reliability, switching cost, and loyalty**.

Each factor is given a weight, and an overall **utility score** is calculated.

For example:

**Price – 30% | Features – 25% | Reliability – 15% | Security – 10% | Loyalty – 10% | Switching Cost – 10%**

## Phase 4 – Negotiation Phase

The Business AI generates a personalized offer, and the Customer AI evaluates it.

If the offer is rejected, the Business AI improves it using **discounts, premium features, or additional benefits** until both sides reach an agreement.

## Phase 5 – Decision Phase

The Customer AI finally decides whether to **renew the subscription or move to a competitor**, based on the utility score.

## Phase 6 – Reinforcement Phase

If the customer renews, the system updates the **customer profile, preference history, and loyalty score**.

This helps improve future negotiations.

## Automation

A scheduler automatically starts the complete workflow **before the subscription expires**.

We designed the replica using **Autodesk Fusion 360** and printed it using **PLA on an Ender 3 V2**. We also developed a dashboard to **monitor status, maintain logs, and manage authorized IDs**. 

In addition, we developed an **AI surveillance system using YOLOv8 and OpenCV** to detect registered or flagged individuals and display their **name and ID with an audio alert**. The model was trained using **more than 6,000 images**. Overall, our project combines **RFID, embedded systems, edge computing, automatic locking, and AI surveillance** to improve safety in restricted areas. **Thank you.**
