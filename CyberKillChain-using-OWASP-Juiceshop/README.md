# Assessing the Effectiveness of Defensive Strategies at Each Stage of the Cyber Kill Chain in Simulated Cyberattacks

## Introduction

The **Cyber Kill Chain** framework, developed by **Lockheed Martin (2011)**, outlines the seven stages of a cyberattack. This model helps organizations understand attacker tactics and apply appropriate defensive measures.  

### **Cyber Kill Chain Stages**
1. **Reconnaissance**  
2. **Weaponization**  
3. **Delivery**  
4. **Exploitation**  
5. **Installation**  
6. **Command and Control (C2)**  
7. **Actions on Objectives**  

This project simulates cyberattacks at each stage using **Kali Linux, Burp Suite, and OWASP ZAP**, targeting the **OWASP Juice Shop**, a deliberately vulnerable web application. The effectiveness of defensive strategies at each stage is evaluated.

🖇️ Documentation: [Github](https://github.com/reyincyber/cybersecurity/tree/9a3b50674c70b47eb1d74a422f618e4a9008f26f/CyberKillChain-using-OWASP-Juiceshop); [Medium](https://cyberrey.medium.com/assessing-the-effectiveness-of-defensive-strategies-at-each-stage-of-the-cyber-kill-chain-in-fdc74515ea58)

---

## **Methodology**
1. **Set up** OWASP Juice Shop on a local machine.  
2. **Simulate cyberattacks** using penetration testing tools.  
3. **Apply defensive strategies** at each attack stage.  
4. **Evaluate the effectiveness** of mitigation techniques.

---

## **Tools Used**
| Tool          | Purpose |
|--------------|---------|
| **Kali Linux**  | Security testing OS with pre-installed hacking tools |
| **Burp Suite**  | Web vulnerability scanner and proxy tool |
| **OWASP ZAP**   | Open-source security testing tool for web applications |
| **OWASP Juice Shop** | A deliberately vulnerable web application for training |

---

## **Installation and Setup**

### **Prerequisites**
**Node.js** (LTS version recommended), **npm** (comes with Node.js), **Git**, **Docker** (optional, for containerized setup)

Setting Up and Testing OWASP Juice Shop on a Local Machine
Prerequisites: Node.js (LTS version recommended), npm (comes with Node.js), Git, Docker (optional, for containerized setup)

#### **Set up Virtual Environment:**
— Install VirtualBox on your host machine.
— Download and install Kali Linux ISO and configure it in VirtualBox.
— Allocate sufficient resources (4GB RAM, 2 CPUs recommended).
— Install Burp Suite: sudo apt install burpsuite
— Update Kali Linux and install other tools if not pre-installed:
sudo apt update && sudo apt install nmap nikto gobuster

#### **Install OWASP Juice Shop:**
**Method 1: Running with Node.js**
— Install Node.js and npm on your host machine or another VM.
— Clone the Juice Shop repository:
git clone https://github.com/juice-shop/juice-shop.git
 cd juice-shop
 npm install
 npm start
— Access Juice Shop at http://localhost:3000 from Kali Linux.

**Method 2: Running with Docker**
Pull the official Docker image: docker pull bkimminich/juice-shop
Run the container:docker run -d -p 3000:3000 bkimminich/juice-shop
Open a browser and navigate to http://localhost:3000.

## **Executing Simulated Attacks Using Penetration Testing Tools**

### **Stage 1: Reconnaissance (Information Gathering)**
The attacker searches through About page and DevTools (main.js) to locate hidden functionalities. Real-world equivalent: Attackers perform open-source intelligence (OSINT), scanning public repositories, exposed endpoints, or metadata leaks.

#### **Find the Hidden ‘Score Board’ Page**
Open the browser Developer Tools and go to the Sources tab.
Locate and open the main.js file.
Use the search function to find occurrences of score, identifying the route mapping section.
Navigate to http://localhost:3000/#/score-board to reveal the hidden scoreboard.

#### **Read Our Privacy Policy**
Log in to the application with any user account.
Open the dropdown menu on your profile picture and choose Privacy & Security.
Navigate to http://localhost:3000/#/privacy-security/privacy-policy.

### **Stage 2&3: Weaponization (Preparing the Attack) & Delivery (Deploying the Exploit)**
Here, The attacker scans for exposed directories using brute-force techniques (Reconnaissance). They identify and access sensitive logs (Weaponization). The log file is retrieved, providing potential sensitive information such as IPs, authentication logs, or even credentials (Delivery).

#### **Gain access to any access log file of the server**
Visit http://localhost:3000/ftp and notice the file `incident-support.kdbx` which is needed for [Log in with the support team’s original user credentials] and) indicates that some support team is performing its duties from the public Internet and possibly with VPN access.
Guess luckily or run a brute force attack with e.g. [OWASP ZAPs DirBuster plugin] for) a possibly exposed directory containing the log files.
Following the hint to drill down deeper than one level, you will at some point end up with http://localhost:3000/support/logs.
Inside you will find at least one `access.log` of the current day. Open or download it to solve this challenge.

### **Stage 4: Exploitation (Executing the Attack)**
The attacker uses weak or default credentials to exploit poor security hygiene. Real-world equivalent: Brute-force attacks, credential stuffing, or guessing common passwords.

#### **View another user’s shopping basket**
1. Log in as any user.
2. Put some products into your shopping basket.
3. Inspect the *Session Storage* in your browser’s developer tools to find a numeric `bid` value.
4. Change the `bid`, e.g. by adding or subtracting 1 from its value.
5. Visit http://localhost:3000/#/basket to solve the challenge.
If the challenge is not immediately solved, you might have to `F5`-reload to relay the `bid` change to the Angular client.

