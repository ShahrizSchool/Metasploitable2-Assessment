# Metasploitable 2 - Vulnerability Assessment Findings
**Target:** Metasploitable 2 IP Address
**Scanner:** Custom Vulnerability Scanner
**Date:** 2026-05-31
**Environment:** Isolated VirtualBox host-only network
 
## Summary
**Total CVEs: 229**
| Service | Total | Critical | High | Medium | Low |
|---|---|---|---|---|---|
| OpenSSH | 25 | 3 | 5 | 13 | 4 |
| ISC BIND | 31 | 1 | 14 | 21 | 1 |
| Apache httpd | 90 | 9 | 20 | 54 | 7 |
| ProFTPD | 18 | 2 | 8 | 7 | 1 |
| MySQL | 11 | 0 | 1 | 9 | 1 |
| PostgreSQL | 54 | 6 | 14 | 33 | 1 |

 
## CVE Findings
### Finding 001 - Apache httpd 2.2.8
| Field | Value |
|---|---|
| CVE | CVE-2010-0425 |
| Severity | CRITICAL |
| CVSS Score | 10.0 |
| Service | Apache httpd 2.2.8 |
| Port | 80/tcp |
| Assessment | Possible False Positive |
 
**Description:** The mod_isapi module in Apache HTTP Server 2.0.37 through 2.2.14 on Windows does not ensure request processing is complete before unloading an ISAPI .dll module, allowing remote attackers to execute arbitrary code via crafted requests.
 
**False Positive Note:** CVE-2010-0425 specifically affects Apache running on **Windows**. Metasploitable 2 runs on Ubuntu Linux. This is likely a false positive due to CPE matching returning Windows-specific CVEs against a Linux host. Manual verification recommended.
 
**Risk:** Remote code execution without authentication.
 
### Finding 002 - ProFTPD 1.3.1
| Field | Value |
|---|---|
| CVE | CVE-2019-12815 |
| Severity | CRITICAL |
| CVSS Score | 9.8 |
| Service | ProFTPD 1.3.1 |
| Port | 2121/tcp |
| Assessment | True Positive |
 
**Description:** An arbitrary file copy vulnerability in mod_copy in ProFTPD up to 1.3.5b allows remote code execution and information disclosure without authentication.
 
**Risk:** Unauthenticated remote code execution allows complete system compromise without credentials.
 
### Finding 003 - PostgreSQL 8.3
| Field | Value |
|---|---|
| CVE | CVE-2013-1902 |
| Severity | CRITICAL |
| CVSS Score | 10.0 |
| Service | PostgreSQL 8.3 |
| Port | 5432/tcp |
| Assessment | True Positive |
 
**Description:** PostgreSQL 8.3.x before 8.3.23 generates insecure temporary files with predictable filenames, which may allow attackers to exploit installation scripts on Linux and Mac OS X.
 
**Risk:** Predictable temporary file names during installation may allow local privilege escalation.
 
### Finding 004 - PostgreSQL 8.3
| Field | Value |
|---|---|
| CVE | CVE-2013-1903 |
| Severity | CRITICAL |
| CVSS Score | 10.0 |
| Service | PostgreSQL 8.3 |
| Port | 5432/tcp |
| Assessment | True Positive |
 
**Description:** PostgreSQL 8.3.x before 8.3.23 incorrectly provides the superuser password to scripts related to graphical installers for Linux and Mac OS X.
 
**Risk:** Superuser credentials exposed during installation scripts may allow full database compromise.
 
### Finding 005 - ISC BIND 9.4.2
| Field | Value |
|---|---|
| CVE | CVE-2008-0122 |
| Severity | CRITICAL |
| CVSS Score | 10.0 |
| Service | ISC BIND 9.4.2 |
| Port | 53/tcp |
| Assessment | Possible False Positive |
 
**Description:** Off-by-one error in the inet_network function in libbind in ISC BIND 9.4.2 and earlier, as used in FreeBSD 6.2 through 7.0-PRERELEASE, allows attackers to cause a denial of service or execute arbitrary code via crafted input.
 
**False Positive Note:** CVE-2008-0122 references FreeBSD operating system specifically. Metasploitable 2 runs on Ubuntu Linux. This may be a platform-specific false positive. Manual verification recommended.
 
**Risk:** Remote code execution via crafted DNS input.
 
### Finding 006 - OpenSSH 4.7p1
| Field | Value |
|---|---|
| CVE | CVE-2016-1908 |
| Severity | CRITICAL |
| CVSS Score | 9.8 |
| Service | OpenSSH 4.7p1 |
| Port | 22/tcp |
| Assessment | True Positive |
 
**Description:** The client in OpenSSH before 7.2 mishandles failed cookie generation for untrusted X11 forwarding, allowing remote X11 clients to obtain trusted X11 forwarding privileges.
 
**Risk:** Allows privilege escalation via X11 forwarding bypass if X11 forwarding is enabled.
 
## Configuration Findings
### Config Finding 001 - FTP Service Detected
| Field | Value |
|---|---|
| Severity | CRITICAL |
| Port | 21/tcp |
| Service | vsftpd |
 
**Description:** FTP transmits all data including credentials in plaintext.
 
**Risk:** Any network observer can capture usernames, passwords, and file transfers without any decryption.
 
### Config Finding 002 — Telnet Service Detected
| Field | Value |
|---|---|
| Severity | CRITICAL |
| Port | 23/tcp |
| Service | telnetd |
 
**Description:** Telnet transmits all data including credentials in plaintext.
 
**Risk:** Authentication and session data are fully visible to any network observer.
 
 
### Config Finding 003 - HTTP Service Detected
| Field | Value |
|---|---|
| Severity | MEDIUM |
| Port | 80/tcp |
| Service | Apache httpd |
 
**Description:** HTTP traffic is unencrypted.
 
**Risk:** Data transmitted between clients and the server including cookies, form data, and session tokens can be intercepted.
 
## Analysis
1. **False positive identification:** Two CRITICAL findings (CVE-2010-0425 and CVE-2008-0122) are likely false positives due to platform-specific CVEs being matched against a Linux host. Demonstrates the importance of human triage.
2. **Plaintext services:** Telnet and FTP represent the most immediately actionable findings. Data being transmitted is unencrypted.
3. **Severely outdated software:** All detected services are multiple major versions behind current releases, representing years of unpatched vulnerabilities.
