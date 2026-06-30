#web_technologies 

# Web Request

![[Pasted image 20260627124716.png]]

| #   | Aktion                                                          | Initiator | Protokoll/Dienst |
| --- | --------------------------------------------------------------- | --------- | ---------------- |
| 1   | URL www.fh-ooe.at zerlegen                                      | Browser   | -                |
| 2   | Laptop benötigt eine IP-Adresse im Netz                         | Laptop    | DHCP             |
| 3   | Hostname www.fh-ooe.at muss in eine IP-Adresse übersetzt werden | Browser   | DNS              |
| 4   | Hardware-Adresse (MAC) des Routers/Gateways erfragen            | OS        | ARP              |
| 5   | TCP-Verbindung zum Webserver auf Port 443 (HTTPS) aufbauen*     | Browser   | TCP              |
| 6   | Verschlüsselten Kanal per TLS verhandeln*                       | Browser   | TLS              |
| 7   | HTTP-Request verschicken*                                       | Browser   | HTTP             |
| 8   | Request verarbeiten (z.B. mit PHP) und HTTP-Response schicken*  | Webserver | HTTP             |
| 9   | HTML parsen und Anzeigen                                        | Browser   | -                |

# URL

![[Pasted image 20260627124909.png]]

- Schema 
- http auth
- FQDN - Host
- Port
- path
- Query  String
- Fragment (jumps to id in html)

# Client Server Model

![[Pasted image 20260627125105.png]]

# OSI Model

- Application
- Presentation
 - Session
 - Transport
 - Network
 - Data Link
 - Physical 

## TCP / IP

- Application
- Transport
- Network
- Link

# DNS

- TLD 
- SLD
- Subdomain

![[Pasted image 20260627130105.png]]

![[Pasted image 20260627130003.png]]

# Network Layer

IP-Address

192.168.0.1/24

Subnetting, ik this

## IP Modes

- Unicast
- Broadcast
- Multicast
## NAT

natting

## DHCP

bleh

## ARP

dns for macs

## Ethernet

physical stuff

Frames

# Transport Layer


TCP and UDP

SYN 

ACK, SYN

ACK

# HTTPs

TLS → Transüprt layer security

tls hadnshake

→ encryption stuff and rng value

← certificate

→ is it valid

←→ symmteric encryption key switch

→ finished 

# HTTP


![[Pasted image 20260627132046.png]]

![[Pasted image 20260627132053.png]]