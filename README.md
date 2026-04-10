# SSH Brute-Force Detection & Analysis using Splunk and Python

# 📌 Project Overview

This project simulates a real-world Security Operations Center (SOC) investigation focused on detecting and analyzing SSH brute-force attacks using Linux authentication logs.

The workflow demonstrates how a SOC analyst can:

* Identify suspicious authentication patterns from raw logs
* Automate detection using Python
* Correlate and visualize attack activity using Splunk SIEM
* Interpret attacker behavior and generate actionable insights
--
# 🎯 Use Case

Detect and analyze high-frequency failed SSH login attempts targeting privileged Linux accounts (e.g., root) to identify brute-force and automated attack activity.

# 🛠️ Tools & Technologies
• Python – Log parsing & automation
• Splunk Cloud (SIEM) – Search, correlation, visualization
• Linux Logs – Authentication log dataset
• VS Code – Manual log analysis

# 📂 Dataset
• Source: LogHub Linux Dataset (GitHub)
• Log Type: Linux authentication logs (auth.log equivalent)
• Contains: SSH login attempts, failures, invalid users, remote hosts (rhost)

# 🔍 Detection Approach
1️⃣ Manual Log Analysis

Initial inspection of logs revealed repeated patterns:

"authentication failure"
"Failed password"
"Invalid user"
"user unknown"

These are strong indicators of:

• Unauthorized access attempts
• Credential brute-force attacks
• Username enumeration

 2️⃣ Python Automation

A Python script was developed to automate detection of suspicious entries.

Key Functions:
• Reads raw log file
• Filters authentication-related events
• Extracts suspicious patterns
• Outputs structured results

Example Logic:
if "authentication failure" in line:
    suspicious_entries.append(("Auth Failure", line.strip()))

✅ Outcome:

• Reduced manual effort
• Structured dataset for analysis
• Faster identification of attack patterns

 3️⃣ SIEM Analysis (Splunk)
• 🔎 Base Search Query
source="Linux_2k.log" sourcetype="Linux"
("Failed password" OR "authentication failure" OR "Invalid user" OR "user unknown")

• 📊 Detection Query (Brute-Force Identification)
| bucket _time span=5m
| stats count by _time, rhost
| where count > 20

📌 Purpose:
Detects potential brute-force attacks where a single IP exceeds 20 failed login attempts within 5 minutes

• 📈 Time-Series Visualization
| timechart span=5m count by rhost

📌 Used to identify:

Attack spikes, 
Burst patterns, 
Automated behavior

# 📊 Key Findings
• 608 suspicious authentication events detected
• Root account was the most targeted user
• 80 failed login attempts from a single IP address
• Multiple remote hosts (rhost) involved → distributed attack pattern
• Authentication attempts occurred within seconds → automated activity
• Time-based analysis revealed clear spikes (attack bursts)

# 🌐 Indicators of Compromise (IOCs)
|Type	      |Value	               | Description
|IP Address	|150.183.249.110	|Highest brute-force attempts (80)
|IP Address	|207.243.167.114	|Secondary attacker
|Username	|root	|Primary target
Log Pattern	authentication failure	Brute-force indicator

# 🧠 Attack Analysis

Observed behavior indicates:

• Automated brute-force attack
• Password guessing & spraying techniques
• Username enumeration (invalid users)
• Distributed attack sources (possible botnet activity)

# 🧬 MITRE ATT&CK Mapping
|Technique	|ID
|Brute Force	|T1110
|Password Guessing	|T1110.001
|Password Spraying	|T1110.003

# 🚨 SOC Detection Use Case

Trigger Condition:

* More than 20 failed login attempts from a single IP within 5 minutes

SOC Response Actions:

• Block offending IP address
• Alert SOC team
• Investigate targeted accounts
• Enforce password reset if necessary


Example:
Splunk Timechart (Attack Spikes)
Top Attacking IPs
Query Results
Python Script Output
--
# 🛡️ Security Recommendations
🔒 Authentication Hardening
Disable direct root login (PermitRootLogin no)
Enforce strong password policies
Enable Multi-Factor Authentication (MFA)
#
🌐 Access Control
Restrict SSH access via firewall or VPN
Allow only trusted IP ranges
# 🚫 Attack Prevention
Deploy Fail2Ban or IPS solutions
Automatically block repeated failed login attempts
# 📡 Detection Improvements
Implement Splunk alerts for brute-force thresholds
Monitor abnormal login patterns
Integrate threat intelligence feeds
# 📉 Risk Assessment
Severity: HIGH
Impact: Potential full system compromise
Likelihood: High (based on automation and frequency)


# 🧠 Skills Demonstrated
Log analysis & threat detection
Python scripting for automation
SIEM (Splunk) investigation
Behavioral attack analysis
MITRE ATT&CK mapping
Security reporting & documentation

--
# 🚀 Why This Project Matters

This project demonstrates how a SOC analyst can:

Detect real-world brute-force attacks
Automate log analysis workflows
Use SIEM tools to identify attack patterns
Translate technical findings into actionable security insights
👨‍💻 Author

Abubakar Yusuf
SOC Analyst | Cybersecurity Enthusiast
