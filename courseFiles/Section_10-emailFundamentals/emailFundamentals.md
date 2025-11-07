# Email Fundamentals

Email remains one of the **most exploited vectors in cyberattacks**, especially for initial access. As a SOC Analyst, having a deep understanding of email-based threats and defenses is essential to protecting your organization.


## Identifying Phishing

Phishing is the most common form of email‑based attack. Attackers impersonate trusted senders, trick users into clicking malicious links, or open dangerous attachments.

Phishing is the most common form of email‑based attack. Attackers impersonate trusted senders, trick users into clicking malicious links, or open dangerous attachments.

### 1.1 Sender Spoofing & Display Name Tricks

- **Spoofing**: faking the “From” address or display name.  
- **Key signs**:  
  - Display name shows `CEO`, but the actual address is `ceo@evil.com`.  
  - Domains with look‑alikes: `micr0soft.com`, `microsoft-suppport.net`.  
- **Best practices**:  
  - Always expand and inspect the full email address.  
  - Use email clients or plugins that highlight mismatches.

### 1.2 Mismatched or Suspicious URLs

- **Hover (desktop) or long‑press (mobile)** on links to reveal the true target.  
- **Red flags**:  
  - URL shorteners (bit.ly, t.co) used without context.  
  - IP‑based links: `http://192.168.1.5/login`.  
  - Subtle typos: `google‑security.co` vs. `google.com`.  
- **Example**:
  ```text
  Displayed: https://accounts.google.com
  Actual:    http://login.g00gle-secure.com
  ```

### 1.3 Unusual Attachments & File Types

- **High‑risk file extensions**:  
  - Executables/scripts: `.exe`, `.js`, `.vbs`, `.hta`, `.scr`  
  - Disk images: `.iso`, `.img`  
  - Macro‑enabled Office: `.docm`, `.xlsm`  
  - Archives hiding payloads: `.zip`, `.rar`  
- **Double extensions**: `invoice.pdf.exe`  
- **Best practice**: NEVER enable macros or run executables from untrusted emails. Always sandbox‑test unknown attachments.

---

## Common Email-Based Attacks

### 2.1 Business Email Compromise (BEC)

- **Definition**: A social engineering scam where attackers impersonate executives or partners to request wire transfers or data.  
- **Characteristics**:  
  - Plain‑text email with no malicious link or attachment.  
  - Urgency (“Wire transfer today!”) and familiarity (“Thanks, John”).  
- **Defense**: Out‑of‑band verification (phone call, in‑person) for any financial request.

### 2.2 Spear Phishing & Whaling

- **Spear phishing**: Highly targeted to individuals or departments using personalized details.  
- **Whaling**: Spear phishing aimed at high‑value targets (C‑suite, finance heads).  
- **Tactics**:  
  - Incorporation of personal or corporate data (recent projects, names).  
  - Use of legal or official language and letterhead.  
- **Warning signs**:  
  - Unusual sender for the target’s role.  
  - Requests for confidential reports or accounts.

### 2.3 Attachment‑Based Malware Delivery

- **Vectors**:  
  - PDF with embedded JavaScript.  
  - Word/Excel with malicious macros.  
  - Compressed archives containing droppers.  
- **Common payloads**: Emotet, LokiBot, ransomware families.  
- **Mitigation**:  
  - Sandbox‑detonate all attachments.  
  - Employ static analysis tools to inspect macros and scripts.

---

## Email Security Solutions

### 3.1 Secure Email Gateways (SEGs)

- **Role**: First line of defense for inbound and outbound mail.  
- **Techniques**:  
  - Reputation‑based filtering (senders, URLs).  
  - Sandbox analysis of attachments and links.  
  - URL rewriting and real‑time threat lookups.  
- **Popular products**: Proofpoint, Mimecast, Cisco Email Security.

### 3.2 Email Authentication Protocols

| Protocol  | Purpose                        | Mechanism                                                        |
| --------- | ------------------------------ | ---------------------------------------------------------------- |
| **SPF**   | Authorize sending IPs          | DNS record lists permitted mail servers; recipient checks IP.   |
| **DKIM**  | Verify message integrity       | Sender signs headers; recipient verifies via DNS public key.     |
| **DMARC** | Enforce policy & reporting     | Receiver applies policy on SPF/DKIM failures; sends reports.     |

