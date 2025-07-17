
I configured a new lab/VM with Windows 10 and downloaded the latest release of **Hayabusa** I could find. Being unfamiliar with this tool, I looked it up to understand what it does and found some useful commands. Here are the most helpful ones I discovered:

```
hayabusa.exe csv-timeline --file FILE.evtx -o timeline.csv               - Generate a timeline in CSV format
hayabusa.exe json-timeline --file FILE.evtx -o timeline.json             - Generate the same timeline in JSON format
hayabusa.exe logon-summary --file FILE.evtx                              - Show a summary of successful and failed logons
hayabusa.exe eid-metrics --file FILE.evtx                                - Display event ID statistics and breakdowns
hayabusa.exe log-metrics --file FILE.evtx                                - Show metadata like hostnames and event count
hayabusa.exe search --file FILE.evtx -k powershell                       - Search logs for the keyword "powershell"
hayabusa.exe search --file FILE.evtx -k mimikatz,pass-the-hash           - Search logs for multiple keywords
hayabusa.exe extract-base64 --file FILE.evtx -o decoded_output.txt       - Extract and decode Base64 strings from logs
hayabusa.exe config-critical-systems --file FILE.evtx                    - Detect domain controllers and key systems
hayabusa.exe pivot-keywords-list                                         - Show a list of keywords for pivot hunting
hayabusa.exe update-rules                                                - Update Hayabusa detection rules
```

Another useful tip I found is that Hayabusa supports the `--directory` option instead of `--file` to analyze a folder of logs.

I tried creating a `.evtx` file using **Sysmon** (which inspired the idea for this lab) to test Hayabusa, but reproducing an attack scenario proved to be quite complicated. Unfortunately, I wasn’t able to test all the commands or explore Hayabusa in depth.

Next, I will work on the actual lab.

---

## Reference

- [Hayabusa GitHub Repository](https://github.com/Yamato-Security/hayabusa)
- [Hayabusa Rules](https://github.com/Yamato-Security/hayabusa-rules)

