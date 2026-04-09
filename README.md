# Web-Penetration-Test
Exploiting WordPress vulnerabilities (CVE-2025-34077), authentication bypass via cookie injection, and privilege escalation to root. Part of my Cybersecurity Specialization.

#  Day 1: Hacking my First Website
## Lab Report: Exploiting the "BigWear" Server

   ![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/9e59c770bf03ed0d5bf05ecda92ba3617a0982e7/images/1.png) 

In this first session of my Cybersecurity journey, I moved away from the myths. I learned that you don't need to be a math genius or an expert programmer to identify security breaches; you need **creativity, logic, and a solid methodology.**


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

![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/acd6fb5a6710f257507b315c82f155f09dbef4fb/images/step%202.png)

* **Action:** I used `WPScan` to act as an inspector and list installed plugins.

![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/acd6fb5a6710f257507b315c82f155f09dbef4fb/images/step%202-1.png)

* **Discovery:** I found a plugin called **Pie Register (v3.7.1.4)**.

  ![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/acd6fb5a6710f257507b315c82f155f09dbef4fb/images/step%202%20%203.png)

  
* **Research:** I searched Google and found a public security flaw: **CVE-2025-34077**. A CVE is like a "faulty factory part" ID for software.

  ![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/acd6fb5a6710f257507b315c82f155f09dbef4fb/images/step%202%205.png)

  

### Step 3: Stealing the "VIP Wristband" (Exploitation)

Once the flaw was identified, I didn't need to guess passwords.

* **Action:** I used an **exploit** from GitHub to target that specific error.

![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/5acc12045bd3ed67f9f869bcd5b7c7105befee3e/images/step%203.png)

![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/5acc12045bd3ed67f9f869bcd5b7c7105befee3e/images/step%203%201.png)
  
* **Result:** The system got confused and handed over the **Administrator's session cookies**.

  ![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/5acc12045bd3ed67f9f869bcd5b7c7105befee3e/images/step%203%202.png)


I exploited CVE-2025-34077 in the Pie Register plugin to bypass authentication on the target WordPress site. By running this script, 
I successfully hijacked the admin session cookies, allowing me to log in as the administrator without a password. I then automated the process using Metasploit to confirm the vulnerability and gain full control over the web application.
  

  ![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/5acc12045bd3ed67f9f869bcd5b7c7105befee3e/images/step%203%203.png)

  ![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/5acc12045bd3ed67f9f869bcd5b7c7105befee3e/images/step%203%204.png)


* **Analogy:** A cookie is like a VIP wristband at a club; if you have it, the bouncer (the web) doesn't ask for ID—it just lets you in.

  


  

### Step 4: Entry without a Password (Cookie Injection)

* **Action:** Using the browser's developer tools (Storage), I pasted the stolen cookies.

![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/842f4d46f62eb661cc78313dc080f3c1b5a82bcb/images/step%205.png)
  
* **Result:** Upon refreshing the page, I was logged in as the **Administrator**. I went from a visitor to the site owner without typing a single password!

![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/842f4d46f62eb661cc78313dc080f3c1b5a82bcb/images/step%205%201%20.png)




  

### Step 5: Creating a Secret Tunnel (Reverse Shell)

Now I wanted to move from the "reception" to the "engine room" (the server).

* **Action:** I installed the **WP File Manager** plugin to access all site files.
  
![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/c86aa83b9b86651e945d7925637d8dec00999ba8/images/5-1.png)

![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/c86aa83b9b86651e945d7925637d8dec00999ba8/images/5-2.png)
  
* **The "Bingo" Moment:** I used `revshells.com` to generate a malicious PHP code, pasted it into `index.php`, and set up a listener with `Netcat` on port 4444.
* 

![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/c86aa83b9b86651e945d7925637d8dec00999ba8/images/5-3.png)




![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/c86aa83b9b86651e945d7925637d8dec00999ba8/images/5-4.png)
  
  
* **Result:** The server connected to my machine, giving me a **Reverse Shell** console to control it.


![images alt](https://github.com/salimelh94/Web-Penetration-Test/blob/c86aa83b9b86651e945d7925637d8dec00999ba8/images/5-5.png)



  

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
