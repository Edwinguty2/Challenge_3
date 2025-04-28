# Real-Time Flood Monitoring and Alert System for Colombian Rivers (ESP32 MQTT Enhanced Edition)

## Abstract
Colombia’s complex topography and dynamic climate pose significant challenges in predicting and managing flood events, now integrated with MQTT protocol for robust and scalable communication. This repository presents an **enhanced IoT-based solution** that leverages an **ESP32 WROOM32** microcontroller running three concurrent tasks—one dedicated to sensor data processing, another to hosting a local web server and the lastone uses MQTT Protocol to share the sensor data worldwide. By integrating **multiple sensors** (ultrasonic, DHT11, tilt switch, raindrop detector, and a BMP180 barometric sensor over I2C), the system continuously monitors river conditions and environmental parameters.  Sensor data collected locally are published to a Raspberry Pi 5 acting as an MQTT broker, gateway, and database server, subsequently relaying data to Ubidots' global MQTT broker. This setup facilitates both local and worldwide monitoring through dual dashboards, greatly enhancing real-time flood detection capabilities.

---

## Introduction
Frequent river floods across Colombia—amplified by phenomena like La Niña—continue to threaten public safety, disrupt infrastructure, and strain emergency response efforts. Traditional monitoring systems often lack the agility to deliver **timely, localized alerts**, leaving communities vulnerable to sudden surges in water levels.

This enhanced version of the system improves upon the ESP32-based dual-thread design by incorporating MQTT communications via a Raspberry Pi 5 with Mosquitto. It provides seamless integration between local sensor networks, local data storage, and global visualization platforms, thus enabling timely, informed decisions for disaster management.

---

## Motivation and Impact
**Imagine a scenario where local authorities and community members can instantly see rising water levels on a dashboard, receiving alerts before flooding occurs.**

The system aims to:
- **Enhance Public Safety** – Rapid, localized alerts give communities critical lead time to evacuate or prepare.
- **Protect Infrastructure** – Early detection reduces damage to roads, bridges, and other essential services.
- **Improve Emergency Management** – Centralized real-time data supports coordinated, data-driven responses.
- **Facilitate Sustainable Urban Planning** – Historical sensor data can guide decisions on urban development, zoning, and flood defense systems.
- **The new updates with IOT technologies and protocols will also**
-   Extend Data Reach – Local data are now globally accessible via Ubidots.
-   Real-Time Decision Making – Enhanced responsiveness through MQTT integration.
-   Automated Alert Controls – Remote control via Ubidots switches for alarms.
-   Enhanced Resilience – Reliable communications and data backup on the Raspberry Pi.

---

## System Architecture Overview

### 1. Concurrent ESP32 Threads
- **Measurement Task:** Collects data from all sensors, applies filtering and calibration, and packages the readings for internal processing.
- **WebServer Task:** Runs an HTTP server on the local network, serving the front-end dashboard (HTML, CSS, and JavaScript) and providing real-time sensor data.
- **MQTT Task:** Captures, calibrates, and publishes sensor data to Raspberry Pi via MQTT.

### 2. Sensor Modules
- **Ultrasonic Sensor (Digital):** Measures river height with high accuracy.
- **DHT11 (Digital):** Provides ambient temperature and humidity.
- **Tilt Switch (Digital):** Detects wind-related inclinations via a windsock mechanism.
- **Raindrop Detector (Digital):** Delivers a Boolean output indicating precipitation events.
- **BMP180 Barometric Sensor (I2C):** Reserved for future storm intensity assessments via atmospheric pressure data.

### 3. Alert and Visualization
- **LCD Display (I2C):** Displays real-time system status and risk levels.
- **Active Buzzer (Digital):** Emits audible alerts when flood risk thresholds are exceeded.
- **LED Indicators (Digital):** Provides immediate visual signals for different alert states.

### 4. LAN-Restricted Web Dashboard
- Renders real-time sensor readings and risk levels in a browser interface.
- Provides charts, tables, and alert notifications for intuitive monitoring.

