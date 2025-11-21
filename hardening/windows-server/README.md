# Windows Server Hardening Checklist — *Jan 2020*

* Check the [Official Microsoft Windows 10 Security Options](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/security-options).
* Consider managing servers using [Windows Admin Center](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/overview).

## General practices

* Do not store credentials in browsers.
  * [Delete saved passwords in Google Chrome](https://support.google.com/chrome/answer/95606).
  * [Delete saved passwords in Mozilla Firefox](https://support.mozilla.org/en-US/questions/731567).
* [Uninstall unnecessary programs](https://kb.blackbaud.com/knowledgebase/Article/46429).
  * Most of the time, browsers like Google Chrome or Mozilla Firefox are not needed on the server. Consider removing them.

## Security Baselines

* It's recommended to import the Security baselines, a preconfigured groups of Windows settings that help you apply the security settings that are suggested by the relevant security teams.
* [Microsoft Security Baselines](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-security-baselines).
* [CIS Benchmarks](https://docs.microsoft.com/en-us/compliance/regulatory/offering-cis-benchmark).

## Protecting credentials

* [Set a complex password to the Administrator user](https://prevps.com/clients/index.php?rp=/knowledgebase/2/How-to-Change-password-Windows-Server-2008-2012-2016-2019.html).
* [Prevent Windows from storing an LM hash of the password](http://support.microsoft.com/kb/299656).
* [Disable guest account](https://www.windows-commandline.com/enable-disable-guest-account/).
* [Delete unused user accounts](https://docs.microsoft.com/en-us/troubleshoot/windows-server/user-profiles-and-logon/delete-user-profile).
* [Prevent users from logging in with Microsoft accounts](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/accounts-block-microsoft-accounts).
* [Use the tiered management model](https://docs.microsoft.com/en-us/security/compass/privileged-access-access-model) and [separate administrator accounts for each tier](https://www.researchgate.net/publication/293807740/figure/fig2/AS:670050327465999@1536763855415/Modified-Microsofts-administrative-three-tier-Model-2.ppm). It prevents exposing privileged credentials.
* [Use the Protected Users Group for high-privileged accounts](https://docs.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/protected-users-security-group).
  ```powershell
  Add-ADGroupMember -Identity "Protected Users" -Members "CN=Ferran Verdés,CN=Users,DC=example,DC=com"
  ```
* [Use a Managed Service Account](https://docs.microsoft.com/en-us/services-hub/health/kb-running-assessments-with-msas):
  * [Standalone Managed Service Account (sMSA)](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/service-accounts-standalone-managed): provides automatic password management.
  * [Group Managed Service Account (gMSA)](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/service-accounts-group-managed): provides the same functionality within the domain but also extends that functionality over multiple servers.
* [Define Authentication Policies for high-privileged accounts](https://docs.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos). Allow the domain admin to only log in to the DC.

## Protecting against malware

* Configure Windows Defender using GPO (e.g. enable MAPS).
* [Use Attack Surface Reduction (ASR) rules](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction?view=o365-worldwide#attack-surface-reduction-rules) on Microsoft Defender Exploit Guard.
* Configure WSUS to auto approve and deploy definition updates for Microsoft Defender.

## Network services

* [Disable NetBIOS and LLMNR Protocols](http://woshub.com/how-to-disable-netbios-over-tcpip-and-llmnr-using-gpo/).
* [Configure encryption types allowed for Kerberos](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/network-security-configure-encryption-types-allowed-for-kerberos).
* [Prevent LocalSystem NULL session fallback](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/network-security-allow-localsystem-null-session-fallback).
* Disable IPv6 (not suggested).
  * [Configuring IPv6 in Windows for advanced users](https://docs.microsoft.com/en-US/troubleshoot/windows-server/networking/configure-ipv6-in-windows).
     ```powershell
     reg add hklm\system\currentcontrolset\services\tcpip6\parameters /v DisabledComponents /t REG DWORD /d 0xFF
     ```
  * The correct value for **DisabledComponents** registry should be `0xff`, not `0xffffffff`.
     ```powershell
     reg query hklm\system\currentcontrolset\services\tcpip6\parameters /v DisabledComponents
     ```
  * You cannot completely disable IPv6 as IPv6 is used internally on the system for many TCPIP tasks. For example, you will still be able to run ping `::1` after configuring this setting or seeing the ipv6 `[::]` address listening in `netstat`.
     ```powershell
     netstat -aon
     ```

### Securing NTLM
* [Improve the LAN Manager authentication level](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/network-security-lan-manager-authentication-level) selecting `Send NTLMv2 responses only and refuse LM & NTLM`, but if it's needed for backward compatibility, select `Send NTLMv2 response only` at least.

### Securing SMB
* [Prevent network shares to be accessed anonymously](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/network-access-shares-that-can-be-accessed-anonymously).
* Make sure SMBv1 is disabled:
  ```powershell
  Get-SmbServerConfiguration | Select EnableSMB1Protocol
  ```
* Configure SMB signing to prevent MiTM attacks:
  * [SMB client must request packet signing](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/microsoft-network-client-digitally-sign-communications-always)
  * [Server SMB component must request packet signing](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/microsoft-network-server-digitally-sign-communications-always)
* Check if SMB encryption is enabled, otherwise enable it:
  ```powershell
  Get-SmbServerConfiguration | Select EncryptData
  ```
  ```powershell
  Set-SmbServerConfiguration -EncryptData $true -Force
  ```
* Reject unencrypted connections:
  ```powershell
  Set-SmbServerConfiguration -RejectUnencryptedAccess $true
  ```

### Securing DNS

* Implement a secure DNS zone with DNSSEC.

### Windows Defender Firewall

* [Enable Windows Defender Firewall for all profiles (domain, private, public) and block inbound traffic by default](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/best-practices-configuring).
* Disable all default inbound rules (not suggested) and create those to open only the specific ports needed for each profile.
* [Map every new rule to a specified service or program](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/create-an-inbound-program-or-service-rule).

## Further reading

* [Windows Server Security Best Practices](https://www.netwrix.com/windows_server_hardening_checklist.html)

|Source: [*"Securing Windows Server 2019" from Pluralsight*.][1]|
|:--:|

[1]: https://www.pluralsight.com/courses/securing-windows-server-2019