#### 3.2.1 Protocol Flow

```text
+----------+   SPF Check   +--------------+   DKIM Verify   +-------------+
| Sender   |  ---------->  | Recipient’s  |  ---------->    | DMARC Eval  |
| Server   |               | Mail Server  |                 | & Reports   |
+----------+               +--------------+                 +-------------+
```


- **SPF** ensures the sending IP is authorized.  
- **DKIM** confirms content authenticity.  
- **DMARC** dictates actions (none/quarantine/reject) and aggregates reports.

#### 3.2.2 Validation Tools

- `dig txt example.com` / `nslookup -type=TXT example.com`  
- Web checkers: MXToolbox, dmarcian, InboxAlly.


---

##  Email Header Parsing Script


Use Python to extract and examine critical header fields:

```python
from email import policy
from email.parser import BytesParser

def analyze_headers(file_name):
    try:
        with open(file_name, 'rb') as f:
            msg = BytesParser(policy=policy.default).parse(f)

        print("\n--- Email Header Analysis ---\n")
        print("From:                 ", msg['From'])
        print("To:                   ", msg['To'])
        print("Subject:              ", msg['Subject'])
        print("Date:                 ", msg['Date'])
        print("Return-Path:          ", msg['Return-Path'])

        print("\nAuthentication-Results:")
        print("  ", msg.get('Authentication-Results', 'None'))

        print("\nAll Received Headers:")
        for h in msg.get_all('Received', []):
            print("  ", h)

    except FileNotFoundError:
        print(f"Error: File '{file_name}' not found.")
    except Exception as e:
        print(f"An error occurred: {e}")

# Call the function with the .eml file name (same folder); Change YOUR_EML_FILE.eml with your actual .eml file name.

if __name__ == "__main__":
    analyze_headers('YOUR_EML_FILE.eml')

```

##  Check for Data Breaches

Use [Have I Been Pwned](https://haveibeenpwned.com/) to check if your email address or personal information has been part of a known data breach. This site provides details about which data (such as passwords, phone numbers, or addresses) may have been compromised.

If you receive an email that contains personal information and you find that the same data has been exposed in a breach (as listed on Have I Been Pwned), this is a major red flag. It strongly suggests that the email may be a phishing attempt using stolen data to appear more legitimate.

Always verify the source and content of such emails carefully.



##  References & Further Reading and Learning Materials


- [Cloudflare Learning: SPF/DKIM/DMARC explanations](https://www.cloudflare.com/learning/email-security/dmarc-dkim-spf/)
- [MITRE ATT&CK – T1566 (Phishing)](https://attack.mitre.org/techniques/T1566/)
- [KBI.Media: Five Ways to Identify Phishing Emails](https://kbi.media/five-ways-to-identify-phishing-emails/)
- [Imperva: Spear Phishing, Whaling](https://www.imperva.com/learn/application-security/spear-phishing/)
- [Valimail Blog: SEGs & Email Authentication](https://www.valimail.com/blog/secure-email-gateways-and-email-authentication-why-you-need-both-infographic/)


---

##  Summary Checklist

- [ ] Always inspect full sender address and display name.  
- [ ] Hover or long‑press links; watch for typos or shortened URLs.  
- [ ] Treat unexpected attachments as malicious; sandbox before opening.  
- [ ] Verify any financial or sensitive request out‑of‑band.  
- [ ] Ensure your organization’s SPF, DKIM, and DMARC are correctly configured.  
- [ ] Use SEGs to filter and sandbox inbound email traffic.  
- [ ] Automate header analysis with scripts to catch anomalies quickly. (optional) 



## Projects to Try

Check out the [phishing_email_lab.md](./phishing_email_lab.md) and also the [solution](./phishing_email_lab_solution.md)

***
<b><i>Continuing the course?</b>
</br>
[Click here for the Next Section](/courseFiles/Section_11-malwareForensics/malwareForensics.md)</i>

<b><i>Want to go back?</b>
</br>
[Click here for the Previous Section](/courseFiles/Section_09-forensicsFundamentals/forensicsFundamentals.md)

<b><i>Looking for a different section? </b></br>[Back to Section Directory](/coursenavigation.md)</i>
