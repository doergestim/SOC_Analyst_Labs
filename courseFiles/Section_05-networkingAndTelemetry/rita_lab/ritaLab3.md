[Part 1](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab1.md) [Part 2](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab2.md) [Part 3](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab3.md) [Part 4](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab4.md) [Part 5](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab5.md) [Part 6](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab6.md) [Part 7](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab7.md)

# For the Network Threat Hunting VM

During these parts you will be going through 7 datasets and you will have to answer some questions for each, you will find the answers if you keep scrolling down

You can view all of them via
```bash
rita list
```
<img width="657" height="203" alt="image" src="https://github.com/user-attachments/assets/6599e66c-6678-42c5-bec7-bbabd1043bf9" />

## icedid


**Dataset description :** Cobalt Strike, Delay 300 s, Jitter 99%, http 8080 ; ScreenConnect, Long connection, tcp 443 ; CSharp-Streamer Long connection, http 80
```bash
rita view icedid
```

1. Which internal host IP appears in this dataset?


2. Primary external IP\:port (Cobalt Strike-like)?


3. What is the beacon score for that entry?


4. Name one long-connection external IP observed.

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

**1. Answer:** 192.168.2.77

**2. Answer:** 143.198.3.13:8080

**3. Answer:** 0.859

**4. Answer:** 147.75.62.184(can be multiple answers)

<br><br>

Continue with [Part 4](/courseFiles/Section_05-networkingAndTelemetry/rita_lab/ritaLab4.md)

b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

---
[Back to the section](/courseFiles/Section_05-networkingAndTelemetry/networkingAndTelemetry.md)
