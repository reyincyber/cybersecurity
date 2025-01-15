# Assessing the Effectiveness of Defensive Strategies at Each Stage of the Cyber Kill Chain

This project examines the effectiveness of defensive strategies during simulated cyberattacks, leveraging the Cyber Kill Chain framework. Simulated attacks were conducted using tools such as OWASP Zap, Burp Suite, and Kali Linux. The defensive strategies were assessed at each stage of the attack cycle, targeting a vulnerable application: OWASP Juice Shop.

---

## Table of Contents
1. [Introduction](#introduction)
2. [Project Overview](#project-overview)
3. [Methodology](#methodology)
4. [Simulation Details](#simulation-details)
   - [Reconnaissance](#reconnaissance)
   - [Weaponization and Delivery](#weaponization-and-delivery)
   - [Exploitation](#exploitation)
   - [Actions on Objectives](#actions-on-objectives)
5. [Findings and Recommendations](#findings-and-recommendations)
6. [Conclusion](#conclusion)
7. [References](#references)

---

## Introduction

The **Cyber Kill Chain framework**, developed by Lockheed Martin, outlines the sequential stages of a cyberattack: reconnaissance, weaponization, delivery, exploitation, installation, command and control (C2), and actions on objectives. By understanding this framework, organizations can strengthen their defenses against cyber threats.

This project used the Cyber Kill Chain to simulate cyberattacks on the **OWASP Juice Shop**—a deliberately vulnerable web application—and evaluated defensive strategies at each stage.

---

## Project Overview

- **Objective**: Assess the effectiveness of defensive strategies in mitigating cyberattacks at various stages of the Cyber Kill Chain.
- **Environment**: A controlled simulation using:
  - **Tools**: OWASP Juice Shop, Kali Linux, Burp Suite
  - **Focus**: Simulated attacks on web application vulnerabilities
- **Limitations**:
  - OWASP Juice Shop's predefined vulnerabilities limit real-world scope.
  - Lack of granular metrics (e.g., precise defense rate, response times).

---

## Methodology

1. **Teams**: Divided into an attack team and a defense team.
   - The attack team followed the Cyber Kill Chain stages to simulate an attack.
   - The defense team implemented and tested defensive strategies.
2. **Target**: OWASP Juice Shop, chosen for its representation of real-world web application vulnerabilities.
3. **Tools Used**:
   - **Burp Suite**: For scanning, vulnerability identification, and payload delivery.
   - **Kali Linux**: For attack simulation and OSINT.
   - **OWASP Zap**: For vulnerability assessment.

---

## Simulation Details

### **Prerequisites**
1. **Set up Virtual Environment**:
   - Install **VirtualBox** on your host machine.
   - Download and install **Kali Linux** ISO and configure it in VirtualBox.
   - Allocate sufficient resources (4GB RAM, 2 CPUs recommended).

2. **Install OWASP Juice Shop**:
   - Install **Node.js** and **npm** on your host machine or another VM.
   - Clone the Juice Shop repository:  
     ```bash
     git clone https://github.com/juice-shop/juice-shop.git
     cd juice-shop
     npm install
     npm start
     ```
   - Access Juice Shop at `http://127.0.0.1:4200` from Kali Linux.

3. **Install Necessary Tools**:
   - Install **Burp Suite**:  
     ```bash
     sudo apt install burpsuite
     ```
   - Update **Kali Linux** and install other tools if not pre-installed:  
     ```bash
     sudo apt update && sudo apt install nmap nikto gobuster
     ```

---

### Reconnaissance

- **Goal**: Gather information about the target system.
- **Steps**:
  1. Use **OSINT tools** (e.g., theHarvester) to collect publicly available data:  
     ```bash
     theHarvester -d example.com -b all
     ```
  2. Use **Burp Suite** to scan the Juice Shop URL for vulnerabilities:
     - Configure Burp Suite as a proxy.
     - Intercept and analyze traffic while browsing the Juice Shop application.
     - Perform a scan and identify vulnerabilities like **Broken Authentication** and **Broken Access Control**.

#### **Defensive Measures**:
- Implement **network segmentation** to limit data exposure.
- Monitor external sources for mentions of the Juice Shop.

---

### Weaponization and Delivery

- **Goal**: Prepare and deliver a payload targeting identified vulnerabilities.
- **Steps**:
  1. Use **Burp Suite's Intruder tool** to execute a brute-force attack:
     - Intercept login requests and set up payload positions.
     - Load a password list (e.g., `rockyou.txt`) and initiate the attack.
  2. Analyze responses for successful login attempts (e.g., HTTP 200 status).

#### **Defensive Measures**:
- Enforce **strong password policies**.
- Enable **MFA** for all user accounts.
- Adhere to the **Principle of Least Privilege**.

---

### Exploitation

- **Goal**: Exploit vulnerabilities to gain unauthorized access.
- **Steps**:
  1. Use stolen credentials to log in to Juice Shop and access restricted areas.
  2. Use **FTP tools** to download sensitive files:
     ```bash
     ftp 127.0.0.1
     ls
     get sensitive_file.txt
     ```

#### **Defensive Recommendations**:
- Deploy **Data Loss Prevention (DLP)** solutions.
- Implement **User Activity Monitoring (UAM)** systems.

---

### Actions on Objectives

- **Goal**: Achieve the final objective (data theft or system disruption).
- **Steps**:
  1. Use admin credentials to delete critical files:
     ```bash
     rm critical_file.txt
     ```

#### **Defensive Recommendations**:
- Maintain a robust **backup and recovery strategy**.
- Encrypt sensitive files to prevent plaintext access.
- Deploy **continuous monitoring tools** to detect anomalies and flag malicious activity.

---

## Findings and Recommendations

**Findings**:
   - OWASP Juice Shop demonstrated significant vulnerabilities to common attack methods.
   - Defensive measures varied in effectiveness; MFA and network segmentation were particularly impactful.

**Recommendations**:
   - Employ a **layered defense approach**, combining network segmentation, DLP, MFA, and strong password policies.
   - Implement incident response plans to respond swiftly to breaches.
   - Continuously monitor systems for suspicious activities.

---

## Conclusion

This project highlighted the vulnerabilities within the OWASP Juice Shop and the importance of a layered defense approach in mitigating cyberattacks. While the simulation was limited in scope, it provided valuable insights into the effectiveness of various defensive strategies against the Cyber Kill Chain.

---

## References

- Boehm, A. (2023). [What is Data Protection?](https://www.crowdstrike.com/cybersecurity-101/secops/data-protection/) - CrowdStrike.
- CrowdStrike. (2024). [What is Data Loss Prevention (DLP)?](https://www.crowdstrike.com/cybersecurity-101/data-loss-prevention-dlp/) - CrowdStrike.
- EC-Council. (2022). [The Cyber Kill Chain: The Seven Steps of a Cyberattack](https://www.eccouncil.org/cybersecurity-exchange/threat-intelligence/cyber-kill-chain-seven-steps-cyberattack/).
- Martin, L. (2011). [Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html) - Lockheed Martin.
- Roy, G. K. (2024). [OWASP Juice Shop](https://www.scaler.com/topics/cyber-security/owasp-juice-shop/) - Scaler Topics.
