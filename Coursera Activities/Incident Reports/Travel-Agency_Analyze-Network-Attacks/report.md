## Cybersecurity Incident Report: Network Security
___

### Scenario
You work as a security analyst for a travel agency that advertises sales and promotions on the company’s website. The employees of the company regularly access the company’s sales webpage to search for vacation packages their customers might like. 

One afternoon, you receive an automated alert from your monitoring system indicating a problem with the web server. You attempt to visit the company’s website, but you receive a connection timeout error message in your browser.

You use a packet sniffer to capture data packets in transit to and from the web server. You notice a large number of TCP SYN requests coming from an unfamiliar IP address. The web server appears to be overwhelmed by the volume of incoming traffic and is losing its ability to respond to the abnormally large number of SYN requests. You suspect the server is under attack by a malicious actor. 

You take the server offline temporarily so that the machine can recover and return to a normal operating status. You also configure the company’s firewall to block the IP address that was sending the abnormal number of SYN requests. You know that your IP blocking solution won’t last long, as an attacker can spoof other IP addresses to get around this block. You need to alert your manager about this problem quickly and discuss the next steps to stop this attacker and prevent this problem from happening again. You will need to be prepared to tell your boss about the type of attack you discovered and how it was affecting the web server and employees.
___

### Wireshark TCP/ HTTP Log

Please see TCP_HTTP-Wireshark_Log.md for the log.
___

## Notes
- What do you currently understand about the network attacks?
- Which type of attack would likely result in the symptons described in this scenario?
- What is the different between a denial of service and distributed denial of service?
- Why is the website taking a long time to load and reporting a connection timeout error?

Currently, packets 52-62 show 2 different IPs following the TCP handshake [SYN -> SYN, ACK -> ACK] model. With a closer look, packets from IP 203.0.113.0 never achieve a HTTP connection. Only sends [SYN] and [ACK] packets. The other IPs do achieve a successful HTTP connection with the website. Analyzing beyond packet 62, the same suspicious IP continues to send [SYN]s and [ACK]s. This behavior is consistent with a SYN flood Denial of Service attack; an attack where a threat actor will only send a larger number of [SYN] packets than the server can handle. As the symptom of the webserver timing out, it's reasonable to suspect further analysis of the remainder packets will indicate a gradual decline in server response, until it completely stops responding. 

Packet 73 is the first indication the webserver timing out; this is a[RST, ACK] packet. After packet 121, it's now only the suspiscious IP sending [SYN] packets. This is the proof of a SYN Flood DoS attack. The server is no longer responding as it is overwhelmed by the attacking IP's SYN flood. 

___
### Incident Report
#### Section 1
**Identify the type of attack that may have caused this network interruption**

- One potential explanation for the website's connection timeout error message is:
- The logs show that:
- This even could be:

After the SIEM alert, a team has been dispatched to analyze the network anomoly using packet sniffing with Wireshark. During analysis, we see the logs initially show typical TCP handshakes with clients achieving an HTTP connection. Upon further analysis, it appears a recurring suspiscious IP 203.0.113.0 is consistently sending SYN packets. The logs show the server begins to send [RST, ACK] packets indicating a server time-out. After that, it's only the suspiscious IP sending [SYN] packets. This symptoms and behavior is consistent with a SYN Flood DoS attack. As it's only 1 IP, this is not a DDoS (Distributed Denial of Service) attack.

___

#### Section 2
**Explain how the attack is causing the website to malfunction**

When website visitors try to establish a connection with the web server, a three-way handshake occurs using the TCP protocol. Explain the three steps of the handshake:

1. Client device sends a [SYN] packet to begin the TCP handshake
2. The server will send back a [SYN, ACK] packet to authorize that client
3. The client device then sends a [ACK] packet to confirm or "Acknowledge" the authorization to establish an HTTP connection and proceed with requesting web pages

Explain what happens when a malicisous actor sends a large number of SYN packets all at once:

When a malicisous actor sends a a higher number of [SYN] packets in a short span of time than the server can handle, it will no longer be able to respond to legitamte site visitors and time-out. 

Explain what the logs indicate and how that affects the server:

The logs indicate a consistent suspiscious IP sending a large number of only [SYN] packets in a very short span of time. There are a few legitimate clients attemtping to connect. As the server's performance begins to degrade, [RST, ACK] packets are sent by the server indicating a time-out. The server is now unable to respond tim legitimate clients attempting to establish a connection. 

A security team is currently taking the affected server offline to update firewall rules to block the offending IP and further implement new policies to block new suspiscious IPs from being able to perform a SYN Flood DoS or DDoS attack. 
___