### 5. Raspberry Pi 5 (Gateway, Broker, Database)
- **MQTT Broker (Mosquitto):** Centralized messaging hub for sensor data.
- **SQLite Database (challenge3.db):** Stores historical sensor readings for analysis.
- **MQTT Relay:** Publishes latest database records to the Ubidots global MQTT broker.

### 6. Ubidots Integration
- **Global Dashboard:** Displays worldwide-accessible dashboards.
- **Remote Alert Control:** MQTT switch control for activating the ESP32 alarm remotely.

> ![Diagrama de bloques del sistema ](https://github.com/Edwinguty2/Challenge_3/blob/main/Diagrama%20de%20Bloques.jpeg)

---

## Hardware Components

### ESP32 WROOM32
- **Dual-core microcontroller** with integrated WiFi, running two FreeRTOS tasks concurrently.
- Supports both I2C communication (for BMP180 and LCD) and digital interfaces for other sensors.

### Ultrasonic Sensor
- Accurately captures water level variations.
- Effective range tailored for the prototype (approximately 2–7 cm in demo settings).

### DHT11 Temperature & Humidity Sensor
- Low-cost sensor for environmental monitoring.
- Captures temperature and humidity data to correlate weather trends with flood conditions.

### Tilt Switch in a Windsock
- Provides wind-related data, indicating the presence of strong gusts.
- Outputs a binary signal that, when combined with other sensor inputs, helps assess storm risk.

### Raindrop Sensor
- Detects the presence of precipitation.
- Refined logic reduces false positives under light drizzle conditions.

### BMP180 Barometric Sensor (I2C)
- Currently reserved for future use to monitor atmospheric pressure and predict storm intensity.

### LCD Display (I2C)
- Offers local, on-site information about water levels and risk states.
- Particularly useful when network connectivity is limited or during field inspections.

### Raspberry Pi 5
- **MQTT Broker:** Ensures efficient local data handling.
- **Database Management:** SQLite for structured sensor data storage.
- **Gateway Functions:** Bridges local sensor network and global cloud infrastructure.

---

## Software Design & Implementation

### Multi-Thread Concurrency
- **Measurement Task:**
  - Samples sensors at predefined intervals.
  - Applies filtering and calibration to raw sensor data.
  - Compiles readings for internal processing.
- **WebServer Task:**
  - Hosts an HTTP server that serves the front-end dashboard.
  - Responds to data requests with real-time sensor information.
  - Handles RESTful API endpoints for future enhancements, such as remote configuration updates.
- **MQTT Task:**
  - Sends MQTT packages to share the data world wide

### MQTT Communication Protocol
- ESP32 publishes JSON-formatted sensor readings to Raspberry Pi MQTT broker.
- Raspberry Pi subscribes, logs data into SQLite, and forwards latest data points to Ubidots.

### Alert Control via Ubidots
- Raspberry Pi subscribes to an Ubidots MQTT switch topic.
- Remote control of ESP32’s active buzzer alerts.

### Database Integration
- Structured SQL storage enables historical data analysis and future predictive modeling.
### Risk Evaluation Algorithm
- **CALM:** Normal water levels with no significant wind or rain.
- **LOW RISK:** Moderate wind or light rainfall detected.
- **OVERFLOW RISK:** Combination of rising water levels, wind, and rain indicates an impending storm.
- **HIGH RISK:** Rapid water-level rise accompanied by intense wind and heavy rainfall signals active flooding.

## Alert and Visualization
- **Local Dashboard:** ESP32 provides immediate, LAN-restricted insights.
- **Global Dashboard:** Ubidots integration facilitates worldwide monitoring.
- **Remote Alert Activation:** Ubidots MQTT switches allow global control of ESP32 alarms.

  ![Esquema UML del modelo ](https://github.com/Edwinguty2/Challenge_3/blob/main/Drawing%20UML.png)

---

## Potential Impact & Future Directions

- **Community-Centric Safety:** Real-time alerts empower local residents and authorities to take proactive measures.
- **Enhanced Emergency Coordination:** Integration with municipal systems can streamline evacuation and resource allocation.
- **Long-Term Urban Planning:** Historical data can inform infrastructure development, floodplain mapping, and zoning decisions.
- **Scalability:** The modular design and dual-thread architecture allow for easy expansion and the addition of new sensor nodes.

---
## Conclusion


## Self-Assessment of the Testing Protocol

The **ESP32 MQTT Enhanced Flood Monitoring System** significantly extends the capabilities of real-time environmental monitoring by integrating local sensor networks with global visualization platforms via MQTT. It provides a powerful, scalable solution for managing flood risks proactively, safeguarding lives and infrastructure. It effectively addresses the verification of all critical components, including individual procedures for each sensor (ultrasonic, DHT11, BMP180, rain detector, and wind sensor), communication tests with the web interface, and validation of the alert system (buzzer, LEDs, LCD).

---
## Conclusion
The **ESP32 MQTT Enhanced Flood Monitoring System** significantly extends the capabilities of real-time environmental monitoring by integrating local sensor networks with global visualization platforms via MQTT. It provides a powerful, scalable solution for managing flood risks proactively, safeguarding lives and infrastructure.

**Flood prevention starts with early detection—join us in creating safer, more resilient communities.**

---

## Summary Comparison Table

| **Feature**          | **ESP32 Edition**           | **ESP32 MQTT Enhanced Edition** |
|----------------------|-----------------------------|---------------------------------|
| **Communication**    | LAN Only                    | LAN + MQTT (Local & Global)     |
| **Data Storage**     | Local Memory                | SQLite on Raspberry Pi 5        |
| **Remote Alerts**    | Manual Onsite Activation    | Ubidots MQTT-controlled         |
| **Visualization**    | Local Dashboard             | Local & Global Dashboards       |

## Experimental Setup and Results
1. **Experimental Setup:**
   - Scale rural house model for realistic testing.
2. **Simulated Conditions:**
   - Realistic flooding scenarios tested to validate system robustness.

---

## Challenges and Design Constraints

- During the development of the Real-Time Flood Monitoring and Alert System (ESP32 Edition), the project faced multiple challenges and constraints. Limited testing time due to short lab access periods, and restricted hardware availability complicated the circuit assembly and thorough validation of the system. Additionally, technical limitations arose from the LAN-restricted design, intended to maintain secure and controlled local communications. Environmental exposure posed risks to electronic components, particularly the rain detector and wiring, necessitating protective solutions that added cost and complexity to potential large-scale deployment. Accurate sensor placement was another challenge due to physical and accessibility constraints.
- Furthermore, integrating the MQTT protocol with Ubidots introduced submission limitations, requiring careful management of data frequency to avoid exceeding platform restrictions while maintaining real-time monitoring capabilities.

---

## Experimental Setup, Results, and Analysis
1. **Experimental Setup:**  
   A scale model of a rural house was used to simulate a real-world environment for testing the system.

2. **Simulated Environmental Conditions:**  
   Scenarios were recreated to mimic variations in water levels and typical rural environmental conditions.

---

## Referencias

- [¿Qué es MQTT? - AWS](https://aws.amazon.com/es/what-is/mqtt/)
- [Explicación MQTT en español - YouTube (Parte 1)](https://www.youtube.com/watch?v=H0dDKo-xgUI&t=137s)
- [Cómo funciona MQTT - Curso de Arduino y Domótica - YouTube](https://www.youtube.com/watch?v=BFyPzC6No8k&t=546s)
- [Tutorial MQTT Ubidots - YouTube](https://www.youtube.com/watch?v=e2uPHYnxkAE&t=114s)
- [Control de dispositivos usando MQTT - Ubidots Help](https://help.ubidots.com/en/articles/654718-controlling-devices-using-mqtt)



## Contributors
- **Edwin Alejandro Gutierrez Rodriguez**  
- **Nicolas Stiven Ortiz Cortes**  
- **Juan Díego Lemus Rey**  
- **Universidad de La Sabana**
