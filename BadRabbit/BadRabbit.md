# BadRabbit Ransomware Attack: Cybersecurity Analysis Report

## Overview

On October 24, 2017, a new ransomware named **BadRabbit** rapidly spread throught parts of Eastern Europe, particularly targeting entities in Russia and Ukraine and slso affecting systems in Turkey and Germany as well. It was a social engineering attack disguised as an Adobe Flash installer that encypted files and demanded ransom in bitcoin.

On deeper analysis it was discovered that the **BadRabbit bore siilar properties as NotPetya** notably in its intent to to disrupt operations rather than demanding ransom. The malware utilized **SMB-based lateral movement**, credentials harvesting tools like **Mimikatz** and spread without relying on **Eternal Blue** setting it uniquely apart from predecessors.

## Tactics, Techniques, and Procedures (TTPs)

The BadRabbit actors executed a multi-phase attack with emphasis on social engineering, lateral movement, and file encryption:

### Initial Infection:

- **Tactic:**  Social engineering 

- **Technique:** Hosted fake Adobe Flash installers on legtimitate but compromised websites

- **Procedures:** Users were prompted to download a legitimate looking Flash update, which executed the malware payload upon installation


### Execution & Deployment

- **Tactic:**  Malware execution and system compromise

- **Technique:** Custom ransomware binary deployment (dispci.exe)

- **Procedures:** Files were encrypted using RSA-2048 + AES-128 encryption, boot configurations modified and ransom note displayed after forced reboot

### Credential Access

- **Tactic:** Credential harvesting

- **Technique:**  Memory dumping with Mimikatz 

- **Procedure:**  Extracted plain-text credentials from system memory to facilitate lateral movement

### Lateral Movement

- **Tactic:** Network propagation

- **Technique:** SMB exploitation and credential reuse

- **Procedures:** Used WMIC and PsExec to execute the malware across other systems in the domain

### Persistance

- **Tactic:** Scheduled execution

- **Technique:** Windows Task Scheduler abuse

- **Procedures:** Created scheduled task named **"rhaegal"** (Game of Thrones reference) to maintain presence

### Impact

- **Tactic:** Denial of access

- **Technique:** Encrypted files and hijacked system bootloader

- **Procedures:** Made machines inaccessible without decryption keys, although there was limited evidence of recovery even after ransom payment

## CIA Triad Components Compromised

- **Confidentiality** was less affected, although evidence shows the harvesting of credentials using **Mimiktaz**, there is no confirmed evidence of large-scale data exfiltration. However, internal credentials could have been potentially reused in future operations.

- **Integrity** suffered a catastrophic blow as system files were encrypted and tampered. Also unauthorized changes were made to bootloaders and schedulers.

- **Availability** was compromised as organizations faced temporary outages due to device going into locked state by forced reboot, displaying a ransom note.

## Exploited Vulnerabilities

BadRabbit took advantage of human error and network misconfiguration, rather than zero-day vulnerabilities

### Primary Weaknesses:

- **Social Engineering Susceptibility:** Users manually downloaded and ran the fake Flash installer, providing initial access

- **Lack of Execution Controls:** Organizations failed to restrict software execution policies and did not validate downloads

- **Credential Exposure:** Harvested domain credentials via Mimikatz, enabling rapid lateral spread across Windows environments

- **Unrestricted SMB Shares:** Misconfigured or overly permissive SMB shares facilitated malware propagation within internal networks

## Attack Motivation

### Surface Motive: Financial Gain

- Demanded 0.05 BTC ransom payments, appearing as typical ransomware

### True Intent: Political Disruption

- No verified file recovery after ransom payment
- Selective targeting of Russian media and Ukrainian infrastructure (Kiev metro, Odessa airport)
- Potential ties to Russian threat groups (GRU-affiliated APTs)
Used ransomware as cover for politically motivated cyber sabotage

##  Detection and Mitigation:

### Early Detection:

- Security vendors like Kaspersky and ESET quickly released indicators of compromise (IOCs).

- Some victims noticed the attack after sudden system reboots followed by ransom screens demanding 0.05 BTC.

### Emergency Mitigation Steps:

- **Access Blocking:** Infected websites hosting the fake installer were blacklisted and taken down

- **Password Resets:** Compromised credentials were reset across affected domains

- **SMB Hardening:** Network administrators disabled SMBv1 (if enabled) and restricted lateral movement

- **User Awareness:** Organizations issued internal alerts against downloading updates from unofficial sources

## Key Takeaways and Lessons Learned

- **Evolution of Ransomware:** Modern ransomware increasingly serves strategic objectives beyond financial gain

- **Social Engineering Effectiveness:** Even sophisticated defenses can be bypassed through user deception

- **Lateral Movement Without Exploits:** Legitimate administrative tools can facilitate rapid network propagation

- **Credential Hygiene Critical:** Poor credential management enables devastating lateral movement

- **Recovery Uncertainty:** Paying ransoms provides no guarantee of data recovery, especially in politically motivated attacks

## Conclusion

**BadRabbit** introduced a shift in ransomware attacks from demanding payments to cyber sabotage disguised as financial gain. This attack shows that modern ransomware often serves political purposes rather than just making money. Organizations need to focus on user training, managing user privileges, and preventing attackers from moving through their networks.

As attacks evolve the defense mechanisms implemented should also be modified as to counter the even evolving nature of attack techniques.