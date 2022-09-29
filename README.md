# TP2 : Ethernet, IP, et ARP

## **I. Setup IP**

voici les info de la lan nouvellement formé :

```
/- 192.168.0.0/26
|
\- 192.168.0.63/26

masque du sous réseau /26 = 255.255.255.192
```

voici nos adresses ip
```
Moi = 192.168.0.1/26
l'autre pc = 192.168.0.2/26
```

* ***Pour changer d'adresse ip:***

Cette commande définit la carte souhaitée, l'adresse statique, le masque de réseau et enfin la passerelle.

```
netsh interface ipv4 set address name="Ethernet" static 192.168.0.2 255.255.255.192 192.168.0.1
```

* ***Pour remettre la configuration DHCP automatique:***
```
netsh interface ipv4 set address name="Ethernet" source=dhcp
```

Nous pouvons voir que le ping entre les deux machines via ethernet est possible.

```
PS C:\Users\baptb> ping 192.168.0.1

Envoi d’une requête 'Ping'  192.168.0.1 avec 32 octets de données :
Réponse de 192.168.0.1 : octets=32 temps=3 ms TTL=128
Réponse de 192.168.0.1 : octets=32 temps=1 ms TTL=128
Réponse de 192.168.0.1 : octets=32 temps=2 ms TTL=128
Réponse de 192.168.0.1 : octets=32 temps=1 ms TTL=128

Statistiques Ping pour 192.168.0.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 1ms, Maximum = 3ms, Moyenne = 1ms
```
les différents types de trames sont des trames ICMPv6 "Multicast Listener Report Message v2", "Neighbor Solicitation..." et "Router Solicitation" ainsi que des trames ICMP "Echo (ping) request"

[ping_tp_2.pcapng](https://github.com/llGrMell/TP2-Ethernet-IP-et-ARP/blob/main/ping_tp_2.pcapng) 

## **II. ARP my bro**

On utilise la commande si dessous pour afficher la table ARP 

```
arp -a
```

```
Interface : 10.33.18.83 --- 0x10
  Adresse Internet      Adresse physique      Type
  ...

Interface : 192.168.0.2 --- 0x11
  Adresse Internet      Adresse physique      Type
  192.168.0.1           e4-a8-df-f8-e1-35     dynamique
  192.168.0.2           e4-a8-df-f8-e1-35     dynamique
  192.168.0.63          ff-ff-ff-ff-ff-ff     statique
  ...
```


ipconfig /all
```
Carte Ethernet Ethernet :
...
Passerelle par défaut. . . . . . . . . : 192.168.0.1
...
```
on suprime la table arp avec la commande si dessous: 
```
PS C:\Users\baptb> arp -d
```

on regarde ce qu'il y a dedans:
```
PS C:\Users\baptb> arp -a

Interface : 10.33.18.83 --- 0x10
  Adresse Internet      Adresse physique      Type
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
  224.0.0.22            01-00-5e-00-00-16     statique
  230.0.0.1             01-00-5e-00-00-01     statique

Interface : 192.168.0.2 --- 0x11
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
  230.0.0.1             01-00-5e-00-00-01     statique
```
on ping pour ajouter et remplire le tableau arp:
```
PS C:\Users\baptb> ping 192.168.0.3
  ...

Interface : 10.33.18.83 --- 0x10
  Adresse Internet      Adresse physique      Type
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.0.2 --- 0x11
  Adresse Internet      Adresse physique      Type
  192.168.0.3           e4-a8-df-f8-e1-35     dynamique
  224.0.0.2             01-00-5e-00-00-02     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  230.0.0.1             01-00-5e-00-00-01     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```
Après avoir vidé la table ARP et fait un ping, on émet un protocole ARP en destination du broadcastjusqu'a trouver d'adresse mac de l'ip du ping.

[arp_tp2.pcapng](https://github.com/llGrMell/TP2-Ethernet-IP-et-ARP/blob/main/arp_tp2.pcapng)

nslookup youtube.com

Serveur :   dns.google

Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    youtube.com
Addresses:  2a00:1450:4007:80e::200e
216.58.214.174


# III. DHCP you too my brooo

[dhcp_tp2.pcapng](https://github.com/llGrMell/TP2-Ethernet-IP-et-ARP/blob/main/dhcp_tp2.pcapng)

j'ai trouvé 5 types de trames lors de l'échange DHCP :

- DHCP Release
- DHCP Discover
- DHCP Offer
- DHCP Request
- DHCP ACK

## **IV. Avant-goût TCP et UDP**

**A/** Wireshark it
Lorsque j'utilise Wireshark pour lire des vidéos YouTube,
Adresse ```77.136.192.86``` trouvée sur le port ```443``` pour le protocole UDP.
Pour le protocole TCP, recherchez l'adresse ```74.125.4.234``` sur le port ```443```.

 [udp-tcp-tp2.pcapng](https://github.com/llGrMell/TP2-Ethernet-IP-et-ARP/blob/main/udp-tcp-tp2.pcapng)
