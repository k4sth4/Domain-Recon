# Domain-Recon
To carry a successfull attack in Active Directory Environment, one should need to enumerate the Domain. We will use [Powerview.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1) module to enumerate the Domain.

## Reconnaissance

First Import the Powerview.ps1 module.
```markdown
import-module Powerview.ps1
```
```markdown
. .\Powerview.ps1
```


1. Get-Domain Useful information includes the domain name, the forest name and the domain controllers.
```markdown
Get-Domain
```

2. Get-DomainController Returns the domain controllers for the current or specified domain.
```markdown
Get-DomainController | select Forest, Name, OSVersion | fl
```

3. Get-ForestDomain Returns all domains for the current forest or the forest specified by -Forest
```markdown
Get-ForestDomain
```

4. Get-DomainPolicyData Useful for finding information such as the domain password policy.
```markdown
Get-DomainPolicyData | select -ExpandProperty SystemAccess
```

5. Get-DomainUser Return all (or specific) user(s).
```markdown
Get-DomainUser -Identity john -Properties DisplayName, MemberOf | fl
```

6. Get-DomainComputer Return all computers or specific computer objects.
```markdown
Get-DomainComputer -Properties DnsHostName | sort -Property DnsHostName
```

7. Get-DomainOU Search for all organization units (OUs) or specific OU objects.
```markdown
Get-DomainOU -Properties Name | sort -Property Name
```

8. Get-DomainGroup Return all groups or specific group objects.
```markdown
Get-DomainGroup | where Name -like "*Admins*" | select SamAccountName
```

9. Get-DomainGroupMember Return the members of a specific domain group.
```markdown
Get-DomainGroupMember -Identity "Domain Admins" | select MemberDistinguishedName
```

10. Get-DomainGPO Return all Group Policy Objects (GPOs) or specific GPO objects.
```markdown
Get-DomainGPO -Properties DisplayName | sort -Property DisplayName
```
(To enumerate all GPOs that are applied to a particular machine, use -ComputerIdentity.)
```markdown
Get-DomainGPO -ComputerIdentity wkstn-1 -Properties DisplayName | sort -Property DisplayName
```

11. Get-DomainGPOLocalGroup Returns all GPOs that modify local group membership.
```markdown
Get-DomainGPOLocalGroup | select GPODisplayName, GroupName
```

12. Get-DomainGPOUserLocalGroupMapping Enumerates the machines where a specific domain user/group is a member of a specific local group.
```markdown
Get-DomainGPOUserLocalGroupMapping -LocalGroup Administrators | select ObjectName, GPODisplayName, ContainerName, ComputerName
```

13. Find-DomainUserLocation finds domain machines where those users are logged in (default domain admin)
```markdown
Find-DomainUserLocation | select UserName, SessionFromName
```

14. Get-NetSession Returns session information for the local (or a remote) machine (where CName is the source IP).
```markdown
Get-NetSession -ComputerName dc01 | select CName, UserName
```

15. Get-DomainTrust Return all domain trusts for the current or specified domain.
```markdown
Get-DomainTrust
```

16. Find-DomainShare will find SMB shares in a domain and -CheckShareAccess will only display those that the executing principal has access to.
```markdown
Find-DomainShare -ComputerDomain hackershell.io -CheckShareAccess
```

(To Get The Writable Share In a Domain)
```markdown
Find-DomainShare -CheckShareAccess
```

