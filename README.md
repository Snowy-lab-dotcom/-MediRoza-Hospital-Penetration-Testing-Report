# 🔐 MediRoza Hospital — Penetration Testing Report

![Cybersecurity](https://img.shields.io/badge/Field-Cybersecurity-red)
![Assessment](https://img.shields.io/badge/Assessment-Penetration%20Testing-blue)
![Environment](https://img.shields.io/badge/Environment-Authorized-green)
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-lightgrey)

---

## 📌 Assessment Information

| Item | Details |
|---|---|
| **Target** | MediRoza Hospital Web Application |
| **Assessment Type** | Web Application Penetration Testing |
| **Authorization** | Written Permission Granted |
| **Environment** | Controlled Educational Environment |
| **Program** | Networkwalks Cybersecurity |
| **Platform** | Kali Linux |
| **Report** | Milestone 4 — Detailed Penetration Testing Report |

> ⚠️ **Ethical Notice:** This assessment was conducted against an authorized target in a controlled educational environment. The techniques documented in this report must never be applied to systems without explicit written permission.

---

# Executive Summary

An authorized penetration test was conducted against the **MediRoza Hospital web application** to identify security weaknesses that could result in unauthorized disclosure of sensitive information.

The assessment included network and web reconnaissance, directory enumeration, web application testing, HTTP request analysis, PDF security testing, password recovery, and analysis of exposed server files.

The assessment identified three significant security issues:

1. **Insufficient access control exposed three confidential patient laboratory reports.**
2. **Weak PDF password protection allowed the protected reports to be opened after password recovery.**
3. **An exposed database backup disclosed highly sensitive employee salary and hospital shareholder information.**

The combined findings demonstrate weaknesses in access control, document protection, sensitive-file storage, and information exposure.

The overall security risk is considered **Critical** because the assessment ultimately demonstrated access to confidential medical, employee, financial, and corporate ownership information.

---

# Scope and Methodology

## Scope

Testing was limited to the authorized MediRoza Hospital environment and the activities required by the Networkwalks penetration-testing milestones.

### In Scope

- MediRoza Hospital web application
- Publicly accessible services
- Web directories and files
- Patient portal functionality
- HTTP requests and responses
- Laboratory PDF reports
- Exposed server resources
- Information discovered during authorized testing

### Out of Scope

- Denial-of-service attacks
- Destructive testing
- Modification or deletion of hospital information
- Systems outside the authorized target
- Public disclosure of confidential information

---

## Tools Used

| Tool | Purpose |
|---|---|
| **Nmap** | Port, service and entry-point discovery |
| **DNSRecon / nslookup** | DNS enumeration |
| **Whois** | Domain registration reconnaissance |
| **WhatWeb** | Web technology fingerprinting |
| **cURL** | HTTP request and response analysis |
| **WAFW00F** | Web Application Firewall detection |
| **Dirsearch** | Web directory and resource enumeration |
| **Burp Suite** | HTTP interception and application analysis |
| **Hash Calculator** | Extraction/analysis of PDF password hashes |
| **John the Ripper** | Password recovery testing |
| **HexStrike** | Supporting security analysis |
| **Kali Linux** | Penetration-testing environment |

---

## Methodology

The assessment followed the following workflow:

```text
Reconnaissance
      ↓
Network & Service Enumeration
      ↓
Web Technology Analysis
      ↓
Directory / Resource Enumeration
      ↓
Patient Portal Investigation
      ↓
Burp Suite HTTP Analysis
      ↓
Patient Report Discovery
      ↓
PDF Security Analysis
      ↓
Password Recovery
      ↓
Document Access
      ↓
Exposed Backup Discovery
      ↓
Sensitive Data Analysis
```

### Limitations

Testing was restricted to the authorized scope.

No denial-of-service or destructive testing was performed.

Some reconnaissance results produced blocked, filtered, redirected, or otherwise non-actionable responses and were therefore not treated as confirmed vulnerabilities.

Sensitive information has been redacted from the public version of this report.

---

# Findings and Proof of Exploitation

# Confidential Patient Laboratory Reports

## Insufficient Access Control to Confidential Patient Reports

### Description

The first milestone required the identification of three confidential patient laboratory reports.

Initial reconnaissance was performed to identify the target's externally accessible services and web application attack surface.

Nmap identified several exposed services and confirmed that HTTP/HTTPS services were available for further web application assessment.

### Evidence — Network Reconnaissance

<img width="482" height="62" alt="nmap for entry points" src="https://github.com/user-attachments/assets/7182ea48-ec64-47db-9c6f-ed3484629dd4" />

Further web reconnaissance was performed using tools including `nslookup`, `DNSRecon`, `WhatWeb`, `cURL`, and `WAFW00F`.

These activities helped establish the target's DNS configuration, HTTP behaviour, server technologies, and available web attack surface.

### Evidence — Web Reconnaissance

![WhatWeb](evidence/whatweb-1.png)

![WAFW00F](evidence/wafw00f.png)

---

## Directory and Resource Enumeration

Web content enumeration was subsequently performed using **Dirsearch**.

The enumeration identified several accessible resources, including an `/old/` directory, which returned a successful HTTP response and required further investigation.

### Evidence — Directory Enumeration

![Dirsearch Results](evidence/dirsearch.png)

Further investigation of the old resources identified an exposed backup associated with the application.

### Evidence — Old Backup Discovery

![Old Backup Discovery](evidence/find-old_backup.png)

---

## Patient Portal Investigation

The patient-facing application functionality was investigated as part of the search for the required laboratory reports.

**Burp Suite** was configured to intercept browser traffic generated while interacting with the patient portal.

### Evidence — Burp Suite Interception

![Burp Suite Intercept](evidence/burpsuite-intercept.png)

The application's HTTP history was then analysed to understand the requests generated by the patient functionality.

### Evidence — HTTP History

![Burp Suite HTTP History](evidence/Burpsuite-Http-history.png)

Burp Suite allowed the relevant requests and responses to be inspected while testing the application's access-control behaviour.

---

## Successful Report Discovery

Testing resulted in access to a patient laboratory-report page containing the **three PDF reports required by M1**.

### Evidence — Three Patient Reports

![Three Patient Reports](evidence/3-Patients-report.png)

This confirmed that confidential patient laboratory documents were exposed through the tested application workflow.

> ⚠️ Patient names, medical information and other personally identifiable information must be redacted before this evidence is published publicly.

---

## M1 Impact

The vulnerability could allow unauthorized access to confidential medical documents.

Successful exploitation could result in:

- Disclosure of patient medical information
- Exposure of personally identifiable information
- Loss of patient confidentiality
- Privacy and regulatory consequences
- Reputational damage to the hospital

---

# M2 — PDF Password Security

## M2-F1 — Weak Password Protection of Confidential PDF Reports

### Description

The laboratory reports obtained during M1 were protected by passwords.

Opening the reports resulted in a password prompt, confirming that PDF-level protection had been applied.

### Evidence — Password-Protected Report

![Report 1 Password Prompt](evidence/Report-1-Pass-prompt.png)

The PDF protection was then assessed to determine whether the passwords could resist password-recovery attempts.

---

## Hash Extraction

The password-protected PDF documents were processed using the available password-analysis tools.

The **Networkwalks Hash Calculator** was used during the analysis of the protected reports.

### Evidence — Report 1 Hash

![Report 1 Hash](evidence/Hash-Calc-Report-1.png)

### Evidence — Report 2 Hash

![Report 2 Hash](evidence/Hash-Calc-Report-2.png)

### Evidence — Report 3 Hash

![Report 3 Hash](evidence/Hash-Calc-Report-3.png)

---

## Password Recovery

Password recovery testing was subsequently performed against the extracted password information.

The evidence demonstrates successful password recovery for the protected reports.

### Evidence — Report 1 Password Recovery

![Report 1 Password](evidence/Report-1-Password.png)

### Evidence — Report 2 Password Recovery

![Report 2 Password](evidence/Report-2-Password.png)

Report 3 initially presented additional difficulty during password recovery.

### Evidence — Initial Report 3 Failure

![Report 3 Password Attempt](evidence/Report-3-Pass-Crack-Fail.png)

Further password recovery testing was performed using **John the Ripper**.

The tool installation/version was verified before the cracking process was performed.

### Evidence — John the Ripper Verification

![John the Ripper Version](evidence/JTR-version-check.png)

### Evidence — Password Recovery Process

![John the Ripper Password Recovery](evidence/JTR-Pass-Crack.png)

### Evidence — Successful Password Recovery

![John the Ripper Completed](evidence/JTR-Pass-Crack-Done.png)

---

## Successful Document Access

After recovering the required passwords, the protected laboratory reports could be opened.

### Evidence — Report 1 Opened

![Report 1 Opened](evidence/Report-1-Doc-Open.png)

### Evidence — Report 2 Opened

![Report 2 Opened](evidence/Report-2-Doc-open.png)

### Evidence — Report 3 Opened

![Report 3 Opened](evidence/Report-3-open.png)

This demonstrated that the passwords protecting the confidential medical documents were not sufficiently resistant to the authorized password-recovery testing.

> ⚠️ Passwords and confidential medical information should be redacted from the public GitHub evidence.

---

## M2 Impact

Weak document passwords reduce the effectiveness of PDF-level protection.

If an unauthorized party obtained the PDF files, recoverable passwords could allow access to confidential medical information despite the documents being password protected.

PDF passwords should therefore not be treated as a substitute for proper application-level access control.

---

# M3 — Critical Sensitive Data Exposure

## M3-F1 — Exposed Internal Database Backup

### Description

The third milestone required further analysis of information discovered during the previous phases.

Directory and file analysis identified an exposed historical database backup associated with the MediRoza Hospital application.

The exposed backup was identified as:

```text
mediroza_db_backup_2019.sql
```

The backup identified itself as an internal MediRoza Hospital database backup and contained confidential staff and shareholder records.

### Evidence — Backup Discovery

![Old Backup Discovery](evidence/find-old_backup.png)

Analysis of the exposed database contents revealed two particularly sensitive datasets:

1. Hospital employee information and salary data
2. Hospital shareholder information

---

## Employee Salary Exposure

The exposed `staff` database table contained employee information including:

- Employee name
- Job title
- Department
- Contact information
- Identification information
- Monthly salary
- Employment information

The assessment confirmed that salary records for **30 hospital employees** were present in the exposed backup.

### Evidence — Employee Salary Data

> 📸 **INSERT REDACTED SCREENSHOT OF THE STAFF/SALARY TABLE HERE**

Suggested filename:

```text
evidence/M3-02-salaries-redacted.png
```

> ⚠️ National identification numbers, telephone numbers, email addresses and other unnecessary personal information must be redacted from the public evidence.

---

## Shareholder Information Exposure

The same database backup contained a `shareholders` table.

The exposed information included:

- Shareholder names
- Ownership percentages
- Number of shares held
- Share class

The assessment identified **10 shareholder records** within the exposed database.

### Evidence — Shareholder Data

> 📸 **INSERT REDACTED SCREENSHOT OF SHAREHOLDER TABLE HERE**

Suggested filename:

```text
evidence/M3-03-shareholders-redacted.png
```

---

## M3 Impact

Exposure of an internal database backup represents a severe confidentiality failure.

The exposed information could potentially facilitate:

- Targeted phishing
- Social engineering
- Employee impersonation
- Financial fraud
- Identity-related abuse
- Targeting of senior employees
- Corporate intelligence gathering
- Further attacks against hospital personnel

The combination of personal employee information, salary information and corporate ownership information significantly increases the potential impact of the exposure.

---

# 04 — Risk Rating

| ID | Vulnerability | Risk | Justification |
|---|---|---|---|
| **M1-F1** | Insufficient Access Control to Patient Reports | 🔴 **Critical** | The application workflow resulted in access to confidential patient medical reports, directly compromising sensitive medical information. |
| **M2-F1** | Weak PDF Password Protection | 🟠 **High** | Password protection on confidential medical reports was successfully defeated during authorized password-recovery testing. |
| **M3-F1** | Exposed Internal Database Backup | 🔴 **Critical** | An accessible internal database backup exposed employee personal information, salaries and hospital shareholder records. |

## Overall Risk — Critical

The overall assessment is rated **Critical** because multiple vulnerabilities resulted in demonstrated access to highly sensitive patient, employee and corporate information.

The combination of inadequate application access controls, weak document protection and exposed internal database information significantly increases the potential impact of compromise.

---

# 05 — Recommendations and Remediation

## M1-F1 — Patient Report Access

The hospital should:

- Enforce server-side authentication and authorization for every patient report request.
- Verify ownership and authorization before returning any medical document.
- Prevent direct unauthenticated access to report files.
- Review patient portal session-management controls.
- Log and monitor access to medical reports.
- Perform regular access-control testing.

---

## M2-F1 — PDF Password Protection

The hospital should:

- Avoid relying on PDF passwords as the primary protection for medical information.
- Use strong, unique and sufficiently complex passwords where document encryption is required.
- Implement application-level authorization before allowing documents to be downloaded.
- Store medical documents outside publicly accessible web directories.
- Review existing protected documents for weak passwords.
- Apply appropriate encryption for sensitive data at rest.

---

## M3-F1 — Exposed Database Backup

The hospital should:

- Immediately remove database backups from web-accessible directories.
- Store backups in dedicated, access-controlled backup infrastructure.
- Review the web server for additional `.sql`, `.bak`, `.zip`, `.old`, and other backup files.
- Restrict backup access using least-privilege permissions.
- Encrypt sensitive backups.
- Remove unnecessary personal information from backup datasets where possible.
- Review access logs to determine whether the exposed backup has previously been accessed.
- Establish automated checks to detect sensitive files accidentally deployed to production web directories.

---

# 📊 Findings Summary

| Milestone | Finding | Evidence | Risk |
|---|---|---|---|
| **M1** | Confidential patient reports accessible | 3 laboratory reports identified | 🔴 **Critical** |
| **M2** | Weak PDF password protection | Protected documents successfully opened following password recovery | 🟠 **High** |
| **M3** | Exposed database backup | Employee salary and shareholder information exposed | 🔴 **Critical** |

---

# 🏁 Conclusion

The penetration test identified multiple vulnerabilities affecting the confidentiality of sensitive information within the MediRoza Hospital environment.

M1 demonstrated access to three confidential patient laboratory reports.

M2 demonstrated that password protection applied to those reports could be defeated through authorized password-recovery testing, allowing the documents to be opened.

M3 identified an exposed internal database backup containing confidential employee salary information and hospital shareholder records.

The findings demonstrate the need for stronger server-side access controls, secure document handling, robust password policies and strict controls over database backups and other sensitive server files.

Remediation should prioritize the patient-report access-control vulnerability and exposed database backup because both provide direct access to highly sensitive information.

---

# 📁 Evidence Structure

```text
evidence/
│
├── M1-01-nmap-entry-points.png
├── M1-02-whatweb.png
├── M1-03-wafw00f.png
├── M1-04-dirsearch.png
├── M1-05-old-backup.png
├── M1-06-burp-intercept.png
├── M1-07-burp-http-history.png
├── M1-08-three-patient-reports-redacted.png
│
├── M2-01-report-password-prompt.png
├── M2-02-report1-hash.png
├── M2-03-report2-hash.png
├── M2-04-report3-hash.png
├── M2-05-report1-password-redacted.png
├── M2-06-report2-password-redacted.png
├── M2-07-report3-initial-failure.png
├── M2-08-jtr-version.png
├── M2-09-jtr-password-recovery.png
├── M2-10-jtr-completed-redacted.png
├── M2-11-report1-open-redacted.png
├── M2-12-report2-open-redacted.png
├── M2-13-report3-open-redacted.png
│
├── M3-01-backup-discovery.png
├── M3-02-salaries-redacted.png
└── M3-03-shareholders-redacted.png
```

---

# ⚖️ Ethical Disclaimer

This project was conducted in a controlled environment for educational purposes only.

The target was authorized for security testing by **Networkwalks**.

All testing documented in this repository was performed within the authorized scope. These techniques must never be applied to systems without explicit written permission from the owner.

Patient, employee and organisational information should be redacted from all publicly published evidence.
