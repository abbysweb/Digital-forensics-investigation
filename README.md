# End-to-End Digital Forensics Investigation: A Practical Tutorial

Welcome to this hands-on digital forensics project! This repository contains a comprehensive, court-ready forensic investigation that spans three major domains of digital forensics: **Disk Forensics**, **Volatile Memory (RAM) Forensics**, and **Mobile Device Forensics**.

If you are a student, cybersecurity enthusiast, or aspiring forensic investigator, this project serves as a practical tutorial on how to approach a multi-faceted digital crime scene.

## 📄 Full Investigation Report
**[👉 Click here to read the full 49-page Forensic Investigation Report (PDF)](./End-to-End%20Digital%20Forensics%20Investigation%20-%20Disk,%20Memory,%20Malware%20&%20Mobile%20Forensics.pdf)**

---

## 🎯 Project Objective
The objective of this investigation is to triage and analyze a compromised system belonging to a suspect ("Peter"). The goal is to uncover evidence of corporate espionage (intellectual property theft), active malware infections (RATs), and communications related to an illicit stolen vehicle trafficking ring.

---

## 🛠️ Tools Required
To replicate or understand the methodologies used in this report, you should be familiar with the following industry-standard tools:
- **Autopsy (v4.19)**: An open-source digital forensics platform used for deep disk analysis, file carving, and registry parsing.
- **Volatility 3 (v2.4)**: The leading framework for volatile memory (RAM) extraction and process analysis. (Requires Python 3)
- **Cellebrite Reader**: A robust, industry-standard tool used for parsing complex UFDR mobile extractions and visualizing communication graphs.
- **John the Ripper**: A password cracking tool used to crack extracted NTLM hashes.

---

## 🔍 Investigation Workflow (Tutorial)

This project follows a strict forensic workflow that adheres to ACPO and SWGDE digital evidence guidelines.

### Phase 1: Disk and Malware Analysis (Autopsy)
**Goal:** Identify the operating system, locate hidden malware, and find evidence of data exfiltration on the physical hard drive.
1. **Acquisition:** Mount the suspect's physical hard drive (E01 format) into Autopsy as a read-only forensic image.
2. **System Analysis:** Use registry hives (`SOFTWARE` and `SYSTEM`) to identify the OS version (Windows 7) and the suspect's SID.
3. **Malware Hunting:** Search for files with hidden attributes in unusual directories (e.g., `AppData`). Analyze timestamps and compare file hashes against databases like VirusTotal. In this case, a Remote Access Trojan (RAT) was discovered maintaining persistence via Registry Run keys.
4. **Data Exfiltration:** Search for proprietary files (e.g., `Snipper` project documentation) located outside of authorized user directories, proving data theft.

### Phase 2: Volatile Memory / RAM Forensics (Volatility 3)
**Goal:** Analyze a live memory capture (`physmem.raw`) to uncover running malicious processes, network connections, and extract system passwords.
1. **Integrity Check:** Always start by generating a SHA-256 hash of your `.raw` dump to ensure evidence integrity.
2. **Process Analysis:** Run `windows.pslist` to view all active processes. Look for anomalies, such as `tor.exe` running unexpectedly.
3. **Network Connections:** Use `windows.netstat.NetStat` to view active connections. Cross-reference established IPs to identify Command and Control (C2) servers.
4. **Credential Extraction:** Use the `hashdump` plugin to extract NTLM password hashes from memory. Crack them using John the Ripper (e.g., `john --format=NT hash.txt --wordlist=rockyou.txt`).

### Phase 3: Mobile Device Forensics (Cellebrite)
**Goal:** Parse a smartphone extraction (`.ufdr`) to reconstruct communication timelines and geolocation data.
1. **Media Analysis:** Review the media gallery for images of vehicles, OBD scanners, and cryptographic assets. Correlate EXIF metadata (timestamps, GPS coordinates) with the case timeline.
2. **Communication Logs:** Extract SMS and Signal messages. Look for coded language, meeting locations, and deleted messages. 
3. **App Usage:** Identify privacy-focused applications. In this case, the suspect used an application called "HideX" (disguised as a calculator) to conceal encrypted data and photos.
4. **Post-Arrest Activity:** Check the device's web search history and audio recording timestamps to determine if the device was accessed or improperly handled *after* the suspect's arrest.

---

## ⚖️ Evidence Correlation & Conclusion
The final step in any forensic tutorial is tying the evidence together into a unified narrative. 
- The **RAM dump** proved live Dark Web (`tor.exe`) activity.
- The **Disk image** proved corporate data theft (`passwords.docx`).
- The **Mobile device** proved involvement in vehicle trafficking (VIN videos, Signal messages).

For a complete breakdown of the findings, timeline reconstructions, and threat severity assessments, please review the **[Full Forensic Report](./End-to-End%20Digital%20Forensics%20Investigation%20-%20Disk,%20Memory,%20Malware%20&%20Mobile%20Forensics.pdf)**.

---
*Created by Abdullah Al Mamun*
