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
Dans cette commande, nous définissons la carte souhaitée, puis l'adresse statique, puis le masque de réseau et enfin la passerelle

```
netsh interface ipv4 set address name="Ethernet" static 192.168.0.2 255.255.255.192 192.168.0.1
```

* ***Pour remettre la configuration DHCP automatique:***
```
netsh interface ipv4 set address name="Ethernet" source=dhcp
```

Nous pouvons constater que le ping entre les deux machines via ethernet est possible.

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

* fichier wireshark


## **II. ARP my bro**

**A/** Check the ARP table

On utilise la commande si dessous pour afficher le table ARP 


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
on suprime la table arp: 
```
PS C:\Users\baptb> arp -d
```

on regare ce qu'il y a dedans:
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
# III. DHCP you too my brooo

## **IV. Avant-goût TCP et UDP**

**A/** Wireshark it
> []
nslookup youtube.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    youtube.com
Addresses:  2a00:1450:4007:80e::200e
          216.58.214.174
