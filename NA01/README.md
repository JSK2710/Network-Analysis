# Malware Compromise

Challenge Link: https://blueteamlabs.online/home/challenge/network-analysis-malware-compromise-e882f32908

Completion Link: https://blueteamlabs.online/achievement/share/challenge/88598/11#

**Scenario**

A SOC Analyst at Umbrella Corporation is going through SIEM alerts and sees the alert for connections to a known malicious domain. The traffic is coming from Sara’s computer, an Accountant who receives a large volume of emails from customers daily. Looking at the email gateway logs for Sara’s mailbox there is nothing immediately suspicious, with emails coming from customers. Sara is contacted via her phone and she states a customer sent her an invoice that had a document with a macro, she opened the email and the program crashed. The SOC Team retrieved a PCAP for further analysis.

# **Network Analysis Report**

## **1. Report Information**

| Field | Details |
| --- | --- |
| **Report ID** | NA01 |
| **Analyst Name** | Jaya Sai Krishna Guttula |
| **Date of Analysis** | 11/11/2025 |
| **Environment** | Blue Teams Labs Online |
| **Tools Used** | Wireshark |

---

## **2. Objective**

A SOC Analyst at Umbrella Corporation is going through SIEM alerts and sees the alert for connections to a known malicious domain. The traffic is coming from Sara’s computer, an Accountant who receives a large volume of emails from customers daily. Looking at the email gateway logs for Sara’s mailbox there is nothing immediately suspicious, with emails coming from customers. Sara is contacted via her phone and she states a customer sent her an invoice that had a document with a macro, she opened the email and the program crashed. The SOC Team retrieved a PCAP for further analysis.

---

## **3. Network Overview**

| Parameter | Details |
| --- | --- |
| **Network Type** | WAN  |
| **Duration of Capture** | 42 min 03 sec |
| **Data Volume** | 1,211 kB |
| **Monitoring Tool** | Wireshark |

---

## **4. Methodology**

[Link](/Network-Analysis/NA01/Process)

---

## **5. Key Observations**

| Observation ID | Description | Source IP | Destination | Protocol | Severity |
| --- | --- | --- | --- | --- | --- |
| OBS-001 | DNS query for domain `klychenogg[.]com` | 10.11.27.101 | 10.11.27.1 | DNS | High |
| OBS-002 | HTTP GET request `/QIC/tewokL.php?L=spet10.spr` | 10.11.27.101 | 95.181.198.231 | HTTP | Critical |
| OBS-003 | Downloaded suspicious `.rar` file `oiioiashdqbwe.rar` | 10.11.27.101 | 95.181.198.231 | HTTP | Critical |
| OBS-004 | Outbound TLS connections to 185.244.150.230 (C2 IP) | 10.11.27.101 | 185.244.150.230 | TLS | Critical |

---

## **6. Traffic Statistics**

| Metric | Value |
| --- | --- |
| **Total Packets Captured** | 2053 |
| **Unique IPs Detected** | 10 |
| **Top External IP** | 95.181.198.231 |
| **Most Used Protocols** | TCP (99.2%), UDP (0.8%), DNS(0.8%), HTTP (0.6%) |

---

## **7. Suspicious Indicators (IOCs)**

| Type | Indicator | Description |
| --- | --- | --- |
| **Domain** | klychenogg[.]com | Command and control domain |
| **IP Address** | 95.181.198.231 | Host serving malicious payload |
| **IP Address** | 185.244.150.230 | Likely C2 communication |
| **File** | `oiioiashdqbwe.rar` | Malware archive downloaded |
| **File** | `spet10.spr` | Malicious script file triggering C2 traffic |

---

## **9. Findings Summary**

### **DNS Activity**

- Initial queries to `klychenogg[.]com` confirm domain resolution to `95.181.198.231`.
- Domain appears in multiple malware reports tied to **Dridex botnet infrastructure**.

### **HTTP Activity**

- HTTP GET request to `/QIC/tewokL.php?L=spet10.spr` indicates malware attempting to fetch configuration or stage payloads.
- `.rar` file download observed — consistent with Dridex’s delivery via compressed executables.

### **Encrypted TLS Sessions**

- Post-download, TLS 1.2 connections were established to `185.244.150.230`, consistent with **C2 beaconing**.
- Multiple “Client Hello” and “Encrypted Handshake Message” packets suggest persistent session attempts.

---

## **10. Recommendations**

| Area | Action | Priority |
| --- | --- | --- |
| **Containment** | Isolate host `10.11.27.101` immediately. | Critical |
| **Eradication** | Scan with updated EDR/AV for Dridex variants. | High |
| **Network Security** | Block IOCs: domains and IPs via firewall and DNS filters. | High |
| **User Awareness** | Conduct phishing and macro training for finance staff. | Medium |
| **Monitoring** | Add SIEM rules for `.rar` downloads and C2 TLS patterns. | High |

---

## **11. Conclusion**

The network traffic confirms an active **Dridex malware infection.**

The host 10.11.27.101 executed a malicious macro document, contacted a **malicious domain**, downloaded payloads, and established **TLS-encrypted sessions with known C2 servers**.

Immediate containment and malware eradication actions are recommended.

---

All the answers are defanged

## **Challenge Submission**

Q. What’s the private IP of the infected host? 

Ans: 10[.]11[.]27[.]101

Q. What’s the malware binary that the macro document is trying to retrieve? 

Ans: spet10.spr

Q. From what domain HTTP requests with GET /images/ are coming from? 

Ans: cochrimato[.]com

Q. The SOC Team found Dridex, a follow-up malware from Ursnif infection, to be the culprit. The customer who sent her the macro file is compromised. What’s the full URL ending in .rar where Ursnif retrieves the follow-up malware from? 

Ans: hxxp[://]95[.]181[.]198[.]231/oiioiashdqbwe[.]rar

Q. What is the Dridex post-infection traffic IP addresses beginning with 185.?

Ans: 185[.]244[.]150[.]230
