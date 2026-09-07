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

The idea behind our project started with a simple problem. Gun violence is not limited to illegal weapons. Even legally licensed firearms can be misused or carried into sensitive public areas such as schools, colleges, temples, and political gatherings, which can put public safety at risk.

To address this problem, we proposed a **Smart Geo-Fenced Safety System for Legal Firearms**. The main idea is to automatically identify a firearm when it enters a restricted zone and check whether it is authorized to be there.

For this, each firearm is given a unique RFID tag, while restricted areas are equipped with RFID readers. When the firearm enters the zone, the reader detects the RFID and sends the information to an ESP32-based controller. The controller verifies the firearm and checks its authorization. If it is unauthorized, the system activates the locking mechanism in real time.

We also considered practical situations such as lost or stolen firearms. Their IDs can be deactivated from the database, so the system can recognize them as unauthorized when they are detected again. To make the response faster, authorized IDs are maintained in local memory.

While developing the prototype, we faced challenges such as protecting the electronics from water, dust, vibration, and heat. We explored **Parylene C coating** for protection and also integrated our system with an existing safety mechanism instead of redesigning the complete firearm.

For the prototype, we used an **ESP32, RFID reader, RFID tags, servo motor, and a 3D-printed firearm model**. We also developed a dashboard for monitoring, logging, and managing authorized users. Along with this, we explored **YOLOv8** for surveillance to detect people, recognize faces, identify suspicious activity, and generate alerts.

Overall, our project combines **RFID, edge computing, embedded systems, and AI surveillance** to provide a real-time safety mechanism for restricted areas. The basic flow is **Detect → Verify → Decide → Lock**.
