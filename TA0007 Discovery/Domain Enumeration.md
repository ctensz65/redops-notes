 - Via powershell and cmd.exe
### Listing DCs
```
# Using nslookup 
nslookup -type=SRV _ldap._tcp.dc._msdcs.DOMAIN.com 
nslookup -type=SRV _kerberos._tcp.DOMAIN.com

# Using nltest 
nltest /dclist:DOMAIN.com

# Using set command to get domain info 
set | findstr USERDOMAIN

# DC Name
echo %logonserver%
```

### Domain Info
```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
[System.Net.Dns]::GetHostByName($env:COMPUTERNAME)
wmic computersystem get domain
nltest /domain_trusts
systeminfo | findstr Domain
```

### User and Group Enumeration
```
net user /domain
net user USERNAME /domain
net group /domain
net group "Domain Admins" /domain
net group "Enterprise Admins" /domain
Get-LocalGroup
```

### Network and Share Discovery
```
## List network shares
net share

## View domain computers
net view /domain

## List cached network connection
net use

## Network discovery
net view \\COMPUTER_NAME 
```

### Service and Process Discovery
```
## List services
tasklist /V
sc query

## Get running processes with user context
tasklist /v

## Query specific services
sc qc "ServiceName"
```