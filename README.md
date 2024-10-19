<h1 align="center">
  Vulnerable-AD
  <br>
</h1>

Create a vulnerable active directory that's allowing you to test most of active directory attacks in local lab

### Main Features
- Randomize Attacks
- you need run the script in DC with Active Directory installed 
  
### Supported Attacks
- Abusing ACLs/ACEs
- Kerberoasting
- AS-REP Roasting
- Abuse DnsAdmins
- Password in Object Description
- User Objects With Default password (Changeme123!)
- Password Spraying
- DCSync
- Silver Ticket
- Golden Ticket 
- Pass-the-Hash
- Pass-the-Ticket
- SMB Signing Disabled

### Example
```powershell
# if you didn't install Active Directory yet , you can try 

# Set static IPv4 address
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress "10.10.1.10" -PrefixLength 24 -DefaultGateway "10.10.1.1"

# Set DNS server (pointing to itself since it will be a DC)
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses "10.10.1.10"

# Install AD Domain Services
Install-WindowsFeature AD-Domain-Services

# Import AD DS Deployment module
Import-Module ADDSDeployment

# Create new Forest
Install-ADDSForest `
    -CreateDnsDelegation:$false `
    -DatabasePath "C:\Windows\NTDS" `
    -DomainMode "7" `
    -DomainName "red.invoke" `
    -DomainNetbiosName "red" `
    -ForestMode "7" `
    -InstallDns:$true `
    -LogPath "C:\Windows\NTDS" `
    -NoRebootOnCompletion:$false `
    -SysvolPath "C:\Windows\SYSVOL" `
    -Force:$true

# After reboot, run the vulnerable AD script
IEX((new-object net.webclient).downloadstring("https://raw.githubusercontent.com/Omaar1/vulnerable-AD/master/vulnad.ps1"))
Invoke-VulnAD -UsersLimit 100 -DomainName "red.invoke"
```

