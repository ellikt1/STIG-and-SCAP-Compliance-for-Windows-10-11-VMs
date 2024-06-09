<h1>STIG and SCAP Compliance for Windows 10 & 11 VMs</h1>

 <img src="https://i.imgur.com/ow6EY8b.png" alt="DoD Cyber Exchange Logo" class="header-image">

<h2>Description</h2>
In this project, I applied the Security Technical Implementation Guide (STIG) to two virtual machines, one running Windows 10 and the other running Windows 11. To automate portions of this process, I utilized the Security Content Automation Protocol (SCAP) tool from the DoD Cyber Exchange website. This tool was used to compare the virtual machines against a baseline (benchmark), scanning for compliance with open security standards to identify compliance or non-compliance of the system. The objective was to perform system hardening in a manner consistent with typical practices used by DoD contractors.
<br />
<br />

Objectives
 
 - <b>Use the SCAP compliance checker to compare virtual machines against a baseline, scanning for security standard compliance. </b>
 - <b>Integrate Group Policy Objects (GPOs) from the DoD Cyber Exchange to streamline and enhance the remediation process. </b>
 - <b>Remediate all Category I (CAT I) vulnerabilities as the primary focus.</b>
 - <b>Achieve a compliance score above 90%.</b>


<h2>Utilities & software Used</h2>

- <b>VMware Workstation Pro 17.5</b>
- <b>STIG viewer 2.17</b>
- <b>SCAP Compliance Checker 5.9(SCC 5.9 Windows)</b>
- <b>Microsoft LGPO (Local Group Policy Object) tool</b>

<h2>STIGS & Benchmarks Used – (DoD Cyber Exchange)</h2>

- <b>Microsoft Windows 10 STIG - Ver 2, Rel 9</b>
- <b>Microsoft Windows 11 STIG - Ver 1, Rel 6</b>
- <b>Microsoft Windows 10 STIG Benchmark - Ver 2, Rel 9</b>
- <b>Microsoft Windows 11 STIG Benchmark - Ver 1, Rel 3</b>
- <b>Group Policy Objects (GPOs) - April 2024</b>
- <b>InstallRoot tool</b>

<h2>Scan Configuration</h2>
In order to effectively scan the Windows 10 virtual machine, it was necessary to configure Tenable Nessus with the appropriate settings. The process involved the following steps:
<br />
<br />
<b>Target Specification:</b>

 - To understand the capabilities of Tenable Nessus in identifying security vulnerabilities. 


<b>Credential Configuration:</b>
 
 - The Nessus vulnerability scanner was configured to perform a credentialed scan. By providing valid Windows credentials, the scanner was able to authenticate to the target system. This approach allowed for a deeper and more comprehensive analysis of the virtual machine, as it could access system files, registry settings, and configuration details that are not available through non-credentialed scans. With credentialed access, Nessus conducted an in-depth evaluation of the virtual machine, checking for missing patches, misconfigurations, insecure settings, and outdated software versions. This comprehensive approach significantly enhanced the scanner's ability to uncover vulnerabilities that would otherwise remain hidden in a standard, non-credentialed scan.

By configuring the Nessus scanner in this manner, the assessment provided a thorough understanding of the security posture of the Windows 10 host, enabling the identification of both surface-level and deep-rooted vulnerabilities. This comprehensive approach ensured that the findings were detailed and actionable, leading to more effective remediation strategies.

<p align="center">
Configuring the Scan <br/>
<img src="https://i.imgur.com/NDEx8CI.png" height="80%" width="80%" alt="Operating System Deployment"/>

<p align="center">
Configuring the Scan cont. <br/>
<img src="https://i.imgur.com/305bLyK.png" height="80%" width="80%" alt="Operating System Deployment"/>

 

<h2>Initial Scan </h2>
The initial scan revealed a total of 392 vulnerabilities, categorized as follows:

- <b>Critical Vulnerabilities: 33</b>


- <b>High Vulnerabilities: 119</b>


- <b>Medium, Low, and Informational Vulnerabilities: 240</b>

Missing Microsoft Security Updates:

- A substantial number of critical vulnerabilities were due to missing security updates from Microsoft. These updates are essential as they often contain patches for recently discovered security flaws that can be exploited by attackers.

- Specific examples included vulnerabilities in Windows operating system components that could allow for remote code execution, privilege escalation, and other severe impacts.

Outdated Microsoft Edge:

- Another significant portion of critical vulnerabilities was related to the outdated Microsoft Edge browser. An outdated browser can have multiple security holes that might be exploited to compromise the system.

- The identified issues included vulnerabilities that could be used for remote code execution, information disclosure, and bypassing security features.

