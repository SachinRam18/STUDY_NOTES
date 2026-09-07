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

**FIRST**
The first challenge was **protecting the electronics** from water, heat, shock, and vibration.

We explored **Parylene C coating**, which protects the components while having minimal effect on RF signals.

**SECOND**
The second challenge was **automatic real-time locking**.

We proposed connecting the existing safety mechanism with an automatic locking system controlled by the microcontroller.

**THIRD**
The third challenge was **identifying police and civilian firearms**.

For this, the edge controller maintains an authorized database, with a fixed prefix helping to identify the category quickly.

**FOURTH**
Another challenge was **security**.

The crypto tag responds only to a valid authorized reader, and the controller verifies the communication before taking action.

We also considered **lost or stolen police firearms**. Their IDs can be removed from the authorized database through the dashboard, so they can be treated as unauthorized.

## Electronics Placement

For compact electronics, we consulted **Makers Hub Limited, Coimbatore**, regarding custom microcontroller boards suitable for small spaces.

## Prototype

For our prototype, we used an **ESP32, RFID reader, two RFID tags, a servo motor, and a 3D-printed firearm replica**.

One tag represents an unauthorized firearm and activates the lock, while the other represents an authorized police firearm.

The servo motor simply **simulates the locking mechanism**.

## 3D-Printed Replica

We designed the replica using **Autodesk Fusion 360** and printed it using **PLA on an Ender 3 V2**.

It is a **non-functional replica** used only to demonstrate the placement of the electronics and locking mechanism.

## Dashboard

We also developed a dashboard to **monitor firearm status, maintain logs, and manage authorized IDs**.

## AI-Powered Security Surveillance

As an additional security layer, we developed an **AI surveillance system using YOLOv8 and OpenCV**.

It can detect registered or flagged individuals through a camera and display their **name and ID**, along with an audio alert.

We trained the model using **more than 6,000 images**.

## Conclusion

Overall, our project combines **RFID, embedded systems, edge computing, automatic safety locking, and AI surveillance**.

The goal is to provide an **additional layer of safety in restricted areas** through automatic identification, authorization, and monitoring.

**Thank you.**
