# Bra kommandon att komma ihåg för att skapa en uppfattning om nätet runt om dig
## Nätskanning
### nmap
Skanna portar för enskilda datorer eller nätverk
1. Basic Nmap scan
The basic Nmap is just running it with no flags, just the IP of the machine you are testing:
``` bash
root@Kali:~# nmap 10.211.55.6
Starting Nmap 7.80 ( https://nmap.org ) at 2020-02-20 17:11 EST
Nmap scan report for 10.211.55.6
Host is up (0.00030s latency).
Not shown: 992 filtered ports
PORT     STATE  SERVICE
20/tcp   closed ftp-data
21/tcp   open   ftp
22/tcp   open   ssh
53/tcp   open   domain
80/tcp   open   http
139/tcp  open   netbios-ssn
666/tcp  open   doom
3306/tcp open   mysql
```
This will scan your IP on a set of around 1,000 of the most common ports, determine if any are open or not, and then list the open ports. Nothing fancy, no determining what the application is, nothing interesting about the host — just a quick check for open ports.
​
2. OS Enumeration
Add an -O flag on the end to tell Nmap to try to figure out the server's operating system:
​
``` bash
root@Kali:~# nmap 10.211.55.6 -O
…
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 – 4.9
```
By the way, some output is going to be trimmed out from these examples to save space, this will be indicated by the ellipses.
​
It found a Linux machine but couldn't really narrow down on the kernel version. Nmap does this by looking at certain characteristics of the reply and compares it to known replies from specific OSes.
​
Other machines might return something more specific, like Windows 2008 R2 or Sun Solaris 11.1, but for Stapler we don't learn much beyond that it's a Linux box.
​
3. Scan all TCP ports
The -p flag is typically used to define a range of ports to scan (for example -p 100-200 would scan ports 100 through 200).  -p- however scans all 65535 TCP ports, helpful for finding services listening on weird, high ports like 12380 in this case.
``` bash
root@Kali:~# nmap 10.211.55.6 -p-
…
12380/tcp open   unknown
```
It might be nothing, or it might be a sysadmin thinking he's clever by hiding something on an odd port.
​
4. Default scripts
This is where the power of Nmap really starts to show. The Nmap Scripting Engine (NSE) allows anyone to add functionality to Nmap by means of scripts which can super charge Nmap to identify specific applications listening on ports, scan for known exploits against those applications, scan for common misconfigurations of services, and much more.
​
The -sC flag runs a set list of default scripts against your target. This can be a gold mine for a poorly configured server:
``` bash
root@Kali:~# nmap 10.211.55.6 -sC
..
21/tcp   open   ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 550 Permission denied.
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to 10.211.55.4
|      Logged in as ftp
…
|      vsFTPd 3.0.3 – secure, fast, stable
|_End of status
22/tcp   open   ssh
| ssh-hostkey:
|   2048 81:21:ce:a1:1a:05:b1:69:4f:4d:ed:80:28:e8:99:05 (RSA)
|   256 5b:a5:bb:67:91:1a:51:c2:d3:21:da:c0:ca:f0:db:9e (ECDSA)
|_  256 6d:01:b7:73:ac:b0:93:6f:fa:b9:89:e6:ae:3c:ab:d3 (ED25519)
53/tcp   open   domain
| dns-nsid:
|   id.server: ATL
|_  bind.version: dnsmasq-2.75
80/tcp   open   http
|_http-title: 404 Not Found
…
3306/tcp open   mysql
| mysql-info:
|   Protocol: 10
|   Version: 5.7.12-0ubuntu1
…
```
The complete output from Stapler is much longer, we just wanted to highlight a few juicy things it found. For example, the FTP server allows for anonymous connections, meaning you can connect without needing to login. Definitely make a note of that as something to check out later.  There's also the FTP server application's name and version. Note those so you can check for known exploits.
​
SSH is open and we get a public key, nothing too useful yet, but maybe we'll find some credentials later or get desperate and try to brute force a login. Dnsmasq and its version are worth checking for exploits. There's a web server running, definitely note that so we can do some further file and directory enumeration with Gobuster. Finally, we have a MySQL instance.  Put that in your notes, finding a credential could lead to later exfiltrating some interesting data.
​
That was a ton of information from just a single Nmap flag!  Again, good enumeration means you are in it for the log game, don't just immediately connect to FTP and see what files are in there. There's no better time waster than going down rabbit holes.
​
Speaking of enumeration, a pro tip is to use -o to output your scan results to a file, handy to get something to reference later without waiting on scans to rerun.
​
5. The All flag
Now what if you could combine those three flags into one all powerful flag? Well, that's as simple as using the -A flag.  That will run OS detection and defaults scripts while scanning all TCP ports, making it a nice time saver.
​
6. More scripts
The default scripts are great, but what if you wanted to attack the machine a little more brazenly? Along with default, there's a whole set of script categories you can throw against a machine with this flag: –script default,discovery,exploit,vuln.
​
It won't reveal much on Stapler as there aren't any vulnerabilities on the exposed services, but keep this flag in the back of your head.
​
7.  UDP scans
If you remember your Net+ training, you should remember that along with TCP, some network services also run on UDP. It's not near as common, but thorough enumeration always includes a check on that protocol as well.
​
UDP scanning is a lot slower than TCP, so it's recommended you use a flag like this, which will only scan the 250 most common UDP ports:
``` bash
root@Kali:~# nmap 10.211.55.6 -sU –top-ports 250
Starting Nmap 7.80 ( https://nmap.org ) at 2020-02-20 18:00 EST
…
PORT    STATE         SERVICE
53/udp  open|filtered domain
68/udp  open|filtered dhcpc
69/udp  open|filtered tftp
137/udp open          netbios-ns
138/udp open|filtered netbios-dgm 
```
In this case, scanning UDP revealed something potentially very interesting in the form of a TFTP server. Spoiler alert, connecting to the TFTP server gets you write access into the same directory as the root of the web server. Upload a PHP reverse shell file, set up a listener on Kali, connect to the file on the web site, and *BOOM* instant shell on the server as an unprivileged user. Good thing you scanned UDP ports, right?
​
Enumeration is super important in pentesting, and Nmap is always the first stop when doing enumeration. A simple port scan gives you the most basic information when pentesting any machine. Think about it, without knowing what ports are open, how else could you start poking and prying at a server?
​
There are many more ways to use Nmap, both powerful and nuanced, but these basics you'll come back to again and again. Get practicing on machines in Vulnhub or HackTheBox and don't ever forget: ENUMERATE, ENUMERATE, ENUMERATE!
​
### gobuster
Gobuster är till för att enumerera fram kataloger och filer på en webserver.
den kan matas med olika söklägen:
``` bash
root@kali:~# gobuster -h
Usage:
  gobuster [command]
​
Available Commands:
  dir         Uses directory/file enumeration mode
  dns         Uses DNS subdomain enumeration mode
  fuzz        Uses fuzzing mode
  help        Help about any command
  s3          Uses aws bucket enumeration mode
  version     shows the current version
  vhost       Uses VHOST enumeration mode
​
Flags:
  -P string
        Password for Basic Auth (dir mode only)
  -U string
        Username for Basic Auth (dir mode only)
  -a string
        Set the User-Agent string (dir mode only)
  -c string
        Cookies to use for the requests (dir mode only)
  -cn
        Show CNAME records (dns mode only, cannot be used with '-i' option)
  -e    Expanded mode, print full URLs
  -f    Append a forward-slash to each directory request (dir mode only)
  -fw
        Force continued operation when wildcard found
  -i    Show IP addresses (dns mode only)
  -k    Skip SSL certificate verification
  -l    Include the length of the body in the output (dir mode only)
  -m string
        Directory/File mode (dir) or DNS mode (dns) (default "dir")
  -n    Don't print status codes
  -np
        Don't display progress
  -o string
        Output file to write results to (defaults to stdout)
  -p string
        Proxy to use for requests [http(s)://host:port] (dir mode only)
  -q    Don't print the banner and other noise
  -r    Follow redirects
  -s string
        Positive status codes (dir mode only) (default "200,204,301,302,307,403")
  -t int
        Number of concurrent threads (default 10)
  -to duration
        HTTP Timeout in seconds (dir mode only) (default 10s)
  -u string
        The target URL or Domain
  -v    Verbose output (errors)
  -w string
        Path to the wordlist
  -x string
        File extension(s) to search for (dir mode only)
```
En sökning efter kataloger kan se ut enligt
``` bash
root@kali:~# gobuster -e -u http://192.168.0.155/ -w /usr/share/wordlists/dirb/common.txt
​
Gobuster v1.2                OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://192.168.0.155/
[+] Threads      : 10
[+] Wordlist     : /usr/share/wordlists/dirb/common.txt
[+] Status codes : 301,302,307,200,204
[+] Expanded     : true
=====================================================
http://192.168.0.155/blog (Status: 301)
http://192.168.0.155/index.html (Status: 200)
http://192.168.0.155/index (Status: 200)
http://192.168.0.155/photo (Status: 301)
http://192.168.0.155/wordpress (Status: 301)
=====================================================
```
filen `common.txt` kommer från SecList projektet på github
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/common.txt
​
### wpscan
WPScan används enskilt för att skanna wordpress installtioner efter användare, sårbarheter, plugins, teman och databasdumpar mm
Börja alltid med att uppdatera  listan över sårbarheter
`wpscan --update`
​
``` bash
root@kali:~# wpscan -h
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|
​
         WordPress Security Scanner by the WPScan Team
                         Version 3.8.22
                               
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________
​
Usage: wpscan [options]
      --url URL                                 The URL of the blog to scan
                                                Allowed Protocols: http, https
                                                Default Protocol if none provided: http
                                                This option is mandatory unless update or help or hh or version is/are supplied
  -h, --help                                    Display the simple help and exit
      --hh                                      Display the full help and exit
      --version                                 Display the version and exit
  -v, --verbose                                 Verbose mode
      --[no-]banner                             Whether or not to display the banner
                                                Default: true
  -o, --output FILE                             Output to FILE
  -f, --format FORMAT                           Output results in the format supplied
                                                Available choices: cli-no-colour, cli-no-color, cli, json
      --detection-mode MODE                     Default: mixed
                                                Available choices: mixed, passive, aggressive
      --user-agent, --ua VALUE
      --random-user-agent, --rua                Use a random user-agent for each scan
      --http-auth login:password
  -t, --max-threads VALUE                       The max threads to use
                                                Default: 5
      --throttle MilliSeconds                   Milliseconds to wait before doing another web request. If used, the max threads will be set to 1.
      --request-timeout SECONDS                 The request timeout in seconds
   ...