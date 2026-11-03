# Wireshark-SOC-Labs-Horoiheoma
Packet-level network traffic analysis labs completed as part of SOC analyst training, using Wireshark to investigate simulated incidents: port scans, TCP stream reconstruction, credential leaks over plaintext HTTP, brute-force login attempts, and call-scam traffic.

Each lab presents a short case and a live packet capture to investigate in a Wireshark-style browser environment but for this project we will focus more on the results of Lab 3: Identify the Attacker's IP

Scenario. You are a SOC analyst. A staff member has reported unusual traffic to your company’s web server. Capture the packets and analyze them in Wireshark. Find out what type of attack this is and what the attacker’s IP address is.

Lab 3: Identify the Attacker's IP

Scenario: A staff member reported unusual traffic to the company's web server. Captured and analyzed the traffic in Wireshark to determine the attack type and the attacker's IP.

Process and findings:

Step 1: Sign in to the Windows PC and capture the network traffic. How many packets did Wireshark capture? 	
Identify the Total packets captured	at the last column

Packets starting a TCP connection	use the frmular tcp.flags.syn == 1	

Attacker's IP	tcp.flags.syn == 1 && tcp.flags.ack == 0, sorted via Statistics → Conversations (IPv4)	192.168.--.--

Target IP	Same filter + ip.src == 192.168.--.--, Statistics → Conversations	192.168.--.--

Attacker's source port (constant across packets)	Inspected Info column	.One port number is the same in every packet. What is it? Use: tcp.flags.syn == 1  or Filter: tcp.flags.syn == 1 && tcp.flags.ack == 0 && ip.src == <the attacker's IP>
Scroll through the Info column and see which number stays constant

Distinct ports probed	tcp.flags.syn==1 && tcp.flags.ack==0 && ip.src==192.168.--.-- && ip.dst==192.168.--.--	

Open ports (SYN, ACK replies)	tcp.flags.syn==1 && tcp.flags.ack==1 && ip.src==192.168.--.-- && ip.dst==192.168.--.--	22, 80, 443

Closed-port reply flags	tcp.flags.reset==1 && ip.src==192.168.--.-- && ip.dst==192.168.--.--	RST, ACK

Closed ports	Count of RST, ACK replies	.tcp.flags.reset==1 && ip.src==192.168.--.-- && ip.dst==192.168.--.--
The count is in the status bar: Displayed: N. That N is the number of closed ports, assuming each port was probed once

Conclusion: This is a TCP SYN scan (half-open scan), consistent with a tool like Nmap's -sS. A single host (192.168.10.66) sent SYN packets from a fixed source port to 40 different destination ports on one target (192.168.10.20), without completing the three-way handshake on any of them — a reconnaissance technique that is stealthier and less likely to be logged by the target application than a full TCP connect scan

<img width="364" height="375" alt="6 wireshark labs abd 2 quizzes" src="https://github.com/user-attachments/assets/3a186f61-bafd-44fb-a383-b8b9c2fc8bec" />

<img width="668" height="308" alt="wireshark lab 3" src="https://github.com/user-attachments/assets/58faea31-3f80-4d10-8c08-dca0e3082905" />

<img width="513" height="304" alt="open ports" src="https://github.com/user-attachments/assets/cf5afacb-0887-428b-a0a9-8e435e165040" />

<img width="938" height="451" alt="attackers ip" src="https://github.com/user-attachments/assets/db6ed3c8-8f87-4bf9-b3ed-f25aaf7a9cf0" />

<img width="703" height="366" alt="40" src="https://github.com/user-attachments/assets/3c9c22fc-a2b5-4e70-b62f-63e353748946" />


