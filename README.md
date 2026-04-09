# Web-Penetration-Test
Exploiting WordPress vulnerabilities (CVE-2025-34077), authentication bypass via cookie injection, and privilege escalation to root. Part of my Cybersecurity Specialization.

#  Day 1: Hacking my First Website
## Lab Report: Exploiting the "BigWear" Server

 ![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/9e59c770bf03ed0d5bf05ecda92ba3617a0982e7/images/1.png) 

In this first session of my Cybersecurity journey, I moved away from the myths. I learned that you don't need to be a math genius or an expert programmer to identify security breaches; you need **creativity, logic, and a solid methodology.**

---

##  The Opportunity in Crisis
While documenting this lab, I kept a key fact in mind: cyberattacks cost companies up to 20% of their annual turnover. There is a global gap of 4 million professionals, and I am training to fill that void.

### My Role in the Team:
During this lab, I acted as part of the **Red Team (Ethical Hacker)**, auditing systems and exploiting vulnerabilities before a real cybercriminal could.

---

##  Step-by-Step: The "BigWear" Compromise

I treated the "BigWear" server like an office building where I needed to reach the safe. Here is the logical process I followed:

### Step 1: Knocking on Doors (Scanning)
I started by identifying entry points.
* **Action:** I used `Nmap` to scan the "building" (the server).
* **Discovery:** I found **Port 80** open, running a website powered by **WordPress**.
  
  ![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/4096309a1aa3c706b8706a73ad7c78c36dfa55cf/images/nmap%201.png)

  ![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/6970362beec7726bd5137a5732fd6c22e1f402cf/images/namp%20def.png)

  

### Step 2: Finding a Faulty Lock (Enumeration & CVE)
I navigated to the lab IP `172.17.0.2` to inspect the site.
* **Action:** I used `WPScan` to act as an inspector and list installed plugins.
* **Discovery:** I found a plugin called **Pie Register (v3.7.1.4)**.
* **Research:** I searched Google and found a public security flaw: **CVE-2025-34077**. A CVE is like a "faulty factory part" ID for software.


  

### Step 3: Stealing the "VIP Wristband" (Exploitation)

Once the flaw was identified, I didn't need to guess passwords.
* **Action:** I used an **exploit** from GitHub to target that specific error.
* **Result:** The system got confused and handed over the **Administrator's session cookies**.
* **Analogy:** A cookie is like a VIP wristband at a club; if you have it, the bouncer (the web) doesn't ask for ID—it just lets you in.


  

### Step 4: Entry without a Password (Cookie Injection)
* **Action:** Using the browser's developer tools (Storage), I pasted the stolen cookies.
* **Result:** Upon refreshing the page, I was logged in as the **Administrator**. I went from a visitor to the site owner without typing a single password!


  

### Step 5: Creating a Secret Tunnel (Reverse Shell)
Now I wanted to move from the "reception" to the "engine room" (the server).
* **Action:** I installed the **WP File Manager** plugin to access all site files.
* **The "Bingo" Moment:** I used `revshells.com` to generate a malicious PHP code, pasted it into `index.php`, and set up a listener with `Netcat` on port 4444.
* **Result:** The server connected to my machine, giving me a **Reverse Shell** console to control it.


  

### Step 6: The Final Checkmate (Privilege Escalation)
Inside the server's "guts," I looked for the ultimate prize: Root access.
* **Discovery:** In `/opt/bigwear/backend/settings.py`, I found a critical human error: **Plaintext credentials** (`pepe:BigWear2024!@#`).
* **Root Access:** I tested those credentials with `su root`. Because of **password reuse**, it worked!
* **The Loot:** The server was 100% compromised. I accessed the Django admin panel on port 3000, exposing customer database records and credit card details.


  

---

## Final Conclusion
This lab proved that a single oversight—an outdated plugin or a reused password—is enough to expose thousands of people. Cybersecurity is a **methodology**, and understanding this logical process allows me to become the shield that organizations desperately need.

---
> **Disclaimer:** This lab was conducted in a controlled environment for educational purposes as part of a Cybersecurity course.
