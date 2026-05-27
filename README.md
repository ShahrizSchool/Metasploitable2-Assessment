# Metasploitable 2 — Vulnerability Assessment
A hands-on vulnerability assessment conducted against Metasploitable 2, 
a deliberately vulnerable Linux VM, using a custom-built Python 
vulnerability scanner. This lab demonstrates a professional 
vulnerability management cycle, from initial discovery through 
remediation and verification.

The assessment was conducted in an isolated VirtualBox host-only 
network environment using Kali Linux as the attack platform. No 
external networks were involved.

## Objectives
- Validate the custom scanner against a known-vulnerable target
- Identify open services, version-specific CVEs, and weak configurations
- Analyze findings to distinguish true positives from false positives
- Apply remediations where possible and verify through rescanning
- Document the full assessment lifecycle in a reproducible format

## Tool Used
Findings were generated using the 
[Custom Vulnerability Scanner](https://github.com/ShahrizSchool/Custom-Vulnerability-Scanner), a Python-based 
network scanner built to integrate Nmap service detection with 
real-time NVD CVE lookups and configuration analysis.

## Assessment Workflow
Scan: Initial scanner run against Metasploitable 2
Document: Record all CVE findings and config issues
Analyze: Research exploitability, flag false positives
Remediate: Apply fixes within the lab environment
Rescan: Verify findings resolved post-remediation
Document fixes: Record changes and rescan results

## Environment
- **Attack VM:** Kali Linux 2025.4
- **Target VM:** Metasploitable 2
- **Network:** VirtualBox host-only (isolated)
- **Scanner:** Custom Python vulnerability scanner

## Legal
This assessment was conducted exclusively in a private, isolated lab 
environment against a VM designed for security testing. No 
unauthorized systems were targeted.