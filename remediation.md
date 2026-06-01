# Metasploitable 2 - Remediation Report
**Target:** Metasploitable 2 IP Address
**Scanner:** Custom Vulnerability Scanner
**Date:** 2026-05-31
**Environment:** Isolated VirtualBox host-only network

## CVE Remediations
### Remediation 001 - Apache httpd 2.2.8 (CVE-2010-0425)
**Remediation Plan:** Upgrade Apache HTTP Server to the latest stable release (2.4.x). Apache 2.2.x reached end-of-life in January 2018 and no longer receives security patches. Upgrading addresses CVE-2010-0425 and the majority of the 90 CVEs detected against this version.

**Note:** CVE-2010-0425 was assessed as a likely false positive as it specifically affects Apache on Windows. Metasploitable 2 runs Ubuntu Linux. However, upgrading Apache remains the correct remediation regardless, as the remaining critical findings are confirmed true positives.

**Priority:** High, end-of-life software with 9 critical CVEs.

### Remediation 002 - ProFTPD 1.3.1 (CVE-2019-12815)
**Remediation Plan:** Upgrade ProFTPD to version 1.3.6 or later, which contains the patch for CVE-2019-12815. Additionally, disable the mod_copy module in `proftpd.conf` if file copy functionality is not required, as mod_copy is the vulnerable component.

If FTP is not operationally required, disable the service entirely and replace with SFTP via OpenSSH.

**Priority:** Critical, unauthenticated remote code execution.

### Remediation 003 & 004 - PostgreSQL 8.3 (CVE-2013-1902, CVE-2013-1903)
**Remediation Plan:** Upgrade PostgreSQL to the latest supported release (16.x). PostgreSQL 8.3 reached end-of-life in February 2013 and has received no security patches since. Both CVEs relate to insecure handling of temporary files and superuser credentials during installation scripts.

Post-upgrade, review and rotate database superuser credentials as a precaution, as CVE-2013-1903 may have exposed them during installation.

**Priority:** Critical, end-of-life database with credential
exposure risk.

### Remediation 005 - ISC BIND 9.4.2 (CVE-2008-0122)
**Remediation Plan:** Upgrade BIND to the latest stable release (9.18.x LTS). BIND 9.4.2 is severely outdated with 31 CVEs detected. CVE-2008-0122 was assessed as a possible false positive due to FreeBSD-specific references, however upgrading BIND addresses the  onfirmed true positive findings in this version regardless.

If DNS resolution is not required on this host, consider disabling the BIND service entirely and using an external DNS resolver.

**Priority:** High, severely outdated DNS software with known
denial of service and code execution vulnerabilities.

### Remediation 006 - OpenSSH 4.7p1 (CVE-2016-1908)
**Remediation Plan:** Upgrade OpenSSH to the latest stable release (9.x). OpenSSH 4.7p1 was released in 2008 and has accumulated 25 CVEs. CVE-2016-1908 is a confirmed true positive.

**Priority:** High, outdated SSH with confirmed X11 forwarding
vulnerability. 

## Configuration Remediations
### Config Remediation 001 - FTP Service
**Remediation Plan:** Disable FTP. Replace with SFTP (port 22) or FTPS if file transfer is required.

### Config Remediation 002 - Telnet Service
**Remediation Plan:** Disable Telnet immediately. Replace with SSH for all remote administration.

### Config Remediation 003 - HTTP Service
**Remediation Plan:** Migrate to HTTPS (port 443) using a valid TLS certificate. Redirect all HTTP traffic to HTTPS.

## Remediation Priority Order
| Priority | Finding | Action |
|---|---|---|
| 1 | ProFTPD CVE-2019-12815 | Upgrade or disable mod_copy immediately |
| 2 | Telnet Port 23 | Disable service immediately |
| 3 | FTP Port 21 | Disable service, replace with SFTP |
| 4 | PostgreSQL 8.3 | Upgrade, rotate credentials |
| 5 | Apache httpd 2.2.8 | Upgrade to 2.4.x |
| 6 | OpenSSH 4.7p1 | plan upgrade |
| 7 | ISC BIND 9.4.2 | Upgrade or disable if not required |
| 8 | HTTP Port 80 | Migrate to HTTPS |