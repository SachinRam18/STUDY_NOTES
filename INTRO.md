# INTERVIEW : SELF-INTRO

Good morning sir/ma’am. Thank you for giving me this opportunity to introduce myself.

I’m Sachin Ram ES, I completed my schooling in Chennai, and currently I’m pursuing my B.Tech in Computer Science and Business Systems at PSG iTech, Coimbatore.

During my college journey, I’ve explored different areas of technology and worked mainly with Python, Java, and Flutter. I’ve also done internships where I worked on an Education Management System and an AI-based Customer Support Agent using RAG and agentic workflows. 

I’m still learning and improving and would consider myself at a growing stage.

Apart from that, I’ve worked on projects like a Smart Geo-Fenced Firearm Safety System, where I explored hardware and software integration, and AIxAI, an AI-powered negotiation system where AI agents negotiate with each other.

Personally, I believe in accepting things as they come, staying positive, and continuing to move forward and improve from every experience.

Going forward, I want to build a good career in the software field, keep learning, and contribute to the organization I work with.

That’s a brief introduction about me. Thank you.




# SMART GEO-FENCED FIREARM SYSTEM

## 1. Introduction & Problem

The Smart Geo-Fenced Firearm Safety System is a project we developed to prevent the misuse of licensed firearms in restricted areas such as schools, airports, political events, and crowded places. The main idea was to automatically identify a firearm and control its safety based on the location and authorization.

## 2. Proposed Solution & Flow

Each firearm is given a unique RFID identity, and restricted areas have RFID readers. When a firearm enters a restricted area, the reader detects its ID and sends it to an edge controller. The controller verifies the firearm and checks whether the user is authorized. Based on the decision, the microcontroller can lock or unlock the safety mechanism.

**Detect → Verify → Decide → Lock**

## 3. Authorization & Security

The system differentiates between authorized and unauthorized firearms. For example, authorized police firearms can remain active, while unauthorized firearms can be locked. Each firearm has a unique encrypted ID, and the system verifies the information before taking any action.

## 4. Lost or Stolen Firearm

If a firearm is lost or stolen, the administrator can deactivate its ID from the database. It then becomes unauthorized and can be automatically locked when detected inside a protected area.

## 5. Engineering Challenges

One challenge was protecting the electronics from water, dust, vibration, and heat. We explored **Parylene C coating** for protection. Another challenge was integrating the system with an existing firearm design, so we connected our system to the existing safety mechanism.

## 6. Fast Identification

Since the system needs to respond quickly, authorized firearm IDs were maintained in local memory. This allowed faster verification without completely depending on a remote system.

## 7. Prototype & Dashboard

For the prototype, we used an **ESP32, RFID reader, RFID tags, servo motor, and a 3D-printed firearm model**. We also developed a dashboard for monitoring activity, maintaining logs, and managing authorized users and firearms.

## 8. AI Surveillance

We also explored an AI-based surveillance component using **YOLOv8**, which can detect people, recognize faces, identify suspicious activity, and generate alerts.

## 9. Conclusion

Overall, the project combines **RFID, edge computing, embedded systems, AI surveillance, and automatic safety control**. The core idea is to detect the firearm, verify it, make an authorization decision, and activate the safety mechanism when required.
