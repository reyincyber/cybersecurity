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
1. **Target**: OWASP Juice Shop, chosen for its representation of real-world web application vulnerabilities.
3. **Tools Used**:
   - **Burp Suite**: For scanning, vulnerability identification, and payload delivery.
   - **OWASP Zap**: For vulnerability assessment.
   - **Kali Linux**: For attack simulation and OSINT.

---

## Simulation Details

### Reconnaissance

- **Goal**: Gather information about the target system.
- **Actions**:
  - Used **OSINT** to collect publicly available data.
  - Leveraged **Burp Suite** to scan for vulnerabilities on the Juice Shop's URL.
- **Findings**:
  - Identified vulnerabilities: **Broken Authentication** and **Broken Access Control**.
- **Defensive Measures**:
  - Implemented network segmentation to limit information access.
  - Monitored external sources for mentions of the Juice Shop.

### Weaponization and Delivery

- **Goal**: Prepare and deliver a payload targeting identified vulnerabilities.
- **Actions**:
  - Used a brute-force password-cracking script via Burp Suite's Intruder tool.
  - Exploited weak password policies.
- **Outcome**:
  - Successfully matched a valid admin password (**admin123**) from the payload.
- **Defensive Measures**:
  - Enforced strong password policies (minimum length, character variety).
  - Enabled **Multi-Factor Authentication (MFA)** for all user accounts.
  - Adhered to the **Principle of Least Privilege**.

### Exploitation

- **Goal**: Exploit vulnerabilities to gain unauthorized access.
- **Actions**:
  - Used stolen credentials to access admin privileges.
  - Downloaded sensitive files from the Juice Shop’s FTP server.
- **Defensive Recommendations**:
  - Deploy **Data Loss Prevention (DLP)** solutions to restrict unauthorized data exfiltration.
  - Implement **User Activity Monitoring (UAM)** systems to detect suspicious behaviors.

### Actions on Objectives

- **Goal**: Achieve the final objective (data theft or system disruption).
- **Actions**:
  - Deleted critical files using stolen admin credentials.
- **Impact**:
  - Potential for severe disruption and compliance issues in real-world scenarios.
- **Defensive Recommendations**:
  - Maintain a robust backup and recovery strategy.
  - Encrypt sensitive files to prevent plaintext access.
  - Deploy continuous monitoring tools for anomaly detection.

---

## Findings and Recommendations

1. **Findings**:
   - OWASP Juice Shop demonstrated significant vulnerabilities to common attack methods.
   - Defensive measures varied in effectiveness; MFA and network segmentation were particularly impactful.

2. **Recommendations**:
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

---

## How to Use This Repository

This repository provides documentation, simulation scripts, and configurations for assessing cyber defense strategies. For further assistance or to contribute, please see the [CONTRIBUTING.md](CONTRIBUTING.md) file.

---
