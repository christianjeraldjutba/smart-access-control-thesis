# IMPLEMENTATION OF SMART ACCESS CONTROL IN HOTEL ROOMS INTEGRATING RFID AND IOT

**A Thesis Project by Leah Mae P. Eyas, Samantha Shane M. Gabales, and Christian Jerald S. Jutba**
_Presented to the Faculty of College of Engineering, Jose Rizal Memorial State University - Main Campus (April 2024)_

---

## 📖 Abstract

The hospitality industry is increasingly shifting towards digital transformation to enhance guest experiences through seamless, secure, and contactless services. This project addresses the critical gap in hotel systems characterized by fragmented solutions for room access, Wi-Fi connectivity, and guest service management. We developed and implemented an Integrated Smart Access Control System that combines RFID-based room access, automated MAC-address-based Wi-Fi provisioning, and a real-time guest service request platform. Utilizing a Raspberry Pi 4, the system offers a robust, scalable, and secure IoT-enabled solution that optimizes hotel operations and enhances guest satisfaction.

**Keywords:** Smart Access Control, RFID, Internet of Things (IoT), Raspberry Pi, Automated Wi-Fi Connectivity, MAC Address Authentication, Hotel Management System

---

## 🚀 Key Features

This project is composed of two primary systems that work in concert: a hardware-based door access system and a full-stack mobile application suite.

#### 🚪 RFID & IoT Door System
* **Contactless Room Access:** Securely unlock hotel doors using a high-frequency (13.56 MHz) RFID card.
* **Automated & Secure Wi-Fi:** Guests connect to the internet automatically after the first RFID scan at their door, with no passwords required. This is achieved through secure MAC address whitelisting on a MikroTik router.
* **Real-Time Occupancy Monitoring:** The system instantly notifies the management dashboard when a room is entered, helping to streamline housekeeping and enhance hotel security.
* **Centralized Hardware Control:** A Raspberry Pi 4 acts as the core microcontroller, managing RFID authentication, the electric strike lock, and all network communications.

#### 📱 Full-Stack Mobile Applications
This project includes two distinct mobile applications built with Flutter:

1.  **SAC Access App (Guest Self Check-in):**
    * **Automated Onboarding:** Allows new guests to register and returning guests to log in, completely bypassing the front desk.
    * **Room & RFID Selection:** Guests can browse and select available rooms and then choose an available RFID card to be linked to their booking.
    * **Seamless Wi-Fi Setup:** Guides the guest to scan a QR code to connect to a preliminary Wi-Fi network, which securely captures their device's MAC address for automated internet access later.

2.  **SAC Service App (Guest & Admin Portal):**
    * **Guest Dashboard:** Once checked in, guests can request services (housekeeping, in-room dining), track the real-time status of their requests, and submit feedback.
    * **Admin Dashboard:** Hotel staff can monitor key metrics (guest count, room availability), manage all service requests, view guest profiles and tiers (Regular, Gold, Platinum), manage the entire lifecycle of RFID cards, and review system logs categorized by severity (Info, Warning, Error).

---

## 🏛️ System Architecture & Design

The system integrates guest interaction, hardware components, and backend services into a cohesive unit. The process begins with a guest using the **SAC Access App** for self-service check-in. Once their RFID card is assigned, they can approach their room. When the guest scans the card, it triggers the Raspberry Pi to validate the UID against the Supabase database. Upon successful authentication, the electric strike lock is activated, and the guest's previously captured MAC address is whitelisted on the MikroTik router, granting immediate and full internet access.

#### System Architecture Diagram
![System Architecture Diagram](assets/system-architecture/1.png)

#### Hardware Prototype
![Final Hardware Prototype](assets/hardware-prototype/1.jpg)

#### Final Output
![Final Hardware Prototype](assets/final-output/final.jpg)

---

## ⚙️ Software Deep Dive & Core Logic

This system is built on a robust software foundation, with distinct logic for handling guest onboarding, authentication, and real-time communication between the hardware and the cloud.

### The Automated Check-in & Wi-Fi Provisioning Flow
To create a seamless "no front desk" experience, the system uses a precise algorithm:

1.  **Guest Onboarding (SAC Access App):** A new guest registers or a returning guest logs in. They select an available room and an RFID card.
2.  **QR Code Scan for MAC Capture:** The app displays a QR code. The guest scans it, which connects their device to a sandboxed "Guest Wifi" network. This network has **no internet access**, but the connection allows the MikroTik router to log the device's unique MAC address in its DHCP lease table.
3.  **Backend Polling & Storage:** A Node.js backend service periodically polls the MikroTik router, retrieves the new MAC address, and stores it in the Supabase database with an `unauthenticated` status, linked to the guest's profile.
4.  **First RFID Scan at the Door:** The guest taps their assigned RFID card on the door reader for the first time. The Raspberry Pi validates the card with the backend.
5.  **Unlocking & Internet Activation:** Upon successful validation, two things happen simultaneously:
    * The electric strike lock is triggered, unlocking the door.
    * The backend updates the guest's MAC address status to `authenticated` and adds their device's IP to the router's `guest_whitelist`, granting full internet access.
