[Part 1](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab1.md) [Part 2](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab2.md) [Part 3](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab3.md) [Part 4](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab4.md) [Part 5](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab5.md) [Part 6](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab6.md) [Part 7](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab7.md)

# For the Network Threat Hunting VM

During these parts you will be going through 7 datasets and you will have to answer some questions for each, you will find the answers if you keep scrolling down

You can view all of them via
```bash
rita list
```
<img width="657" height="203" alt="image" src="https://github.com/user-attachments/assets/6599e66c-6678-42c5-bec7-bbabd1043bf9" />

## rdp_ms_dev_tunnels

**Dataset description:** Inbound RDP connection tunnelled via outbound MS Dev Tunnels, tcp 443
```bash
rita view rdp_msdt
```

1. Which external IP is contacted with the rare signature ACMS/1.0, generating nearly 35,000 connections?

 
2. One High-severity entry shows a Beacon Score of 0.23, despite a long connection duration of over 85,000 seconds. What FQDN was contacted?

 
3. Which entry shows a single connection moving over 300,000,000 bytes of data to a Microsoft domain (*.visualstudio.com)? Provide the FQDN.

 
4. An entry communicates with the same suspicious IP (168.63.129.16) but on a non-standard port (32526). What modifier is attached to this connection?

 
5. Two None-severity flows go to unusual external IPs (143.198.3.13 and 70.24.242.145) on strange ports (8808 and 7707). What is the total connection count for the 143.198.3.13 entry?

 
6. At the bottom of the dataset, one entry shows communication with an external IP where the same destination port (443) is listed with both SSL and UDP. What is the destination IP?


<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

**1. Answer:** 168.63.129.16

**2. Answer:** prod-eastus.access-point.cloudmessaging.edge.microsoft.com

**3. Answer:** use-data.rel.tunnels.api.visualstudio.com

**4. Answer:** mime_type_mismatch

**5. Answer:** 375

**6. Answer:** 23.48.203.103

<br><br>

b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

---
[Back to the section](/courseFiles/Section_05-networkingAndTelemetry/networkingAndTelemetry.md)
