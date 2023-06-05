# Automated Discovery
Automated discovery is the process of using tools to discover content rather than doing it manually. This process is automated as it usually contains hundreds, thousands or even millions of requests to a web server. These requests check whether a file or directory exists on a website, giving us access to resources we didn't previously know existed. This process is made possible by using a resource called wordlists.

use https://github.com/danielmiessler/SecLists

## Files and folders
### ffuf
``` bash
ffuf -w SecLists/Discovery/Web-Content/common.txt -u http://10.10.51.203/FUZZ

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.5.0 Kali Exclusive <3
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.51.203/FUZZ
 :: Wordlist         : FUZZ: SecLists/Discovery/Web-Content/common.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500
________________________________________________

assets                  [Status: 301, Size: 178, Words: 6, Lines: 8, Duration: 47ms]
contact                 [Status: 200, Size: 3108, Words: 747, Lines: 65, Duration: 61ms]
customers               [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 51ms]
development.log         [Status: 200, Size: 27, Words: 5, Lines: 1, Duration: 51ms]
monthly                 [Status: 200, Size: 28, Words: 4, Lines: 1, Duration: 58ms]
news                    [Status: 200, Size: 2538, Words: 518, Lines: 51, Duration: 51ms]
private                 [Status: 301, Size: 178, Words: 6, Lines: 8, Duration: 52ms]
robots.txt              [Status: 200, Size: 46, Words: 4, Lines: 3, Duration: 47ms]
sitemap.xml             [Status: 200, Size: 1383, Words: 260, Lines: 43, Duration: 58ms]
:: Progress: [4713/4713] :: Job [1/1] :: 686 req/sec :: Duration: [0:00:07] :: Errors: 0 ::
```
### dirb
``` bash
dirb http://10.10.51.203/ SecLists/Discovery/Web-Content/common.txt

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sat Aug 20 15:40:06 2022
URL_BASE: http://10.10.51.203/
WORDLIST_FILES: SecLists/Discovery/Web-Content/common.txt

-----------------

GENERATED WORDS: 4712                                                          

---- Scanning URL: http://10.10.51.203/ ----
==> DIRECTORY: http://10.10.51.203/assets/                                                                                                                                                                       
+ http://10.10.51.203/contact (CODE:200|SIZE:3108)                                                                                                                                                               
+ http://10.10.51.203/customers (CODE:302|SIZE:0)                                                                                                                                                                
+ http://10.10.51.203/development.log (CODE:200|SIZE:27)                                                                                                                                                         
+ http://10.10.51.203/monthly (CODE:200|SIZE:28)                                                                                                                                                                 
+ http://10.10.51.203/news (CODE:200|SIZE:2538)                                                                                                                                                                  
==> DIRECTORY: http://10.10.51.203/private/                                                                                                                                                                      
+ http://10.10.51.203/robots.txt (CODE:200|SIZE:46)                                                                                                                                                              
+ http://10.10.51.203/sitemap.xml (CODE:200|SIZE:1383)                                                                                                                                                           
                                                                                                                                                                                                                 
---- Entering directory: http://10.10.51.203/assets/ ----
==> DIRECTORY: http://10.10.51.203/assets/avatars/                                                                                                                                                               
                                                                                                                                                                                                                 
---- Entering directory: http://10.10.51.203/private/ ----
+ http://10.10.51.203/private/index.php (CODE:200|SIZE:49)                                                                                                                                                       
                                                                                                                                                                                                                 
---- Entering directory: http://10.10.51.203/assets/avatars/ ----
                                                                                                                                                                                                                 
-----------------
END_TIME: Sat Aug 20 15:58:18 2022
DOWNLOADED: 18848 - FOUND: 8
```
### gobuster
``` bash
gobuster dir --url http://10.10.51.203/ -w SecLists/Discovery/Web-Content/common.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.51.203/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                SecLists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/08/20 15:53:49 Starting gobuster in directory enumeration mode
===============================================================
/assets               (Status: 301) [Size: 178] [--> http://10.10.51.203/assets/]
/contact              (Status: 200) [Size: 3108]                                 
/customers            (Status: 302) [Size: 0] [--> /customers/login]             
/development.log      (Status: 200) [Size: 27]                                   
/monthly              (Status: 200) [Size: 28]                                   
/news                 (Status: 200) [Size: 2538]                                 
/private              (Status: 301) [Size: 178] [--> http://10.10.51.203/private/]
/robots.txt           (Status: 200) [Size: 46]                                    
/sitemap.xml          (Status: 200) [Size: 1383]                                  
                                                                                  
===============================================================
2022/08/20 15:54:16 Finished
===============================================================
```
## Virtual Hosts
### ffuf
Man kan med ffuf göra anrop med en ordlista för att försöka hitta virtual hosts som finns på en webserver. Detta genom att sätta host-headern
``` bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP
```
Alla träffar kommer att få en returkod 200 så när man har kört ett tag han man kolla storleken på svaret för majoriten av anropen och sen filtrera ut dem

``` bash
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP -fs {size}
```
## Username enumeration
### ffuf

``` bash
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.183.238/customers/signup -mr "username already exists"
```
-w är ordlistan som ska användas  
-X är http request metoden  
-d är datat som ska skickas in och där FUZZ är det som byts ut mot orden i ordlistan  
-H är http headern  
-u är den url som ska anropas  
-mr är det regexp vi ska matcha mot för att se det som en lyckad träff  

## Brute force
### ffuf
``` bash
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.183.238/customers/login -fc 200
``` 