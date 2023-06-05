# knäcka lösenord
Det finns ett antal olika verktyg för att knäcka lösenord. Här är några av dem.
## hash-id
För att kunna knäcka en lösenordshash behöver man veta vilken typ att algoritm som används. Till detta finns ett pythonskript `hash-id.py`
Kör detta genom att köra `python3 hash-id.py hash`  
T.ex. 
``` bash
$ python3 hash-id.py b6b0d451bbf6fed658659a9e7e5598fe
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

Least Possible Hashs:
[+] RAdmin v2.x
[+] NTLM
[+] MD4
[+] MD2
[+] MD5(HMAC)
[+] MD4(HMAC)
[+] MD2(HMAC)
[+] MD5(HMAC(Wordpress))
[+] Haval-128
[+] Haval-128(HMAC)
[+] RipeMD-128
[+] RipeMD-128(HMAC)
[+] SNEFRU-128
[+] SNEFRU-128(HMAC)
[+] Tiger-128
[+] Tiger-128(HMAC)
[+] md5($pass.$salt)
[+] md5($salt.$pass)
[+] md5($salt.$pass.$salt)
[+] md5($salt.$pass.$username)
[+] md5($salt.md5($pass))
[+] md5($salt.md5($pass))
[+] md5($salt.md5($pass.$salt))
[+] md5($salt.md5($pass.$salt))
[+] md5($salt.md5($salt.$pass))
[+] md5($salt.md5(md5($pass).$salt))
[+] md5($username.0.$pass)
[+] md5($username.LF.$pass)
[+] md5($username.md5($pass).$salt)
[+] md5(md5($pass))
[+] md5(md5($pass).$salt)
[+] md5(md5($pass).md5($salt))
[+] md5(md5($salt).$pass)
[+] md5(md5($salt).md5($pass))
[+] md5(md5($username.$pass).$salt)
[+] md5(md5(md5($pass)))
[+] md5(md5(md5(md5($pass))))
[+] md5(md5(md5(md5(md5($pass)))))
[+] md5(sha1($pass))
[+] md5(sha1(md5($pass)))
[+] md5(sha1(md5(sha1($pass))))
[+] md5(strtoupper(md5($pass)))
--------------------------------------------------
```
## john-the-ripper
Ett bra verktyg för att knäcka själva hashen är `john`.
Det går att använda utan att först identifiera algoritmen och man kör det då på detta sätt.  
`john --wordlist=[path to wordlist] [path to file]`  
T.ex.
``` bash
$ john --wordlist=rockyou.txt first_task_hashes/hash1.txt 
Warning: detected hash type "LM", but the string is also recognized as "dynamic=md5($p)"
Use the "--format=dynamic=md5($p)" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "HAVAL-128-4"
Use the "--format=HAVAL-128-4" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "MD2"
Use the "--format=MD2" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "mdc2"
Use the "--format=mdc2" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "mscash"
Use the "--format=mscash" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "mscash2"
Use the "--format=mscash2" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "NT"
Use the "--format=NT" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "NT-long"
Use the "--format=NT-long" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "Raw-MD4"
Use the "--format=Raw-MD4" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "Raw-MD5"
Use the "--format=Raw-MD5" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "Raw-MD5u"
Use the "--format=Raw-MD5u" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "Raw-SHA1-AxCrypt"
Use the "--format=Raw-SHA1-AxCrypt" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "ripemd-128"
Use the "--format=ripemd-128" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "Snefru-128"
Use the "--format=Snefru-128" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "ZipMonster"
Use the "--format=ZipMonster" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "LM-opencl"
Use the "--format=LM-opencl" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "mscash-opencl"
Use the "--format=mscash-opencl" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "mscash2-opencl"
Use the "--format=mscash2-opencl" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "NT-opencl"
Use the "--format=NT-opencl" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "raw-MD4-opencl"
Use the "--format=raw-MD4-opencl" option to force loading these as that type instead
Warning: detected hash type "LM", but the string is also recognized as "raw-MD5-opencl"
Use the "--format=raw-MD5-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Using default target encoding: CP850
Loaded 2 password hashes with no different salts (LM [DES 256/256 AVX2])
Warning: poor OpenMP scalability for this hash type, consider --fork=4
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
0g 0:00:00:02 DONE (2022-06-18 20:48) 0g/s 3746Kp/s 3746Kc/s 7492KC/s !!1GOOD..*7¡VA
Session completed.
```
Om detta inte lyckas får man kolla upp vilket formatet på hashen är med t.ex. hash-id. I det här fallet är det `md5`
man skickas sedan in parametern `--format=kod`. En lista över alla format får genom kommandot `john --list=formats`
``` bash
$ john --list=formats
descrypt, bsdicrypt, md5crypt, md5crypt-long, bcrypt, scrypt, LM, AFS, 
tripcode, AndroidBackup, adxcrypt, agilekeychain, aix-ssha1, aix-ssha256, 
aix-ssha512, andOTP, ansible, argon2, as400-des, as400-ssha1, asa-md5, 
AxCrypt, AzureAD, BestCrypt, BestCryptVE4, bfegg, Bitcoin, BitLocker, 
bitshares, Bitwarden, BKS, Blackberry-ES10, WoWSRP, Blockchain, chap, 
Clipperz, cloudkeychain, dynamic_n, cq, CRC32, cryptoSafe, sha1crypt, 
sha256crypt, sha512crypt, Citrix_NS10, dahua, dashlane, diskcryptor, Django, 
django-scrypt, dmd5, dmg, dominosec, dominosec8, DPAPImk, dragonfly3-32, 
dragonfly3-64, dragonfly4-32, dragonfly4-64, Drupal7, eCryptfs, eigrp, 
electrum, ENCDataVault-MD5, ENCDataVault-PBKDF2, EncFS, enpass, EPI, 
EPiServer, ethereum, fde, Fortigate256, Fortigate, FormSpring, FVDE, geli, 
gost, gpg, HAVAL-128-4, HAVAL-256-3, hdaa, hMailServer, hsrp, IKE, ipb2, 
itunes-backup, iwork, KeePass, keychain, keyring, keystore, known_hosts, 
krb4, krb5, krb5asrep, krb5pa-sha1, krb5tgs, krb5-17, krb5-18, krb5-3, 
kwallet, lp, lpcli, leet, lotus5, lotus85, LUKS, MD2, mdc2, MediaWiki, 
monero, money, MongoDB, scram, Mozilla, mscash, mscash2, MSCHAPv2, 
mschapv2-naive, krb5pa-md5, mssql, mssql05, mssql12, multibit, mysqlna, 
mysql-sha1, mysql, net-ah, nethalflm, netlm, netlmv2, net-md5, netntlmv2, 
netntlm, netntlm-naive, net-sha1, nk, notes, md5ns, nsec3, NT, NT-long, 
o10glogon, o3logon, o5logon, ODF, Office, oldoffice, OpenBSD-SoftRAID, 
openssl-enc, oracle, oracle11, Oracle12C, osc, ospf, Padlock, Palshop, 
Panama, PBKDF2-HMAC-MD4, PBKDF2-HMAC-MD5, PBKDF2-HMAC-SHA1, 
PBKDF2-HMAC-SHA256, PBKDF2-HMAC-SHA512, PDF, PEM, pfx, pgpdisk, pgpsda, 
pgpwde, phpass, PHPS, PHPS2, pix-md5, PKZIP, po, postgres, PST, PuTTY, 
pwsafe, qnx, RACF, RACF-KDFAES, radius, RAdmin, RAKP, rar, RAR5, Raw-SHA512, 
Raw-Blake2, Raw-Keccak, Raw-Keccak-256, Raw-MD4, Raw-MD5, Raw-MD5u, Raw-SHA1, 
Raw-SHA1-AxCrypt, Raw-SHA1-Linkedin, Raw-SHA224, Raw-SHA256, Raw-SHA3, 
Raw-SHA384, restic, ripemd-128, ripemd-160, rsvp, RVARY, Siemens-S7, 
Salted-SHA1, SSHA512, sapb, sapg, saph, sappse, securezip, 7z, Signal, SIP, 
skein-256, skein-512, skey, SL3, Snefru-128, Snefru-256, LastPass, SNMP, 
solarwinds, SSH, sspr, Stribog-256, Stribog-512, STRIP, SunMD5, SybaseASE, 
Sybase-PROP, tacacs-plus, tcp-md5, telegram, tezos, Tiger, tc_aes_xts, 
tc_ripemd160, tc_ripemd160boot, tc_sha512, tc_whirlpool, vdi, OpenVMS, vmx, 
VNC, vtp, wbb3, whirlpool, whirlpool0, whirlpool1, wpapsk, wpapsk-pmk, 
xmpp-scram, xsha, xsha512, zed, ZIP, ZipMonster, plaintext, has-160, 
HMAC-MD5, HMAC-SHA1, HMAC-SHA224, HMAC-SHA256, HMAC-SHA384, HMAC-SHA512, 
AndroidBackup-opencl, agilekeychain-opencl, ansible-opencl, axcrypt-opencl, 
axcrypt2-opencl, bcrypt-opencl, Bitcoin-opencl, BitLocker-opencl, 
bitwarden-opencl, blockchain-opencl, cloudkeychain-opencl, md5crypt-opencl, 
cryptosafe-opencl, sha1crypt-opencl, sha256crypt-opencl, sha512crypt-opencl, 
dashlane-opencl, descrypt-opencl, diskcryptor-opencl, diskcryptor-aes-opencl, 
dmg-opencl, electrum-modern-opencl, EncFS-opencl, enpass-opencl, 
ethereum-opencl, ethereum-presale-opencl, FVDE-opencl, geli-opencl, 
gpg-opencl, iwork-opencl, KeePass-opencl, keychain-opencl, keyring-opencl, 
keystore-opencl, krb5pa-md5-opencl, krb5pa-sha1-opencl, krb5asrep-aes-opencl, 
lp-opencl, lpcli-opencl, LM-opencl, lotus5-opencl, mscash-opencl, 
mscash2-opencl, mysql-sha1-opencl, notes-opencl, NT-opencl, ntlmv2-opencl, 
o5logon-opencl, ODF-opencl, office-opencl, oldoffice-opencl, 
OpenBSD-SoftRAID-opencl, PBKDF2-HMAC-SHA256-opencl, 
PBKDF2-HMAC-SHA512-opencl, PBKDF2-HMAC-MD4-opencl, PBKDF2-HMAC-MD5-opencl, 
PBKDF2-HMAC-SHA1-opencl, pem-opencl, pfx-opencl, pgpdisk-opencl, 
pgpsda-opencl, pgpwde-opencl, phpass-opencl, pwsafe-opencl, RAKP-opencl, 
rar-opencl, RAR5-opencl, raw-MD4-opencl, raw-MD5-opencl, raw-SHA1-opencl, 
raw-SHA256-opencl, raw-SHA512-free-opencl, raw-SHA512-opencl, 
salted-SHA1-opencl, sappse-opencl, 7z-opencl, SL3-opencl, solarwinds-opencl, 
ssh-opencl, sspr-opencl, strip-opencl, TrueCrypt-opencl, telegram-opencl, 
tezos-opencl, vmx-opencl, wpapsk-opencl, wpapsk-pmk-opencl, 
XSHA512-free-opencl, XSHA512-opencl, zed-opencl, ZIP-opencl, dummy, crypt
510 formats (149 dynamic formats shown as just "dynamic_n" here)
```