6.  **Seamless Experience:** The guest enters their room and their device is now fully connected to the internet automatically for the duration of their stay.

---

## 🛠️ Technology Stack

| Category                | Technology / Component                                                                                |
| ----------------------- | ----------------------------------------------------------------------------------------------------- |
| **Mobile Apps** | `Flutter`, `Android Studio`, `VSCode`                                                                 |
| **Backend & API** | `Node.js`, `PostgreSQL`, `Supabase`                                                                   |
| **Hardware** | `Raspberry Pi 4`, `High-Frequency RFID Reader`, `Electric Strike Lock`, `5V Relay Module`, `MikroTik Router` |
| **Protocols & Methods** | `REST API`, `MAC Address Authentication`, `SHA-256 Encryption`, `QR Code Scanning`, `IoT`                  |
| **Development Model** | `System Development Life Cycle (SDLC) - Waterfall Model`                                              |

---

## 🌐 Project Website & Demos

#### Official Website
Click the thumbnail below to visit the project's official landing page, where you can learn more and download the app.

[![Project Website Thumbnail](assets/website/website.png)](https://smartaccesscontrol-thesis.web.app/)

#### Hardware Demo Video
Click the thumbnail below to watch our 7-minute project presentation and demo.

[![Project Demo Video Thumbnail](assets/video/video_thumbnail.jpg)](https://youtu.be/MRfp8YENM7M)

---

## 📸 Application Screenshots

#### Guest Mobile Application (Android)
| Guest Login                                       | Home Dashboard                                    | Service Request Form                                      | Request History                                     |
| :------------------------------------------------ | :------------------------------------------------ | :---------------------------------------------------------- | :-------------------------------------------------- |
| ![Guest Login](assets/mobile/guest_login.png)     | ![Guest Home](assets/mobile/guest_home.png)       | ![Service Request](assets/mobile/service_request.png)       | ![Request History](assets/mobile/request_history.png) |

---

## 🧪 Testing & Performance Validation

The system underwent rigorous testing to ensure reliability, accuracy, and a high-quality user experience, with results confirming the system meets or exceeds its defined objectives and industry standards.

| Metric                        | Result                      | Standard / Goal                                          |
| ----------------------------- | --------------------------- | -------------------------------------------------------- |
| **Door Unlock Speed** | `178.25 ms` (Average)       | ≤ 500 ms (ISO/IEC 17025:2017)                     |
| **Wi-Fi Auto-Connect Time** | `458.02 ms` (Average)       | ≤ 5 seconds (IEEE 802.11ax)                         |
| **RFID Tag Accuracy** | `100%`                      | ≥ 99.5% (ISO/IEC 14443)                                 |
| **RFID Error Rate** | `0 Errors`                  | ≤ 0.1 errors per 10 scans (High-Reliability Systems)  |
| **Unlock Reliability**| `96.67%`                    | ≥ 99.9% (ISO/IEC 27001:2013)                            |
| **App Navigation Ease** | `4.93 / 5.0` Rating         | ≥ 4.5 (Nielsen Norman Group Guidelines)           |
| **Overall User Satisfaction** | `5.0 / 5.0` Rating          | ≥ 4.5 (User Experience Research Studies)     |
| **Recommendation Rate** | `100%`                      | N/A                                                    |

---

## 💡 Conclusion & Future Recommendations

The successful deployment of the RFID-integrated Smart Access Control System substantiates the benefits and viability of embedding IoT technologies within hospitality settings. By creating a unified digital ecosystem that consolidates door access, Wi-Fi connectivity, and service request management, the study significantly mitigated operational inefficiencies and elevated the overall guest experience. The system establishes a strong foundation for continued innovation within the field of smart hospitality, directly enhancing guest satisfaction and operational performance.

**Future Work:**
Based on the project's outcomes, the following enhancements are recommended:

* **Enhanced Security:** Implement advanced security protocols like multi-factor authentication and blockchain-based logging to further strengthen data protection.
* **Power Backup:** Incorporate an uninterrupted power supply (UPS) and failover systems to maintain critical functions during power outages.
* **Cross-Platform Support:** Expand the guest apps to iOS and add offline capabilities to ensure seamless operation across all mobile operating systems.
* **AI Integration:** Explore using AI-driven analytics for predictive guest preference management and optimized service delivery.
* **Guest Feedback Integration:** Integrate formal mechanisms for capturing post-stay feedback and satisfaction surveys directly within the app to continuously refine the user experience.