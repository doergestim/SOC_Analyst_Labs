[Part 1](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab1.md) [Part 2](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab2.md) [Part 3](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab3.md) [Part 4](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab4.md) [Part 5](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab5.md) [Part 6](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab6.md) [Part 7](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab7.md)

# For the Network Threat Hunting VM

During these parts you will be going through 7 datasets and you will have to answer some questions for each, you will find the answers if you keep scrolling down

You can view all of them via
```bash
rita list
```
<img width="657" height="203" alt="image" src="https://github.com/user-attachments/assets/6599e66c-6678-42c5-bec7-bbabd1043bf9" />

## beacon_jitter

**Dataset description:** Cobalt Strike — HTTP, Delay 30s, Jitter 25%
```bash
rita view beacon_jitter
```

1. Which internal host IP is contacting the FQDN?


2. What FQDN is being contacted?


3. What protocol and port are used?


4. What is the beacon score?


5. What is the connection count?



<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

**1. Answer:** 192.168.2.77

**2. Answer:** timeserversync.com

**3. Answer:** HTTP (tcp 80)

**4. Answer:** 0.99

**5. Answer:** 3283

<br><br>

Continue with [Part 3](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab3.md)

b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

---
[Back to the section](/courseFiles/Section_05-networkingAndTelemetry/networkingAndTelemetry.md)