Så med formatet raw-md5 blir resultatet detta
``` bash
$ john --format=raw-md5 --wordlist=rockyou.txt first_task_hashes/hash1.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
Warning: no OpenMP support for this hash type, consider --fork=4
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
biscuit          (?)     
1g 0:00:00:00 DONE (2022-06-18 20:49) 50.00g/s 134400p/s 134400c/s 134400C/s skyblue..nugget
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed.
```

### unshadow
För att `john` ska klara av att knäcka en hash från `/etc/shadow` behöver den information från `/etc/passwd`. För att få ihop denna information kör man kommandot `unshadow` (som ska komma med john-the-ripper).

Kör det med kommando `unshadow [path to passwd] [pass to shadow]`
``` bash
$ unshadow /etc/passwd /etc/password > unshadowed.txt
```
Sen går det att köra john och peka ut filen. Formatet för hashen är `sha512crypt`

``` bash
$ john --wordlist=rockyou.txt --format=sha512crypt unshadowed.txt
```

### single mode
john kan köra i ett så kallat single-läge `--single` där man då testar olika kombinationer av användarnamet för att hitta lösenordet. Med användarnamnet Markus kan t.ex dessa testas
* Markus1, Markus2, Markus3 (etc.)
* MArkus, MARkus, MARKus (etc.)
* Markus!, Markus$, Markus* (etc.)

