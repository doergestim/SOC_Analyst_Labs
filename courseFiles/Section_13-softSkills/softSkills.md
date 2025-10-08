# Soft Skills
Soft skills are what will make you stand out as a SOC analyst, even while technical skills are what got you in. Developing the mentality, communication patterns, and teamwork that increase your efficacy in a real-world Security Operations Center is the main goal of this session. Asking more intelligent questions, outlining your research in detail, recording your findings, and making a significant contribution to your team are all skills you will acquire. You'll not only develop more quickly, but you'll also aid in the growth of those around you by forming the habits of curiosity, clarity, and constant learning.

## Ask Questions
### Why does it matter
SOC work is a mess, you may encounter log entries, notifications, or tools that you don't entirely understand

What distinguishes true analysts from button-pushers is curiosity.

### How to practice
When reviewing logs, pause and ask yourself
- What is this log source?
- What process generated this?
- Is this normal for this user/system?

Dig deeper instead of just closing an alert, ask why it happened, not just what happened

### For Example
You see this log line from an EDR:
<pre>powershell.exe -nop -w hidden -encodedCommand JABX...</pre>
Bad analysts ignore it, curious analysts
- Ask: “Why is PowerShell running encoded commands?”
- Look up: `-nop`, `-w hidden`, `-encodedCommandv`
- Investigate the decoded string
- Research the user and host behavior history

### Tools to assist
- MITRE ATT&CK for technique mapping
- `CyberChef` to decode strings



## Explain Your Process
### Why does it matter
You’ll frequently need to explain what you did — during handoffs, retrospectives, or escalations, being clear = being trusted

### How to practice
- Narrate during investigations on Zoom/Teams.
- Try structured thinking (like `What did I see?`, `What did I check?`, `What did I find?`, `What’s my conclusion?`)

### For Example
During a live incident review:
<pre>“We got a Defender alert about mimikatz.exe. I first confirmed the hash and PID from the EDR. I then checked network logs for lateral movement and found suspicious SMB traffic. I flagged the host for containment and started timeline analysis.”</pre>

### Tools to assist
- Templates for incident handoff notes
- **Jupyter Notebooks** for sharing steps with queries
- Visual aids (screenshots, diagrams)



## Start a Blog
### Why does it matter
Writing reinforces memory, a central notes repo helps you avoid re-Googling and shows initiative to others

### How to practice
Start small
- A GitHub repo with markdown files
- A Notion or Obsidian vault
- A public blog on Medium or GitHub Pages

Structure ideas
- “How I Investigated X”
- “My Notes on Zeek/Suricata/Splunk”
- “Alert of the Week: False Positive or Not?”

### For Example
Create SOC-Notebook on GitHub
<pre>/Windows_Event_Logs/4624_Logon_Types.md
/Splunk/Useful_Searches.md
/EDR_Analysis/MDE_Alert_Workflows.md</pre>

### Tools to assist
- Obsidian for personal markdown-based wiki
- GitHub or GitLab for versioned notes
- HackMD, Notion, or Jupyter for collaborative formats


## Learn & Teach
### Why does it matter
Not only are the top SOC analysts intelligent, but they also inspire others, provide clarity, and foster team trust

### How to practice
- Do a 5-minute "show and tell" each week on something new you learned
- Write internal playbooks or FAQs
- Mentor a new hire by walking through investigations together

### For Example
You learn a new Sigma rule format, don't keep it to yourself
- Post it in your SOC Slack with “New Trick: Custom Sigma Tuning”
- Explain when/why to use it
- Offer to help others tune their rules

### Tools to assist
- Internal Confluence/wiki for team knowledge sharing
- SOC Slack or Teams channel with a #tips thread
- Lunch & Learns or short talks



## Final Exercise 
### Pick any one alert or log entry from the last week and do the following
- Write out 3 questions you had about it
- Document the steps you took to understand it
- Write a brief blog-style post explaining what you learned
- Share the post (internally or externally) and invite feedback

---
>[!TIP]
>
>Your profession will be defined by how you think, communicate, learn, and teach, not by the tools and alerts that change, the analysts who advance the fastest aren't the most intelligent; rather, they're the most lucid, inquisitive, and cooperative


***
<b><i>Want to go back?</b>
</br>
[Click here for the Previous Section](/courseFiles/Section_11-documentationAndCaseNotes/documentationAndCaseNotes.md)

<b><i>Looking for a different section? </b></br>[Back to Section Directory](/coursenavigation.md)</i>