<p align="center">
Scan 1 Vulnerabilities <br/>
<img src="https://i.imgur.com/7qPDh8p.png" height="80%" width="80%" alt="Operating System Deployment"/>

<p align="center">
Scan 1 Vulnerabilities cont. <br/>
<img src="https://i.imgur.com/pQWuBiQ.png" height="80%" width="80%" alt="Operating System Deployment"/>

<p align="center">
Scan 1 Vulnerabilities cont. <br/>
<img src="https://i.imgur.com/krWo0MR.png" height="80%" width="80%" alt="Operating System Deployment"/>








<h2>Initial Remediations </h2>
The goal was to remediate all critical and high vulnerabilities. The remediation process involved the following steps:

<b>Applying Microsoft Security Updates:</b>
- Ensure the Windows Update service was enabled and fully functional.
- Manually initiated the update process to apply all pending critical security updates.
- Configured automatic updates to ensure future updates would be applied promptly.

<b>Updating Microsoft Edge:</b>
- Downloaded and installed the latest version of Microsoft Edge from the official Microsoft website.
- Enabled automatic updates for Edge to ensure it remains up-to-date.

<b>Updating Microsoft Store Apps:</b>
- Updated all Microsoft-installed apps through the Microsoft Store, including addressing vulnerabilities in Microsoft 365 Office apps.
- This step was crucial to mitigate specific critical vulnerabilities found in these applications.

<b>Updating Microsoft Software:</b>
- Performed a comprehensive update of all Microsoft software to the latest versions.
- This included addressing critical vulnerabilities due to absent security updates across various Microsoft products.

<b>Re-scanning and Verification:</b>
- After applying the necessary updates, a re-scan was conducted using Tenable Nessus to verify that the critical and high vulnerabilities had been successfully remediated.
- The re-scan confirmed a significant reduction in the number of critical and high vulnerabilities, demonstrating the effectiveness of the remediation efforts.

<p align="center">
Nessus Suggested Remediations <br/>
<img src="https://i.imgur.com/qKCKm07.png" height="80%" width="80%" alt="Operating System Deployment"/>





<h2>Scan 2 </h2>
The second scan resulted in 124 vulnerabilities, with 1 critical vulnerability and 1 high vulnerability. The critical vulnerability was due to Microsoft Internet Explorer lacking support for new security patches. Nessus marked this vulnerability as critical because it likely contained multiple security issues. The recommended solution was to either upgrade to a supported version of Internet Explorer or disable it. I decided to disable Internet Explorer because the virtual machine already had supported and reliable versions of both Microsoft Edge and Google Chrome as web browsers. 

I disabled Internet Explorer by opening Command Prompt as an administrator and using the following command I found on [Microsoft Learn](https://learn.microsoft.com/en-us/troubleshoot/developer/browsers/installation/disable-internet-explorer-windows):
dism /online /Remove-Capability /CapabilityName:Browser.InternetExplorer~~~~0.0.11.0.


The high vulnerability was WinVerifyTrust Signature Validation CVE-2013-3900. The remote system might be vulnerable to CVE-2013-3900 due to missing or misconfigured registry keys. The recommended action, which I found through [Nessus](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2013-3900), was to add "EnableCertPaddingCheck"="1" to the following registry file path: [HKEY_LOCAL_MACHINE\Software\Microsoft\Cryptography\Wintrust\Config]. 

To implement this, I added the following text to Notepad and saved it as enableAuthenticodeVerification64.reg and restarted the VM:

[HKEY_LOCAL_MACHINE\Software\Microsoft\Cryptography\Wintrust\Config]   "EnableCertPaddingCheck"="1"
[HKEY_LOCAL_MACHINE\Software\Wow6432Node\Microsoft\Cryptography\Wintrust\Config] "EnableCertPaddingCheck"="1"

<p align="center">
Scan 2 Vulnerabilities <br/>
<img src="https://i.imgur.com/UQfjpdW.png" height="80%" width="80%" alt="Operating System Deployment"/>


<h2>Final Scan </h2>
The third and final scan returned only 9 vulnerabilities, with none being critical or high.
<br />
<br />

This project demonstrated the effectiveness of using Tenable Nessus for identifying and remediating vulnerabilities on a Windows 10 virtual machine. Through a series of scans and targeted remediation actions, I successfully reduced the number of vulnerabilities from 392 to just 9, with no critical or high vulnerabilities remaining. This highlights the importance of regular vulnerability assessments and timely updates to maintain a secure IT environment.

<p align="center">
Final Scan Vulnerabilities <br/>
<img src="https://i.imgur.com/TcQPJHh.png" height="80%" width="80%" alt="Operating System Deployment"/>





<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

