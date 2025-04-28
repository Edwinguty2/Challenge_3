# Real-Time Flood Monitoring and Alert System for Colombian Rivers (ESP32 Edition)

## Abstract
Colombia’s complex topography and dynamic climate pose significant challenges in predicting and managing flood events. This repository presents an **enhanced IoT-based solution** that leverages an **ESP32 WROOM32** microcontroller running two concurrent tasks—one dedicated to sensor data processing and another to hosting a local web server. By integrating **multiple sensors** (ultrasonic, DHT11, tilt switch, raindrop detector, and a BMP180 barometric sensor over I2C), the system continuously monitors river conditions and environmental parameters. Sensor data are streamed over a LAN-based dashboard, providing **real-time insights** and early flood warnings to local communities and authorities.

---

## Introduction
Frequent river floods across Colombia—amplified by phenomena like La Niña—continue to threaten public safety, disrupt infrastructure, and strain emergency response efforts. Traditional monitoring systems often lack the agility to deliver **timely, localized alerts**, leaving communities vulnerable to sudden surges in water levels.

This project builds upon previous work by replacing the Arduino Nano with an **ESP32 WROOM32**, offering **dual-thread concurrency** for simultaneous data processing and web service hosting. The system now streams sensor readings over a local network, providing a **scalable, modular**, and **near-real-time** framework for flood detection and early warning.

---

## Motivation and Impact
**Imagine a scenario where local authorities and community members can instantly see rising water levels on a dashboard, receiving alerts before flooding occurs.**

The system aims to:
- **Enhance Public Safety** – Rapid, localized alerts give communities critical lead time to evacuate or prepare.
- **Protect Infrastructure** – Early detection reduces damage to roads, bridges, and other essential services.
- **Improve Emergency Management** – Centralized real-time data supports coordinated, data-driven responses.
- **Facilitate Sustainable Urban Planning** – Historical sensor data can guide decisions on urban development, zoning, and flood defense systems.

By adopting **cutting-edge sensor fusion** and **concurrent data processing**, this solution takes a proactive approach to disaster risk reduction.

---

## System Architecture Overview

### 1. Concurrent ESP32 Threads
- **Measurement Task:** Collects data from all sensors, applies filtering and calibration, and packages the readings for internal processing.
- **WebServer Task:** Runs an HTTP server on the local network, serving the front-end dashboard (HTML, CSS, and JavaScript) and providing real-time sensor data.

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

---

## Software Design & Implementation

### Dual-Thread Concurrency
- **Measurement Task:**
  - Samples sensors at predefined intervals.
  - Applies filtering and calibration to raw sensor data.
  - Compiles readings for internal processing.
- **WebServer Task:**
  - Hosts an HTTP server that serves the front-end dashboard.
  - Responds to data requests with real-time sensor information.
  - Handles RESTful API endpoints for future enhancements, such as remote configuration updates.

### Risk Evaluation Algorithm
- **CALM:** Normal water levels with no significant wind or rain.
- **LOW RISK:** Moderate wind or light rainfall detected.
- **OVERFLOW RISK:** Combination of rising water levels, wind, and rain indicates an impending storm.
- **HIGH RISK:** Rapid water-level rise accompanied by intense wind and heavy rainfall signals active flooding.

