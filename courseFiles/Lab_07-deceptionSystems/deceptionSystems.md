# Deception Systems
The goal of deception technology is to draw cybercriminals to a trap or decoy instead of an organization's real assets. In order to fool the criminal into thinking they have infiltrated and obtained access to the company's most valuable assets when they haven't, the decoy imitates authentic servers, apps, and data. The tactic is used to reduce harm and safeguard an organization's real assets

Organizations rarely use deception technology as their main cybersecurity tactic. Protection against all unauthorized access is the aim of any security posture, and once a suspected breach has occurred, deception technology can be a helpful tactic to have in place. Protecting the company's actual assets may depend on directing the cybercriminal to phony information and login credentials

Research is another advantage of deceptive technology. IT security analysts can gain a thorough understanding of cybercriminals' behavior by examining how they breach the security perimeter and try to steal what they perceive to be authentic data. In fact, some organizations deploy a centralized deception server that records the movements of malicious actors—first as they gain unauthorized access and then as they interact with the decoy. The server records and tracks every vector utilized during the attack, giving the IT staff useful information to improve security and stop future occurrences of the same kind of attack

The goal of implementing deception isn't to replace anything, as most think, but to increase the attack time, as the main rule of thumb in SOC and IR is **Detetection time + Response time < Attack time** always if you want to stand a change against attackers

Deception technology must be invisible to an organization's workers, subcontractors, or clients in order to operate effectively

[MITRE Engage Matrix](https://engage.mitre.org/matrix/) - A valuable framework for designing deception and adversary engagement strategies

## Intro to Deception Techniques
Deception techniques fall into several categories:
- **Decoy Systems (Honeypots & Honeynets)** - Simulated systems designed to attract attackers, may imitate production servers or endpoints ***(Modern Honey Network)***
- **Honeytokens** - Fake credentials, files, or data planted in real systems, trigger alerts when accessed or exfiltrated ***(Canarytokens, Thinkst Canary)***
- **Deceptive Credentials** - Planted usernames/passwords that, if used, signal compromise or credential stuffing ***(HoneyUsers)***
- **Breadcrumbs** - Registry entries, mapped drives, or browser history designed to mislead attackers deeper into decoy environments ***(HoneyCreds)***

The objective is to shape adversary behavior and gather intelligence without tipping them off, or even to inhibit them from attacking you if they realize you may have active defence measures

## Generating Traps
When designing traps
- **Blend with Your Environment** - Decoys must match the organization’s OS versions, naming conventions, and service stacks
- **Deploy with Intent** - Traps should be placed in high-value areas like DMZs, internal network segments, or cloud environments
- **Track Interaction** - Use tools to log every keystroke, file access, lateral movement, and privilege escalation
- **Enable Real-Time Alerts** Alerts must integrate with your SIEM/SOAR platform for immediate response

Examples
- Deploying a fake domain admin account in Active Directory
- Planting honeyfiles labeled "Payroll_Q3.xlsx" on shared drives
- Creating hundreds of users with passwords from known wordlists to throw off **password sprays**
- Make fake Admin users with **0 Logon Hours**

## Active Defense & Cyber Deception
Active defense shifts from passive monitoring to proactive engagement, it includes
- **Tarpitting** - Slowing attacker tools and scripts using low-interaction decoys ***(Dionaea)***
- **Misattribution** - Feeding attackers false information ***(PortSpoof, SpiderTrap)***
- **Adversary Engagement** - Actively observing and sometimes manipulating adversaries in controlled environments ***(Thinkst Canary)***

By upsetting adversary decision cycles and making them doubt the reliability of their information and actions, these tactics enhance conventional defenses

***
<b><i>Continuing the course?</b>
</br>
[Click here for the Next Lab](/courseFiles/Lab_08-emailFundamentals/emailFundamentals.md)</i>

<b><i>Want to go back?</b>
</br>
[Click here for the Previous Lab](/courseFiles/Lab_06-browserAndCloudSecurity/browserAndCloudSecurity.md)

<b><i>Looking for a different lab? </b></br>[Back to Lab Directory](/coursenavigation.md)</i>