Man kör single-mode så här `john --single --format=[format] [path to file]`

Man måste först lägga in användaren före hashen enligt `[user]:[hash]` t.ex.`markus:1efee03cdcb96d90ad48ccc7b8666033`
t.ex
``` bash
$ john --single --format=raw-sha256 hashes.txt
```
### Engendefinerade lösenordsregler
Man kan skriva egna regler för hur `john` ska generera lösenord. Detta kommer väl till pass ifall man vet vilken policy som gäller för lösenorden på ett företag. Om policyn t.ex. säger följande:
* Minst en stor bokstav
* Minst en siffra
* Minst ett specialtecken  

Då tenderar folk att välja lösenord enligt:  
_Sommar2022!_  
#### Definera reglerna
Man lägger in reglerna i filen _john.conf_ (i _/etc/john.conf_, _/opt/john.conf_ eller _/var/lib/snapd/snap/john-the-ripper/535/run/john.conf_). Fulla regler för hur de defineras finns här: https://www.openwall.com/john/doc/RULES.shtml.  

Börja med att definera regeln med `[List.Rules:myRule]` och sen används en regex liknande syntax.  
` Az ` - Tar ordet och lägger till de bokstäver du definerar _efter_ ordet.  
` A0 ` - Tar ordet och lägger till de bokstäver du definerar _före_ ordet.  
` c ` - Gör stor bokstav

