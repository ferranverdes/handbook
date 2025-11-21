# Service Accounts — *Jan 2020*

* A service account is a user account that is created explicitly to provide a security context for services running on Windows Server operating systems.
* A service account is created to run and isolate a particular service or application following the principle of least privilege, which means giving the subject only the minimum rights and permissions required.
* A service has a primary security identity that determines the access rights for local and network resources. The security context for a Microsoft service is determined by the service account that's used to start the service.
* It is recommended that you add a prefix such as `svc-` to all accounts that you use as service accounts. This naming convention will make the accounts easier to find and manage.

|Source: [*"Introduction to Active Directory service accounts" from Microsoft Docs*][1], [*"Service Accounts" from  Microsoft Docs*][2] and [*"MCITP 70-640: Service Accounts" from  YouTube.*][4]|
|:--:|

## Types of on-premises service accounts

### Group managed service accounts

* Use `group Managed Service Accounts (gMSAs)` whenever possible. gMSAs are managed domain accounts that you use to help secure services. 
* gMSAs can run on a server farm, such as systems behind a network load balancing or Internet Information Services (IIS) server. gMSAs can also be used for services that run only on a single server.
* Benefits of using gMSAs:
  * Cycle passwords regularly: password management for that account is handled by the Windows operating system, which changes the password every 30 days. Service and domain administrators no longer need to schedule password changes.
  * Strong passwords: gMSAs use 240-byte, randomly generated complex passwords. The complexity and length of gMSA passwords minimizes the likelihood of a service getting compromised by brute force or dictionary attacks.
  * Support deployment to server farms: the ability to deploy gMSAs to multiple servers allows for the support of load balanced solutions where multiple hosts run the same service.
  * Support simplified service principal name (SPN) management.
* Remove the gMSA from privileged groups and grant the gMSA only the rights and permissions it requires to run its service.

### Standalone managed service accounts

* If you can't use a gMSA, use a `standalone Managed Service Account (sMSA)`. sMSAs are managed domain accounts that you use to help secure one or more services that run on a server.
* Unlike gMSAs, sMSAs run on only one server and they cannot be used in server clusters where a service is replicated on multiple cluster nodes. They can still be used for multiple services on an specific server.
* Use sMSAs when you have one or more services deployed to a single server and you can't use a group managed service account (gMSA).
* Although you can use sMSAs for more than one service, it's recommended that each service have its own identity for auditing purposes.
* Benefits of using sMSAs (same as gMSAs):
  * Cycle passwords regularly.
  * Strong passwords.
  * Support simplified service principal name (SPN) management.

### Computer accounts

* A computer account, or `LocalSystem account`, is a built-in, highly privileged account with access to virtually all resources on the local computer.
* The computer account's predefined name is `NT AUTHORITY\SYSTEM`.
* Services that run as a LocalSystem account access network resources by using the credentials of the computer account in the format `<domain_name>\<computer_name>`.
* Computer accounts are highly privileged accounts and should be used only when your service needs unrestricted access to local resources on the machine and you can't use a managed service account (MSA).

### User accounts

* If you can't use an MSA and you don't need a highly privileged account with access to virtually all resources on the local computer, consider using a user account.

#### Domain user accounts

* It enables the service to take full advantage of the service security features of Windows and Microsoft Active Directory Domain Services.
* The service will have local and network permissions granted to the account.
* It will also have the permissions of any groups of which the account is a member.
* Domain service accounts support Kerberos mutual authentication.

#### Local user account

* It exists only in the Security Account Manager (SAM) database of the host computer. It doesn't have a user object in Active Directory Domain Services.
* A local account can't be authenticated by the domain. So, a service that runs in the security context of a local user account doesn't have access to network resources (except as an anonymous user).
* Services that run in the local user context can't support Kerberos mutual authentication in which the service is authenticated by its clients. For this reason, local user accounts are ordinarily inappropriate for directory-enabled services.
* Service accounts shouldn't be members of any privileged groups, because privileged group membership confers permissions that might be a security risk.
* Each service should have its own local service account for auditing and security purposes.

##### Create a local user account as a service account

1. Create a new local user account, starting with `svc-`, and set the password to never expire. Use a complex password.
2. Add the user with whatever required permissions to accomplish your service task (e.g. give it permissions to the Apache folder).
3. Go to the `Local Security Policy` > `Local Policies` > `User Rights Assignment` and configure the account as `Log on as a service`. The `Log on as a service` user right allows accounts to start network services or services that run continuously on a computer, even when no one is logged on to the console. This allows services to run under a user accounts, and not in the context of a Local System, Local Service, or Network Service.
4. Go again to the `Local Security Policy` > `Local Policies` > `User Rights Assignment` and assign the `Deny log on locally` user right to the account. This policy setting determines which users are prevented from logging on directly at the device's console.
5. Go to `Administrative Tools` > `Services` to change the user account that runs the service (e.g. Apache service) or, if you need to install the application and it requests for it, enter the account as `computer name\svc-username`.

|Source: [*"Introduction to Active Directory service accounts" from Microsoft Docs*][1], [*"Secure on-premises computer accounts" from Microsoft Docs*][5], [*"Log on as a service" from Microsoft Docs*][6] and [*"Managing “Logon as a service” Group Policy" from TheITBros.*][7]|
|:--:|

## Further reading

* [What is the difference between user and service account?][3]

[1]: https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/service-accounts-on-premises
[2]: https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/service-accounts
[3]: https://unix.stackexchange.com/questions/314725/what-is-the-difference-between-user-and-service-account
[4]: https://www.youtube.com/watch?v=wHa_eh96UHg
[5]: https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/service-accounts-computer
[6]: https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/log-on-as-a-service
[7]: https://theitbros.com/logon-as-a-service/
