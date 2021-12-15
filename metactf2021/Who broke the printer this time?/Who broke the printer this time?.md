# CTF Name
## Who broke the printer this time? (200pts)
### Description: 
>Malicious operators typically exploit unpatched vulnerabilities within target environments to gain initial access, escalate privileges, and more. What recent vulnerability have Conti ransomware operators exploited to run arbitrary code with SYSTEM privileges?
The flag format will be CVE-xxxx-xxxxx


### Step 1 - Understand the Problem:
This challenge is a topical one and is based around how earlier this year (2021), it was determined that the print spooler service on Windows operating systems had a vulnerability that allowed for RCE.



### Step 2 - Determine the vulnerability:
Given the knowledge of the vulnerability explained in the previous step, Google sleuthing was our decided method for determining the CVE that was used by the Conti ransomware.
By searching "conti ransomware cve", we found a [publication](https://media.defense.gov/2021/Sep/22/2002859507/-1/-1/0/CSA_CONTI_RANSOMWARE_20210922.PDF) by CISA that outlined the specific vulnerabilities used by the Conti ransomware.
Since the challenge name was "Who broke the printer this time?", we had high suspicion that the vulnerability being asked for would be related to the Print Spooler service. In the publication, PrintNightmare was listed as the vulnerability used by the Conti ransomware to escalate privileges on the machine, since the Print Spooler service runs with administrative privileges.

You can learn more about the Conti ransomware 'playbook' that was translated by Cisco's Talos Intelligence Group here: https://blog.talosintelligence.com/2021/09/Conti-leak-translation.html

<details>
  <summary> Flag Spoiler </summary>
  CVE-2021-34527
</details>

## Learning Takeaways
Having an understanding of what vulnerabilties are being utilized to escalate privilege in a system is necessary to limit the attack surface of your network. Staying 'in the know' on what vulnerabilities are being exploited by threat actor groups will help system administrators patch their systems before an attack occurs on their network. Luckily, the vulnerability used in this challenge was widely reported on, so finding information on it came with relative ease, but that won't always be the case.