Dessa regler kan användas för att definera vika delar av lösenordet du vill modifiera.  
Slutligen måste man definera vilka tecken som ska läggas till och detta görs med set av ` [] ` följt av teckenmönstret inom ` " " `.

` [0-9] ` - Lägger till siffror  
` [0] ` - Lägger endast siffran 0  
` [A-z] ` - Lägger till både stora och små bokstäver  
` [A-Z] ` - Lägger till endast stora bokstäver  
` [a-z] ` - Lägger till endast små bokstäver  
` [a] ` - Lägger endast till bokstaven a  
` [!£$%@] ` - Lägger till tecknen !£$%@

För att generera en regel för _Sommar2022!_ ser blocket ut enligt  
``` bash
[List.Rules:myRule]
cAz"[0-9] [!£$%@]"
```
#### Använda regeln
För att sen använda regel lägger man på argumentet ` --rule=myRule ` så att kommandot blir  
`john --wordlist=[path to wordlist] --rule=myRule [path to file]`

### zip2john
Man kan också använda `john` till att knäcka lösenord för lösenordsskyddade zip-filer. Man måste då först konvertera zip-filen till nåt john kan förstå. Detta görs enligt följande `zip2john [options] [zip file] > [output file]` t.ex `zip2john zipfile.zip > zip_hash.txt`

När man har genererar lösenordshashen kör man `john` med filen som input  
`john --wordlist=rockyou.txt zip_hash.txt`

### rar2john
Man kan också använda `john` till att knäcka lösenord för lösenordsskyddade rar-filer. Man måste då först konvertera rar-filen till nåt john kan förstå. Detta görs enligt följande `rar2john [rar file] > [output file]` t.ex `rar2john rarfile.zip > rar_hash.txt`

När man har genererar lösenordshashen kör man `john` med filen som input  
`john --wordlist=rockyou.txt rar_hash.txt`

### ssh2john
Man kan också använda `john` till att knäcka lösenord för ssh-nycklar. Man måste då först konvertera den privata nyckeln till nåt john kan förstå. Detta görs enligt följande `ssh2john[privat nyckel] > [output file]` t.ex `ssh2john id_rsa > id_rsa_hash.txt`

När man har genererar lösenordshashen kör man `john` med filen som input  
`john --wordlist=rockyou.txt id_rsa_hash.txt`