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

[svg](https://github.com/SachinRam18/STUDY_NOTES/blob/main/SMART_GEOFENCED_FIREARM.md#smart-geo-fenced-firearm-system)

The Smart Geo-Fenced Firearm Safety System is a project we developed to prevent the misuse of licensed firearms in restricted areas. Even legally owned firearms can be misused or accidentally taken into sensitive locations such as schools, airports, political events, or crowded public places. Our goal was to build a system that could automatically identify a firearm and control its safety mechanism based on the location and authorization.

The basic idea of our system is to give every firearm a unique RFID identity and place RFID readers at restricted locations. When a firearm enters such an area, the RFID reader detects its ID and sends the information to an edge controller. The controller verifies the firearm and checks whether the user is authorized. Based on this decision, the microcontroller can activate or lock the firearm's safety mechanism. So, the overall flow is **Detect → Verify → Decide → Lock**.

One important part of the system is distinguishing between authorized and unauthorized firearms. For example, authorized police firearms can remain active, while unauthorized civilian firearms can be locked inside restricted areas. Each firearm has a unique encrypted ID, which helps the system identify it securely.

Security was also an important consideration in our design. The RFID information is encrypted, only authorized readers can access the required information, and the controller verifies the received data before taking any action. The firearm also verifies the command before responding to it. This gives us a layered security approach where the system verifies information at every level.

We also considered the situation where a firearm is lost or stolen. In that case, the administrator can remove or deactivate its ID from the authorized database. The firearm then becomes unauthorized, and when it is detected inside a protected area, the system can automatically activate the locking mechanism.

During development, we faced several engineering challenges. One challenge was protecting the electronic components from water, dust, vibration, and heat. To address this, we explored the use of **Parylene C coating**, which provides protection against moisture and chemicals while still allowing RFID signals to pass through. Another challenge was integrating the system with an existing firearm design. Instead of designing a completely new firearm mechanism, we connected our system to the existing safety mechanism.

Fast identification was another important requirement because the system needs to make the authorization decision quickly. For this, we maintained authorized firearm IDs in local memory so that the verification process could happen with low delay without depending completely on a remote system.

For the prototype, we used an **ESP32 as the edge controller, an RFID reader, RFID tags, a servo motor to demonstrate the locking mechanism, and a 3D-printed firearm model**. We also developed a dashboard for monitoring system activity, maintaining logs, and managing authorized users and firearms.

Along with the hardware system, we also explored an AI-based surveillance component using **YOLOv8**. It can be used for detecting people, recognizing faces, identifying suspicious activity, and generating alerts. This adds an additional layer of monitoring to the overall safety system.

Overall, the project combines **RFID technology, edge computing, embedded systems, AI-based surveillance, and automatic safety control** into one system. The main concept is simple: **detect the firearm, verify its identity, make an authorization decision, and activate the safety mechanism when required.**
