<div align=center>
       
# 🔐 MediRoza Hospital — Penetration Testing Report

![Cybersecurity](https://img.shields.io/badge/Field-Cybersecurity-red)
![Assessment](https://img.shields.io/badge/Assessment-Penetration%20Testing-blue)
![Environment](https://img.shields.io/badge/Environment-Authorized-green)
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-lightgrey)

</div>

---


## 📌 Assessment Information

| Item | Details |
|---|---|
| **Pen Tester Name** | Malehloa Seroke |
| **Target** | MediRoza Hospital Web Application |
| **Assessment Type** | Web Application Penetration Testing |
| **Authorization** | Written Permission Granted |
| **Environment** | Controlled Educational Environment |
| **Program** | Networkwalks Cybersecurity |
| **Platform** | Kali Linux |
| **Report** | Detailed Penetration Testing Report |

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

## Confidential Patient Laboratory Reports

### Description

The first milestone required the identification of three confidential patient laboratory reports.

Initial reconnaissance was performed to identify the target's externally accessible services and web application attack surface.

Nmap identified several exposed services and confirmed that HTTP/HTTPS services were available for further web application assessment.

### Evidence — Network Reconnaissance

**Nmap** — Used to identify open ports and exposed network services that could provide potential entry points into the target.

<img width="482" height="62" alt="nmap for entry points" src="https://github.com/user-attachments/assets/7182ea48-ec64-47db-9c6f-ed3484629dd4" /> 

<br/>

Further web reconnaissance was performed using tools including `nslookup`, `DNSRecon`, `WhatWeb`, `cURL`, and `WAFW00F`.

**nslookup** — Used to resolve the target domain name to its associated IP address and verify DNS information.

<img width="405" height="149" alt="nslookup" src="https://github.com/user-attachments/assets/cec08684-9630-4cff-b689-6bdb31c66516" />
 <br/>
 
**DNSRecon** — Used to enumerate DNS records such as nameservers, mail servers, IP addresses, SPF, and DMARC information.

<img width="956" height="772" alt="dnsrecon" src="https://github.com/user-attachments/assets/4b2ae02c-fa1e-4569-8608-54b4a1f70aa5" />
 <br/>
 
These activities helped establish the target's DNS configuration, HTTP behaviour, server technologies, and available web attack surface.

### Evidence — Web Reconnaissance

**WHOIS** — Used to gather publicly available domain registration, registrar, nameserver, and domain-related information. 

<img width="734" height="845" alt="whois 1" src="https://github.com/user-attachments/assets/0f260ce3-f880-4cac-b9c7-ac3553dbfe5f" />

 <br/>
 
**WhatWeb** — Used to fingerprint the web application and identify technologies, server software, frameworks, and other web components.

<img width="940" height="332" alt="whatweb 1" src="https://github.com/user-attachments/assets/b96adfa8-dd7e-44ef-8cc4-0d658415b700" />
<br/>

**WAFW00F** — Used to determine whether the target web application was protected by a Web Application Firewall (WAF).

<img width="535" height="360" alt="wafw00f" src="https://github.com/user-attachments/assets/4f0c67ad-b7ba-4b93-8129-3fe7f1e64514" />
 <br/>
 
**cURL** — Used to send HTTP requests to the target and inspect the responses returned by the web server.

<img width="967" height="830" alt="curl" src="https://github.com/user-attachments/assets/324ce8b5-7f51-4200-a6e8-e6eac20b3617" />
 <br/>
 
**cURL `-I`** — Used to retrieve and inspect HTTP response headers without downloading the full page content.

<img width="526" height="170" alt="curl -I" src="https://github.com/user-attachments/assets/1f8306d9-d14f-4907-be5b-e68d04e927bc" />
 <br/>
 
**cURL `-v`** — Used to view detailed HTTP connection information, including the request, response headers, TLS connection, and redirects.

<img width="889" height="852" alt="curl -v" src="https://github.com/user-attachments/assets/e1feb869-43a5-477e-83df-4d49d50bda19" />
 
 <br/>
 
---

## Directory and Resource Enumeration

Web content enumeration was subsequently performed using **Dirsearch**.

The enumeration identified several accessible resources, including an `/old/` directory, which returned a successful HTTP response and required further investigation.

### Evidence — Directory Enumeration

**Dirsearch** — Used to enumerate hidden directories and files on the web server, helping identify exposed resources such as the `/old/` directory for further investigation.

<img width="948" height="856" alt="dirsearch" src="https://github.com/user-attachments/assets/3d38b680-2eb3-469f-9405-6dcd16df8597" />

Further investigation of the old resources identified an exposed backup associated with the application.

### Evidence — Old Backup Discovery

<img width="1900" height="1012" alt="find-old_backup" src="https://github.com/user-attachments/assets/a5e479f1-2024-4413-bc62-b2a1e5c04313" />

---

## Patient Portal Investigation

The patient-facing application functionality was investigated as part of the search for the required laboratory reports.

**Burp Suite** was configured to intercept browser traffic generated while interacting with the patient portal.

<img width="953" height="705" alt="burpsuite" src="https://github.com/user-attachments/assets/262405b5-e241-4578-8911-c897a72f5a8e" />
<br/>

### Evidence — Burp Suite Interception

<img width="1912" height="918" alt="burpsuite intercept" src="https://github.com/user-attachments/assets/4d4d21c5-520c-48ae-8418-d08ba21e4cd1" />

**SQL Injection Testing** — Used to assess whether the application improperly handled user-supplied input in database queries and could potentially allow unauthorized database access.

<img width="615" height="462" alt="sql inj" src="https://github.com/user-attachments/assets/fd0208f3-5e98-4ef0-892e-fc063cfec533" /> <br/>

The application's HTTP history was then analysed to understand the requests generated by the patient functionality.

### Evidence — HTTP History

<img width="1911" height="263" alt="Burpsuite Http history" src="https://github.com/user-attachments/assets/5a1e62ab-50fa-4478-81a1-b4855a878363" />

Burp Suite allowed the relevant requests and responses to be inspected while testing the application's access-control behaviour.

---

## Successful Report Discovery

Testing resulted in access to a patient laboratory-report page containing the **three PDF reports required by M1**.

### Evidence — Three Patient Reports

<img width="952" height="630" alt="3 Patients report" src="https://github.com/user-attachments/assets/294606d3-8170-4465-8adc-d7f07873e393" />

This confirmed that confidential patient laboratory documents were exposed through the tested application workflow.

> ⚠️ Patient names, medical information and other personally identifiable information must be redacted before this evidence is published publicly.

---

## Impact

The vulnerability could allow unauthorized access to confidential medical documents.

Successful exploitation could result in:

- Disclosure of patient medical information
- Exposure of personally identifiable information
- Loss of patient confidentiality
- Privacy and regulatory consequences
- Reputational damage to the hospital

---

# PDF Password Security

## Weak Password Protection of Confidential PDF Reports

### Description

The laboratory reports obtained during M1 were protected by passwords.

Opening the reports resulted in a password prompt, confirming that PDF-level protection had been applied.

---

## Hash Extraction

The password-protected PDF documents were processed using the available password-analysis tools.

The **Networkwalks Hash Calculator** was used during the analysis of the protected reports.

### Evidence — Report 1 Hash

<img width="1666" height="768" alt="Hash Calc Report 1" src="https://github.com/user-attachments/assets/6d422a1b-30f4-4689-b54e-e9ac067add55" />

### Evidence — Report 2 Hash

<img width="1783" height="837" alt="Hash Calc Report 2" src="https://github.com/user-attachments/assets/00ae92a0-37b8-4a17-9e48-81b70aec35c1" />

### Evidence — Report 3 Hash

<img width="1628" height="849" alt="Hash Calc Report 3" src="https://github.com/user-attachments/assets/14c9a09d-98fd-49f6-8b1d-b40f8b13674b" />

---

## Password Recovery

Password recovery testing was subsequently performed against the extracted password information.

The evidence demonstrates successful password recovery for the protected reports.

### Evidence — Report 1 Password Recovery

<img width="1886" height="859" alt="Report 1 Password" src="https://github.com/user-attachments/assets/d9ee61cd-814b-4655-ad1e-5d06b49132cd" />

### Evidence — Report 2 Password Recovery

<img width="1884" height="863" alt="Report 2 Password" src="https://github.com/user-attachments/assets/487f2a43-7438-414d-99da-70a05526d529" />

Report 3 initially presented additional difficulty during password recovery.

### Evidence — Initial Report 3 Failure

<img width="1906" height="888" alt="Report 3 Pass Crack Fail" src="https://github.com/user-attachments/assets/672b9832-1e60-412b-a46e-bc701caa175f" />

Further password recovery testing was performed using **John the Ripper**.

The tool installation/version was verified before the cracking process was performed.

### Evidence — Password Recovery Process

<img width="825" height="716" alt="JTR Pass Crack" src="https://github.com/user-attachments/assets/ff51fa71-4473-4586-95ed-2c0ead8f6600" />

### Evidence — Successful Password Recovery

<img width="774" height="723" alt="JTR Pass Crack  Done" src="https://github.com/user-attachments/assets/b926ea98-f738-4f07-9111-cca4ebfedd39" />

---

## Successful Document Access

After recovering the required passwords, the protected laboratory reports could be opened.

### Evidence — Report 1 Opened

<img width="842" height="822" alt="Report 1 Doc Open" src="https://github.com/user-attachments/assets/530e73f2-936b-41ed-84de-725653993ae0" />

### Evidence — Report 2 Opened

<img width="835" height="822" alt="Report 2 Doc open" src="https://github.com/user-attachments/assets/c72aebf7-77d9-45ed-bcaf-8e7a81244c1d" />

### Evidence — Report 3 Opened

<img width="837" height="819" alt="Report 3 open" src="https://github.com/user-attachments/assets/c2332062-6ea3-4ea4-b78e-3843b9a039de" />

This demonstrated that the passwords protecting the confidential medical documents were not sufficiently resistant to the authorized password-recovery testing.

> ⚠️ Passwords and confidential medical information should be redacted from the public GitHub evidence.

---

## Impact

Weak document passwords reduce the effectiveness of PDF-level protection.

If an unauthorized party obtained the PDF files, recoverable passwords could allow access to confidential medical information despite the documents being password protected.

PDF passwords should therefore not be treated as a substitute for proper application-level access control.

---

# Critical Sensitive Data Exposure

## Exposed Internal Database Backup

### Description

The third milestone required further analysis of information discovered during the previous phases.

Directory and file analysis identified an exposed historical database backup associated with the MediRoza Hospital application.

The exposed backup was identified as:

```text
mediroza_db_backup_2019.sql
```

The backup identified itself as an internal MediRoza Hospital database backup and contained confidential staff and shareholder records.

### Evidence — Backup Discovery

<img width="1900" height="1012" alt="find-old_backup" src="https://github.com/user-attachments/assets/c5b7fd1e-dfd2-40d6-8472-eeab508d5020" />

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

<img width="1136" height="523" alt="image" src="https://github.com/user-attachments/assets/37f8f9d6-da58-48ae-9548-97e276f8cebd" />

### Suggested filename:

**cURL `-s`** — Used to send HTTP requests in silent mode, displaying the server response without progress or transfer information for cleaner analysis.

<img width="857" height="146" alt="curl -s" src="https://github.com/user-attachments/assets/9f8f76e4-abd4-40f1-b434-ea437f153985" />
<br/>
Salaries will also be found on the Stakeholders.txt file

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

<img width="924" height="462" alt="image" src="https://github.com/user-attachments/assets/da499315-9952-4015-8511-1d6713eca34c" />

### Suggested filename:

Shareholder details will also be found on the Stakeholders.txt file

---

## Impact

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

# Recommendations and Remediation

## Patient Report Access

The hospital should:

- Enforce server-side authentication and authorization for every patient report request.
- Verify ownership and authorization before returning any medical document.
- Prevent direct unauthenticated access to report files.
- Review patient portal session-management controls.
- Log and monitor access to medical reports.
- Perform regular access-control testing.

---

## PDF Password Protection

The hospital should:

- Avoid relying on PDF passwords as the primary protection for medical information.
- Use strong, unique and sufficiently complex passwords where document encryption is required.
- Implement application-level authorization before allowing documents to be downloaded.
- Store medical documents outside publicly accessible web directories.
- Review existing protected documents for weak passwords.
- Apply appropriate encryption for sensitive data at rest.

---

## Exposed Database Backup

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

# ⚖️ Ethical Disclaimer

This project was conducted in a controlled environment for educational purposes only.

The target was authorized for security testing by **Networkwalks**.

All testing documented in this repository was performed within the authorized scope. These techniques must never be applied to systems without explicit written permission from the owner.

Patient, employee and organisational information should be redacted from all publicly published evidence.

---

# 👤 Author
Malehloa Seroke
Cybersecurity Professional B082

LinkedIn: [www.linkedin.com/in/malehloa-seroke]

# 📌 Project Information
Program Name: Cybersecurity at Networkwalks | Week: 04 | Project: WK4-Penetration Testing Project | Repository: GitHub

