### Cybersecurity Incident Report: Network Traffic Analysis
___

Part 1:<br>Provide a summary of the problem found in the DNS and ICMP traffic log

- The UDP protocol reveals that:
  
- This is based on the results of the network analysis, which show that the ICMP echo reply returned the error message:
  
- The port noted in the error message is used for:
  
- The most likely issue is:
  
After receiving several complaints from clients regarding the website "yummyrecipesforme.com" producing the error "destination port unreachable" in their browser. Upon analysing the packets using wireshark, ICMP packets have been seen returning the error message "udp port 53 unreachable". Port 53 is the standard port for DNS and is the suspected cause of the unreachable website.
  

___
Part 2:<br>Explain your analysis of the data and provide at least one cause of the incident

- Time incident occurred:
  
- Explain how the IT team became aware of the incident:
  
- Explain the actions taken by the IT department to investigate the incident:
  
- Note key findings of the IT department's investigation(i.e., details related to the port affected, DNS server, etc.):
  
- Note a likely cause of the incident:

The time of the incident starting with the first initial packet is 13:24:32.1925571 local. The team was made aware by complaints from the client customers. This incident was assigned to me to further analyze the issue. Packets show an ICMP packet containing the error "udp port 53 unreachable". Unreachable typically indicates the UDP message requesting an IP address for the domain did not go through to the DNS server because no service was listening on the receiving DNS port. This is the most likely cause at time of analysis. 