#### **Log in with the administrator’s user credentials**
Visit http://localhost:3000/#/login.
Use the following credentials:
Email: admin@juice-sh.op
Password: admin123
This exploits weak password security, logging into the admin account.

#### **Admin stolen credentials**
**SQL Injection to Log in as Admin**
Visit http://localhost:3000/#/login.
Use SQL injection payloads:
Email: ' OR 1=1--
Password: Any value
OR Email: admin@juice-sh.op'--
This allows authentication as the administrator without a valid password.
The attacker exploits SQL Injection to bypass authentication (‘ OR 1=1 — ).
Real-world equivalent: Exploiting insecure login forms using SQL Injection techniques.

### **Stage 5: Installation (Establishing Persistence)**
The attacker locates the admin panel and logs in using previously stolen credentials. This could be part of installation if the attacker plants a backdoor or escalates privileges. Real-world equivalent: Attackers gaining access to a C2 (Command & Control) panel or deploying web shells.

#### **Access the administration section of the store**
Open the main.js in your browser’s developer tools and search for “admin”.
One of the matches will be a route mapping to path: “administration”.
Navigating to http://localhost:3000/#/administration will give a 403 Forbidden error.
Log in to an administrator’s account by solving the challenge

#### **Perform a persisted XSS attack without using the frontend application at all**
Here, The attacker injects malicious JavaScript into the product description via direct API manipulation (Exploitation). The payload is stored in the database and executed when a user visits the affected page (Installation).
1. Log in to the application with any user.
2. Copy your `Authorization` header from any HTTP request submitted via browser.
3. Submit a POST request to http://localhost:3000/api/Products with
◦ `{“name”: “XSS”, “description”: “<iframe src=\”javascript:alert(`xss`)\”>”, “price”: 47.11}` as body,
◦ `application/json` as `Content-Type`
◦ and `Bearer ?` as `Authorization` header, replacing the `?` with the token you copied from the browser.
4. Visit http://localhost:3000/#/search.
5. An alert box with the text “xss” should appear.
6. Close this box. Notice the product row which has a frame border in the description in the *All Products* table
7. Click the “eye”-button next to that row.
8. Another alert box with the text “xss” should appear. After closing it the actual details dialog pops up showing the same frame border.


### **Stage 6: Command & Control (C2)**
No direct example from this simulation. However, an attacker could escalate the attack by uploading a malicious script in a real-world scenario.

### **Stage 7: Actions on Objectives (Achieving the Attack Goal)**
#### **Access the Administration Section**
Open Developer Tools and search for “admin” in main.js.
Find a route mapping to path: "administration".
Navigate to http://localhost:3000/#/administration.
If forbidden, log in as an admin using the previously exploited credentials.

#### **Access a Confidential Document**
Click on Check out our boring terms of use from the About Us page (http://localhost:3000/ftp/legal.adoc).
Modify the URL to browse the directory: http://localhost:3000/ftp.
Open http://localhost:3000/ftp/acquisitions.adoc to solve the challenge.
The attacker successfully bypasses directory restrictions (http://localhost:3000/ftp/acquisitions.adoc). Real-world equivalent: Data breaches, exfiltration of sensitive files.

#### **Perform an unwanted information disclosure by accessing data cross-domain**
Here, the attacker exploits a misconfigured API that allows unauthenticated requests with JSONP (Exploitation). This vulnerability could lead to Cross-Origin Data Theft, where sensitive user information is accessed (Actions on Objectives).
1. Find a request to the `/rest/user/whoami` API endpoint. Notice that you can remove the “Authorization” header and it still works.
2. Add a URL parameter called “callback”. This will cause the API to return the content as a JavaScript fragment (JSONP) rather than just a standard JSON object.


### **Limitations:**
The predefined vulnerabilities of OWASP Juice Shop may not fully represent real-world scenarios.
The absence of active security defenses (e.g., SIEM, IDS/IPS) in the test environment.

### **Key Takeaways**
Persistent XSS Injection via API Manipulation
Early-stage prevention (Reconnaissance & Weaponization) reduces attack success rates significantly.
Regular security assessments and penetration testing improve defenses.
Combining multiple security layers provides the best protection.

### **Conclusion**
This project demonstrated the effectiveness of layered security defenses against attacks at each stage of the Cyber Kill Chain. The simulations reinforced the importance of proactive security measures, including network monitoring, WAFs, user training, and endpoint security solutions.
Future work could explore:
Real-world threat intelligence integration.
Advanced machine learning-based anomaly detection.
Red Team/Blue Team exercises for a more robust security assessment.


## References
- Boehm, A. (2023). [What is Data Protection?](https://www.crowdstrike.com/cybersecurity-101/secops/data-protection/) - CrowdStrike.
- CrowdStrike. (2024). [What is Data Loss Prevention (DLP)?](https://www.crowdstrike.com/cybersecurity-101/data-loss-prevention-dlp/) - CrowdStrike.
- EC-Council. (2022). [The Cyber Kill Chain: The Seven Steps of a Cyberattack](https://www.eccouncil.org/cybersecurity-exchange/threat-intelligence/cyber-kill-chain-seven-steps-cyberattack/).
- Martin, L. (2011). [Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html) - Lockheed Martin.
- Roy, G. K. (2024). [OWASP Juice Shop](https://www.scaler.com/topics/cyber-security/owasp-juice-shop/) - Scaler Topics.
