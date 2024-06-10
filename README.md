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

<h2>Initial Scan </h2>
In the initial phase of the project, I imported the STIG benchmarks, specifically the Microsoft Windows 10 STIG Benchmark - Version 2, Release 9, into the SCAP compliance tool. This step was taken to assess the current state of the virtual machines. I opted to conduct this assessment before implementing the Group Policy Objects (GPOs) to gauge the potential impact and effectiveness of the GPOs on system security and compliance. I configured the SCAP compliance checker tool to output the scans in a specified location for improved accessibility.
<br />
<br />
 <p align="center">
Initial SCAP Summary <br/>
<img src="https://i.imgur.com/dgT6gQp.png" height="80%" width="80%" alt="Initial Scan"/>
 <br />
<br />

After creating a checklist in the STIG viewer to organize notes and reference the STIG, I proceeded to import the SCAP XCCDF file into the STIG viewer. This allowed for a clearer visualization of vulnerabilities, color-coded according to severity (CAT I, II, III), enhancing the assessment process. 

 <p align="center">
Initial STIG View <br/>
<img src="https://i.imgur.com/4XLn5Xd.png" height="80%" width="80%" alt="Initial Scan"/>



<h2>Group policy</h2>
 
To update the current group policies, I utilized the Microsoft LGPO tool to integrate the latest GPOs obtained from the DoD Cyber Exchange. Following the LGPO tool's documentation, I launched a Command Prompt as an administrator and navigated to the LGPO tool directory. Then, I executed the command LGPO.exe /g “File Path of the DOD GPO” twice, once for the computer configuration and once for the user configuration, adjusting the file paths accordingly. After completing this step, I initiated a quick update of the group policy by running the command gpupdate /force and I restarted the VM.

<p align="center">
Applying DoD GPOs using Microsoft's LGPO tool  <br/>
<img src="https://i.imgur.com/NUWCKrN.png" height="80%" width="80%" alt="LGPO"/>

<p align="center">
 Applying DoD GPOs using Microsoft's LGPO tool cont. <br/>
<img src="https://i.imgur.com/rOLgr7T.png" height="80%" width="80%" alt="LGPO"/>

<p align="center">
 DoD Startup Screen <br/>
<img src="https://i.imgur.com/PIpVc6B.png" height="80%" width="80%" alt="LGPO"/>
 

<h2>2nd Scan </h2>

- After successfully integrating the GPOs from the DoD Cyber Exchange, I conducted another SCAP scan to assess the updated state of the virtual machines. The scan resulted in a compliance score of 96.21, indicating overall compliance. However, there were 2 remaining CAT I vulnerabilities that required remediation.
The first CAT I vulnerability involved configuring DEP (Data Execution Prevention) to at least Opt-out. To address this, I followed the provided fix from the STIG by navigating to the Control Panel and enabling DEP for all programs and services except those I selected. Then, I changed this open finding to "Not a Finding" in the STIG viewer and updated the comment to "Resolved with the fix provided above."

- The second CAT I vulnerability required enabling BitLocker. Upon attempting to enable BitLocker, I encountered a Group Policy conflict error. To resolve this, I adjusted the GPO settings to disallow TPM startup, enabling me to configure BitLocker successfully. After configuring BitLocker, I restarted the system and conducted another SCAP scan. Then, I changed this open finding to "Not a Finding" in the STIG viewer and updated the comment to "Resolved with the fix provided above."

- I decided to resolve the CAT II vulnerabilities V-220903, V-220904, V-220905, and V-220906 to achieve a 100% compliance score. These vulnerabilities were related to installing the correct DoD Root CA certificates. To address them, I downloaded the InstallRoot tool from the Cyber Exchange website and used it to install the correct DoD Root CAs.  

- It's worth noting that the SCAP tool may not scan all rules in the STIG, necessitating manual verification in real-world scenarios to ensure all vulnerabilities are addressed. However, for the purpose of this project, manual verification was not performed.


<p align="center">
 DEP CAT I Vulnerability <br/>
<img src="https://i.imgur.com/LM6vjvF.png" height="80%" width="80%" alt="CAT I"/>

<p align="center">
 BitLocker CAT I Vulnerability <br/>
<img src="https://i.imgur.com/ag8hYoP.png" height="80%" width="80%" alt="CAT I"/>




<h2>Final Scan</h2>
After remediating all the CAT I and CAT II vulnerabilities, I used the SCAP tool to perform another scan. This is what I was left with:

<p align="center">
Final SCAP Summary Viewer <br/>
<img src="https://i.imgur.com/TLtPxoR.png" height="80%" width="80%" alt="100% SCAP SCAN"/>

<p align="center">
Final SCAP Summary Viewer cont. <br/>
<img src="https://i.imgur.com/UXqkV71.png" height="80%" width="80%" alt="No CAT Vulnerabilities"/>



<h2>Windows 11 </h2>
I repeated the same steps outlined above to remediate the Windows 11 virtual machine. I focused on achieving a green compliance score with the SCAP tool, addressing the same types of vulnerabilities and using similar remediation methods as with the Windows 10 virtual machine. Once the SCAP tool indicated a green compliance status, I concluded the remediation process. I included some screenshots below of the process.



<p align="center">
SCAP Summary Viewer (Before adding DoD GPOs) cont. <br/>
<img src="https://i.imgur.com/EK8rvpw.png" height="80%" width="80%" alt="100% SCAP SCAN"/>

<p align="center">
Stig viewer (Before adding DoD GPOs) <br/>
<img src="https://i.imgur.com/gXROakW.png" height="80%" width="80%" alt="100% SCAP SCAN"/>

<p align="center">
Stig Viewer after remediating BitLocker, GPOs, DEP, and password vulnerabilities <br/>
<img src="https://i.imgur.com/sQmcDAM.png" height="80%" width="80%" alt="100% SCAP SCAN"/>

<p align="center">
Final SCAP Summary Viewer <br/>
<img src="https://i.imgur.com/p5wbipQ.png" height="80%" width="80%" alt="100% SCAP SCAN"/>


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