### User Interface & Alerts
- **LCD Display:** Cycles through system metrics and current risk levels.
- **LED Indicators:** Change color or blink based on the severity of the alert.
- **Active Buzzer:** Activates distinct audible tones when critical thresholds are surpassed.

  ![Esquema UML del modelo ](https://github.com/Edwinguty2/Challenge_3/blob/main/Drawing%20UML.png)

---

## Potential Impact & Future Directions

- **Community-Centric Safety:** Real-time alerts empower local residents and authorities to take proactive measures.
- **Enhanced Emergency Coordination:** Integration with municipal systems can streamline evacuation and resource allocation.
- **Long-Term Urban Planning:** Historical data can inform infrastructure development, floodplain mapping, and zoning decisions.
- **Scalability:** The modular design and dual-thread architecture allow for easy expansion and the addition of new sensor nodes.

---

## Self-Assessment of the Testing Protocol

The testing protocol for the Real-Time Flood Monitoring and Alert System (ESP32 Edition) demonstrates a comprehensive and methodical approach to ensuring the reliability and functionality of the system prior to field deployment. It effectively addresses the verification of all critical components, including individual procedures for each sensor (ultrasonic, DHT11, BMP180, rain detector, and wind sensor), communication tests with the web interface, and validation of the alert system (buzzer, LEDs, LCD). The protocol is practical and replicable, offering clear steps that integrate both hardware and software validation, such as calibrating the ultrasonic sensor using real-world references, simulating rainfall and wind conditions, and testing REST API endpoints. The inclusion of a troubleshooting section enhances the protocol's usability by enabling quick identification and resolution of common issues. Although the current manual procedures are effective, future iterations could benefit from the introduction of automation, stress testing under prolonged use, and structured logging of test results to increase scalability and long-term reliability. Overall, the protocol supports the project's goal of delivering a robust, real-time flood monitoring system by ensuring that each module operates correctly under both normal and emergency conditions.

---

## Conclusion
The **Real-Time Flood Monitoring and Alert System (ESP32 Edition)** delivers a **robust, scalable**, and **cost-effective** solution to the challenges faced by flood-prone regions in Colombia. By combining **advanced sensor fusion**, **dual-thread processing**, and a **LAN-based dashboard**, the system provides rapid, reliable insights that can help **save lives** and **protect infrastructure**.

**Flood prevention starts with early detection—join us in creating safer, more resilient communities.**

---

## Summary Comparison Table

| **Feature**             | **Original (Arduino Nano)** | **ESP32 Edition**             |
|-------------------------|-----------------------------|-------------------------------|
| **Microcontroller**     | Arduino Nano                | ESP32 WROOM32 (Dual-Thread)   |
| **Data Exchange**       | I2C, Digital pins           | Same + Streamed JSON over LAN |
| **Rain Logic**          | Basic Thresholds            | Corrected and parametrized    |
| **Architecture**        | Linear Loop                 | Dual-Thread with Interrupts   |


---
## Design Constraints Identified
 
LAN Restriction:   The system is limited to operating exclusively on a local area network (LAN), ensuring secure and controlled communication.

Also, During the development of the Real-Time Flood Monitoring and Alert System (ESP32 Edition), several technical and operational constraints were identified that influenced the design and implementation decisions. One of the main limitations was the exposure of electronic components to environmental conditions, especially the wiring and sensors such as the rain detector, whose sensitivity to direct water contact posed electrical risks and required protective solutions that could increase both cost and logistical complexity for large-scale deployment. Additionally, there was a need to place sensors in strategic positions to ensure accurate readings, which is not always feasible in real-world settings due to physical or access limitations.

---

## Experimental Setup, Results, and Analysis
1. **Experimental Setup:**  
   A scale model of a rural house was used to simulate a real-world environment for testing the system.

2. **Simulated Environmental Conditions:**  
   Scenarios were recreated to mimic variations in water levels and typical rural environmental conditions.

---

## Challenges Faced During Project Development
1. **Limited Testing Time:**  
   Sensors were available for lab use only for 2 hours, which significantly restricted testing time.

2. **Holiday Impact:**  
   The testing schedule was further affected by a holiday on Monday, reducing the opportunity to conduct tests under real conditions.

3. **Logistical Limitations:**  
   Coordination and planning of the tests were impacted by restrictions in lab availability and access.

## Contributors
- **Edwin Alejandro Gutierrez Rodriguez**  
- **Nicolas Stiven Ortiz Cortes**  
- **Juan Díego Lemus Rey**  
- **Universidad de La Sabana**
