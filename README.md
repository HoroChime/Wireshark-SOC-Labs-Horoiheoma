# 🔍 Wireshark-SOC-Labs-Horoiheoma

![Tool](https://img.shields.io/badge/Tool-Wireshark-blue)
![Focus](https://img.shields.io/badge/Focus-SOC%20Analysis-purple)
![Category](https://img.shields.io/badge/Category-Network%20Forensics-green)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 1. PROJECT OVERVIEW

This repository documents practical packet-level network traffic analysis exercises completed as part of my Security Operations Center (SOC) Analyst training.

Using Wireshark, I investigated simulated security incidents to understand network behaviour, identify suspicious communication patterns and interpret packet-level evidence.

The training covered:

- TCP port scanning.
- TCP stream reconstruction.
- Plaintext HTTP credential exposure.
- Brute-force login attempts.
- Suspicious call-scam traffic.

**Primary Focus:** Lab 3 – Identify the Attacker's IP.

## 2. LAB 3 – IDENTIFY THE ATTACKER'S IP

### Scenario

A staff member reported unusual network traffic directed at the company's web server.

As the SOC analyst, my task was to capture and investigate the network traffic using Wireshark, identify the suspected attack technique and determine the source and destination IP addresses.

### Investigation Objectives

1. Determine the total number of captured packets.
2. Identify TCP connection initiation packets.
3. Identify the suspected attacker's IP address.
4. Identify the target server's IP address.
5. Determine the attacker's source port.
6. Identify the distinct destination ports probed.
7. Identify open and closed ports.
8. Classify the observed network activity.

## 3. INVESTIGATION METHODOLOGY

### Step 1: Examine Captured Traffic

Opened the packet capture in Wireshark and examined the total number of captured packets displayed in the status bar.

**Total packets captured:** [Insert verified count]

### Step 2: Identify TCP SYN Packets

Applied the following display filter:

```wireshark
tcp.flags.syn == 1
```

To isolate initial TCP connection requests:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

These filters helped identify connection initiation attempts.

### Step 3: Identify the Attacker's IP Address

Applied the initial SYN packet filter and examined:

**Statistics → Conversations → IPv4**

Reviewed the source and destination addresses and connection distribution to identify the suspected scanning host.

**Attacker's IP:** [Insert verified IP]

### Step 4: Identify the Target IP Address

Examined the corresponding destination address in the IPv4 conversations.

**Target IP:** [Insert verified IP]

### Step 5: Identify the Attacker's Source Port

Applied:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0 && ip.src == <ATTACKER_IP>
```

Inspected the Info column and TCP details to identify the source port associated with the connection attempts.

**Attacker's source port:** [Insert verified port]

### Step 6: Identify Distinct Ports Probed

Applied:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 0 && ip.src == <ATTACKER_IP> && ip.dst == <TARGET_IP>
```

Reviewed the TCP destination ports to establish the number of distinct ports probed.

**Distinct destination ports:** [Insert verified count]

### Step 7: Identify Open Ports

Used the SYN-ACK response filter:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 1 && ip.src == <TARGET_IP> && ip.dst == <ATTACKER_IP>
```

The investigation identified the following open ports:

| Port | Common Service |
|---|---|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |

SYN-ACK responses indicated that these ports accepted the initial connection requests during the capture.

### Step 8: Identify Closed-Port Responses

Applied the TCP reset filter:

```wireshark
tcp.flags.reset == 1 && ip.src == <TARGET_IP> && ip.dst == <ATTACKER_IP>
```

Reviewed the RST responses to identify connection attempts rejected by the target.

**Closed-port responses:** [Insert verified count]

Where each probed port generated one corresponding reset response, the response count can be used to estimate the number of closed ports.

## 4. INVESTIGATION FINDINGS

| Investigation Item | Result |
|---|---|
| Total packets captured | To be confirmed |
| Suspected attack type | TCP SYN port scan |
| Attacker IP | To be confirmed |
| Target IP | To be confirmed |
| Attacker source port | To be confirmed |
| Distinct ports probed | To be confirmed |
| Identified open ports | 22, 80, 443 |
| Closed-port responses | To be confirmed |

### Attack Classification: TCP SYN Port Scanning

The observed pattern of multiple TCP SYN requests directed at different destination ports, followed by SYN-ACK and RST responses, is consistent with TCP SYN port scanning.

A SYN scan is a reconnaissance technique used to identify accessible network services.

The presence of scanning traffic alone does not establish malicious intent. Further investigation and correlation with authorized scanning activities would be required.

## 5. SOC ANALYST OBSERVATIONS AND LESSONS LEARNED

This exercise strengthened my practical understanding of:

1. Packet capture and network traffic inspection.
2. TCP three-way handshake behaviour.
3. SYN, ACK and RST flag interpretation.
4. Identifying suspicious connection patterns.
5. Source and destination IP investigation.
6. Port-state identification.
7. Wireshark display filters.
8. Network reconnaissance detection.
9. Interpreting packet-level evidence during incident investigations.

**Key takeaway:** Understanding network traffic at packet level is essential for identifying suspicious behaviour and supporting effective SOC investigations.

## 6. SCREENSHOTS AND INVESTIGATION EVIDENCE

The following screenshots document the practical training activities and findings from Lab 3.

### 6.1 Wireshark SOC Training Labs

Overview of the Wireshark training exercises and available practical labs.

![Wireshark SOC Labs](https://github.com/user-attachments/assets/3a186f61-bafd-44fb-a383-b8b9c2fc8bec)

### 6.2 Lab 3 – Identify the Attacker's IP

Evidence of the Lab 3 investigation scenario and packet analysis exercise.

![Lab 3 Investigation](https://github.com/user-attachments/assets/58faea31-3f80-4d10-8c08-dca0e3082905)

### 6.3 Identified Open Ports

Screenshot showing the open-port investigation results.

![Open Ports](https://github.com/user-attachments/assets/cf5afacb-0887-428b-a0a9-8e435e165040)

### 6.4 Attacker IP Identification

Packet analysis evidence used to investigate the suspected attacker's IP address.

![Attacker IP](https://github.com/user-attachments/assets/db6ed3c8-8f87-4bf9-b3ed-f25aaf7a9cf0)

### 6.5 Closed-Port Count

Evidence showing the closed-port response analysis.

![Closed Port Count](https://github.com/user-attachments/assets/3c9c22fc-a2b5-4e70-b62f-63e353748946)

## 7. TRAINING MATERIALS AND REFERENCES

The following resources supported the practical exercises:

- **Lab Recording:** [Watch Wireshark Packet Analysis Training](https://drive.google.com/file/d/1H-c9OUkjqud3hPj6JWMBfc70uY0k0mW1/view?usp=sharing)
- **Lab Documentation:** [View Training Document](https://docs.google.com/document/d/1I0YFlQvh6wmYx6shr05fdiO7kzZ6yD1f7ASihZUEcWs/edit?tab=t.0)
- **Wireshark Official Website:** [https://www.wireshark.org/](https://www.wireshark.org/)

## 8. CONCLUSION

This practical exercise provided valuable exposure to packet-level network traffic analysis using Wireshark.

By examining TCP connection requests, SYN-ACK responses and TCP reset flags, I developed a better understanding of how network reconnaissance activities can be identified through packet captures.

The investigation also demonstrated the importance of source and destination IP analysis, port identification and traffic interpretation in Security Operations Center activities.

One major lesson from this exercise is that effective SOC analysis goes beyond identifying suspicious traffic. It requires understanding network protocols, interpreting evidence, validating observations and documenting findings accurately.

**Every packet tells a story. The SOC analyst's responsibility is to understand it.**

---

## AUTHOR

**Horoiheoma Lazarus Chimezie**

Cybersecurity Enthusiast | SOC Analyst Training | Network Security

**Project:** Wireshark SOC Labs  
**Focus:** Network Traffic Analysis and Incident Investigation

*Learning by doing. Investigating traffic. Building defensive security skills.*
