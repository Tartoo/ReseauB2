#    **TP 1 - Mise en jambes**

##    I. Exploration locale en solo

##   1. Affichage d'informations sur la pile TCP/IP locale
###    **Informations des cartes réseaux :** 

###    Wifi : 
####    commande :```ipconfig /all```
####    résultat :    
```
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . : auvence.co
   Description. . . . . . . . . . . . . . : Killer(R) Wi-Fi 6 AX1650x 160MHz Wireless Network Adapter (200NGW)
   Adresse physique . . . . . . . . . . . : 50-EB-71-D6-4E-8F
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.2.81(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : lundi 13 septembre 2021 08:52:35
   Bail expirant. . . . . . . . . . . . . : lundi 13 septembre 2021 12:42:41
   Passerelle par défaut. . . . . . . . . : 10.33.3.253
   Serveur DHCP . . . . . . . . . . . . . : 10.33.3.254
   Serveurs DNS. . .  . . . . . . . . . . : 10.33.10.2
                                       10.33.10.148
                                       10.33.10.155
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```
    
**Nom :** *Carte réseau sans fil Wi-Fi*
**MAC :** *50-EB-71-D6-4E-8F*
**IP :** *10.33.2.81*

###    Ethernet : 
####    commande :```ipconfig /all```
####    résultat :    
```
Carte Ethernet Ethernet :

   Statut du média. . . . . . . . . . . . : Média déconnecté
   Suffixe DNS propre à la connexion. . . : lan
   Description. . . . . . . . . . . . . . : Killer E2600 Gigabit Ethernet Controller
   Adresse physique . . . . . . . . . . . : B4-2E-99-F6-A6-43
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
```
**Nom :** *Carte Ethernet Ethernet*
**MAC :** *B4-2E-99-F6-A6-43*
**IP :** *non connnecté*

***

###    **Affichage du gateway :**
####    commande :```ipconfig /all```
####    résultat :    
```
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . : auvence.co
   Description. . . . . . . . . . . . . . : Killer(R) Wi-Fi 6 AX1650x 160MHz Wireless Network Adapter (200NGW)
   Adresse physique . . . . . . . . . . . : 50-EB-71-D6-4E-8F
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.2.81(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : lundi 13 septembre 2021 08:52:35
   Bail expirant. . . . . . . . . . . . . : lundi 13 septembre 2021 12:42:41
   Passerelle par défaut. . . . . . . . . : 10.33.3.253
   Serveur DHCP . . . . . . . . . . . . . : 10.33.3.254
   Serveurs DNS. . .  . . . . . . . . . . : 10.33.10.2
                                       10.33.10.148
                                       10.33.10.155
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```
**Gateway de la carte wifi :** *10.33.3.253*

***

###    **afficher les informations sur une carte IP (GUI):**
```
Windows :
Parametres->Réseau et Internet->Etat->Afficher les proprités du matériel et de la connexion
```
![](https://i.imgur.com/GXh8Zdq.png)
**IP :** *10.33.2.81/22*
**MAC :** *50:eb:71:d6:4e:8f*
**Gateway :** *10.33.3.253*

***

###    **Question :  à quoi sert la gateway dans le réseau d'YNOV ?**
La Gateway dans le réseau d'Ynov permet de nous relier le réseau de l'école à des réseaux extérieurs.

***

##   2. Modifications des informations:
###    **A. Modification d'adresse IP (part 1)**:
####    **Changer d'adresse IP (GUI):**
```
Windows :
Parametres->Réseau et Internet->Wifi->WiFi@YNOV->ParemètreIP->Modifier->Manuel
```
![](https://i.imgur.com/XenKnw6.png)
***
![](https://i.imgur.com/9dbUe38.png)
***
![](https://i.imgur.com/fm6RdDj.png)

***
###    **Question : Il est possible que vous perdiez l'accès internet. Que ce soit le cas ou non, expliquez pourquoi c'est possible de perdre son accès internet en faisant cette opération.**
Le serveur nous associe une adresse IP et MAC, lorsque l'on change l'adresse IP, celui-ci verifie si l'adresse correspond a celle donnée en même temps que l'adresse MAC. Le serveur étant en DHCP, il ne nous reconnait pas et donc ne nous accepte pas.

***

###    **B. Table ARP:**
####    **Exploration de la table ARP:**
####    commande :```arp -a```####    résultat :
####    résultat : 
```
Interface : 10.33.2.81 --- 0x5
  Adresse Internet      Adresse physique      Type
  10.33.1.90            d0-ab-d5-18-55-f6     dynamique
  10.33.3.33            c8-58-c0-63-5a-92     dynamique
  10.33.3.81            3c-58-c2-9d-98-38     dynamique
  10.33.3.253           00-12-00-40-4c-bf     dynamique
  10.33.3.255           ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.56.1 --- 0x6
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.120.1 --- 0x9
  Adresse Internet      Adresse physique      Type
  192.168.120.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.10.1 --- 0xe
  Adresse Internet      Adresse physique      Type
  192.168.10.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 169.254.123.85 --- 0x1a
  Adresse Internet      Adresse physique      Type
  169.254.255.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.25.1 --- 0x1b
  Adresse Internet      Adresse physique      Type
  192.168.25.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```
L'adresse MAC de la passerelle par défaut est: ```00-12-00-40-4c-bf```.
J'ai pu la repéré en comparant les adresse Internet et celle de la passerelle par défaut jusqu'a que j'en trouve une identique.

***

####    **Remplissage de la table ARP:**
* envoyez des ping vers des IP du même réseau : 
    
    #### commande : `ping 10.33.2.8, ping 10.33.0.119, ping 10.33.3.10`
    ####    résultat : 
    
        Envoi d’une requête 'Ping'  10.33.*.* avec 32 octets de données :
        Réponse de 10.33.0.119 : octets=32 temps=8 ms TTL=128
        Réponse de 10.33.0.119 : octets=32 temps=4 ms TTL=128
        Réponse de 10.33.0.119 : octets=32 temps=4 ms TTL=128
        Réponse de 10.33.0.119 : octets=32 temps=3 ms TTL=128

* affichez votre table ARP et listez  les adresses MAC associées aux adresses IP que vous avez ping : 
    #### commande : `arp -a`
    ####    résultat : 
        Interface : 10.33.2.81 --- 0x5
      Adresse Internet      Adresse physique      Type
      10.33.0.119           18-56-80-70-9c-48     dynamique
      10.33.2.8             a4-5e-60-ed-0b-27     dynamique
      10.33.3.10            a4-83-e7-59-ae-c2     dynamique
      10.33.3.253           00-12-00-40-4c-bf     dynamique
      10.33.3.254           00-0e-c4-cd-74-f5     dynamique
      10.33.3.255           ff-ff-ff-ff-ff-ff     statique
      224.0.0.22            01-00-5e-00-00-16     statique
      224.0.0.251           01-00-5e-00-00-fb     statique
      224.0.0.252           01-00-5e-00-00-fc     statique
      239.255.255.250       01-00-5e-7f-ff-fa     statique
      255.255.255.255       ff-ff-ff-ff-ff-ff     statique
Les adresses MAC des ip que j'ai ping sont : `18-56-80-70-9c-48`,`a4-5e-60-ed-0b-27` et `a4-83-e7-59-ae-c2`

***

###    **C. Nmap:**
####    **Scanner le réseau de votre carte WiFi et trouver une adresse IP libre:**
#### commande : `nmap -sP 10.33.0.0/22`,`arp -a`
####    résultat (seulement une petite partie):       
```
PS C:\WINDOWS\system32> nmap -sP 10.33.0.0/22
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-16 09:53 Paris, Madrid (heure dÆÚtÚ)
Nmap scan report for 10.33.0.3
Host is up (0.16s latency).
MAC Address: 40:EC:99:8D:22:23 (Intel Corporate)
Nmap scan report for 10.33.0.5
Host is up (0.031s latency).
MAC Address: 1C:BF:C0:17:32:09 (Chongqing Fugui Electronics)
Nmap scan report for 10.33.0.6
Host is up (0.014s latency).
MAC Address: 84:5C:F3:80:32:07 (Intel Corporate)
Nmap scan report for 10.33.2.105
Host is up (0.0070s latency).
MAC Address: EC:2E:98:CA:DA:E9 (AzureWave Technology)
Nmap scan report for 10.33.3.105
Host is up (0.010s latency).
MAC Address: 54:14:F3:B5:AA:36 (Intel Corporate)
Nmap scan report for 10.33.3.206
Host is up (0.11s latency).
MAC Address: 78:31:C1:CF:CB:26 (Apple)
Nmap scan report for 10.33.3.212
Host is up (0.60s latency).
MAC Address: F8:5E:A0:17:91:ED (Intel Corporate)
Nmap scan report for 10.33.3.214
Host is up (0.70s latency).
MAC Address: 6C:40:08:BC:78:92 (Apple)
Nmap scan report for 10.33.3.218
Host is up (0.40s latency).
MAC Address: 8C:85:90:65:6A:52 (Apple)
Nmap scan report for 10.33.3.219
Host is up (0.45s latency).
MAC Address: A0:78:17:B5:63:BB (Apple)
Nmap scan report for 10.33.3.225
Host is up (0.084s latency).
MAC Address: 34:2E:B7:49:5A:CF (Intel Corporate)
Nmap scan report for 10.33.3.253
Host is up (0.014s latency).
MAC Address: 00:12:00:40:4C:BF (Cisco Systems)
Nmap scan report for 10.33.3.254
Host is up (0.0040s latency).
MAC Address: 00:0E:C4:CD:74:F5 (Iskra Transmission d.d.)
Nmap scan report for 10.33.2.81
Host is up.
Nmap done: 1024 IP addresses (142 hosts up) scanned in 95.33 seconds
```
***
```
Interface : 10.33.2.81 --- 0x5
  Adresse Internet      Adresse physique      Type
  10.33.0.7             9c-bc-f0-b6-1b-ed     dynamique
  10.33.0.60            e2-ee-36-a5-0b-8a     dynamique
  10.33.0.111           d2-41-f0-dc-6a-ed     dynamique
  10.33.0.180           26-91-29-98-e2-9d     dynamique
  10.33.2.196           08-71-90-87-b9-8c     dynamique
  10.33.2.216           08-d2-3e-35-00-a2     dynamique
  10.33.3.59            02-47-cd-3d-d4-e9     dynamique
  10.33.3.214           6c-40-08-bc-78-92     dynamique
  10.33.3.253           00-12-00-40-4c-bf     dynamique
  10.33.3.255           ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.10.1 --- 0xc
  Adresse Internet      Adresse physique      Type
  192.168.10.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.1.1.1 --- 0x12
  Adresse Internet      Adresse physique      Type
  10.1.1.255            ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 169.254.123.85 --- 0x19
  Adresse Internet      Adresse physique      Type
  169.254.255.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.25.1 --- 0x1a
  Adresse Internet      Adresse physique      Type
  192.168.25.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```
***
###    **D. Modification d'adresse IP (part 2)**:
*    Modifiez de nouveau votre adresse IP vers une adresse IP que vous savez libre grâce à nmap
        #### commandes: `nmap -sP -v 10.33.0.0/22`
        ####    résultat (seulement une petite partie):
        ![](https://i.imgur.com/L8KNIKq.gif)

```
PS C:\WINDOWS\system32> nmap -sP -v 10.33.0.0/22
Warning: The -sP option is deprecated. Please use -sn
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-16 10:07 Paris, Madrid (heure dÆÚtÚ)
Initiating ARP Ping Scan at 10:07
Scanning 1023 hosts [1 port/host]
Completed ARP Ping Scan at 10:07, 25.05s elapsed (1023 total hosts)
Initiating Parallel DNS resolution of 108 hosts. at 10:07
Completed Parallel DNS resolution of 108 hosts. at 10:07, 11.09s elapsed
Nmap scan report for 10.33.0.0 [host down]
Nmap scan report for 10.33.0.1 [host down]
Nmap scan report for 10.33.0.2 [host down]
Nmap scan report for 10.33.0.3 [host down]
Nmap scan report for 10.33.0.4 [host down]
Nmap scan report for 10.33.0.5 [host down]
Nmap scan report for 10.33.0.6
Host is up (0.10s latency).
MAC Address: 84:5C:F3:80:32:07 (Intel Corporate)
Nmap scan report for 10.33.0.7
Host is up (0.14s latency).
MAC Address: 9C:BC:F0:B6:1B:ED (Xiaomi Communications)
Nmap scan report for 10.33.0.64 [host down]
Nmap scan report for 10.33.0.65 [host down]
Nmap scan report for 10.33.0.66 [host down]
Nmap scan report for 10.33.0.67 [host down]
Nmap scan report for 10.33.0.68 [host down]
Nmap scan report for 10.33.0.69 [host down]
Nmap scan report for 10.33.0.70 [host down]
Nmap scan report for 10.33.0.96
Host is up (0.11s latency).
MAC Address: CA:4F:F4:AF:8F:0C (Unknown)
Nmap scan report for 10.33.0.97 [host down]
Nmap scan report for 10.33.0.98 [host down]
Nmap scan report for 10.33.0.99 [host down]
Nmap scan report for 10.33.0.100
Host is up (0.098s latency).
MAC Address: E8:6F:38:6A:B6:EF (Chongqing Fugui Electronics)
Nmap scan report for 10.33.0.101 [host down]
Nmap scan report for 10.33.0.102 [host down]
Nmap scan report for 10.33.0.103 [host down]
Nmap scan report for 10.33.0.104 [host down]
Nmap scan report for 10.33.0.105 [host down]
Nmap scan report for 10.33.0.106 [host down]
Nmap scan report for 10.33.0.107 [host down]
Nmap scan report for 10.33.0.108 [host down]
Nmap scan report for 10.33.0.109 [host down]
Nmap scan report for 10.33.0.110 [host down]
Nmap scan report for 10.33.0.111
Host is up (1.3s latency).
MAC Address: D2:41:F0:DC:6A:ED (Unknown)
Nmap scan report for 10.33.0.112 [host down]
Nmap scan report for 10.33.0.113 [host down]
Nmap scan report for 10.33.0.114 [host down]
Nmap scan report for 10.33.0.115 [host down]
Nmap scan report for 10.33.0.116 [host down]
Nmap scan report for 10.33.0.135
Host is up (0.14s latency).
Nmap scan report for 10.33.3.249
Host is up (0.13s latency).
MAC Address: 58:96:1D:17:43:F5 (Intel Corporate)
Nmap scan report for 10.33.3.250 [host down]
Nmap scan report for 10.33.3.251 [host down]
Nmap scan report for 10.33.3.252
Host is up (0.044s latency).
MAC Address: 00:1E:4F:F9:BE:14 (Dell)
Nmap scan report for 10.33.3.253
Host is up (0.035s latency).
MAC Address: 00:12:00:40:4C:BF (Cisco Systems)
Nmap scan report for 10.33.3.254
Host is up (0.11s latency).
MAC Address: 00:0E:C4:CD:74:F5 (Iskra Transmission d.d.)
Nmap scan report for 10.33.3.255 [host down]
Initiating Parallel DNS resolution of 1 host. at 10:07
Completed Parallel DNS resolution of 1 host. at 10:08, 11.09s elapsed
Nmap scan report for 10.33.2.81
Host is up.
Read data files from: C:\Program Files (x86)\Nmap
Nmap done: 1024 IP addresses (109 hosts up) scanned in 49.96 seconds
           Raw packets sent: 1965 (55.020KB) | Rcvd: 136 (3.808KB)
```   
*    prouvez en une suite de commande que vous avez une IP choisi manuellement, que votre passerelle est bien définie, et que vous avez un accès internet : 
        #### Commande:`ipconfig ; ping 1.1.1.1`
        #### Résultat: 
```
PS C:\WINDOWS\system32> ipconfig ; ping 1.1.1.1

Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::ad3d:ea5:a6c9:edd3%5
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.0.102
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Passerelle par défaut. . . . . . . . . : 10.33.3.253

Envoi d’une requête 'Ping'  1.1.1.1 avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=21 ms TTL=58
Réponse de 1.1.1.1 : octets=32 temps=31 ms TTL=58
Réponse de 1.1.1.1 : octets=32 temps=48 ms TTL=58
Réponse de 1.1.1.1 : octets=32 temps=45 ms TTL=58

Statistiques Ping pour 1.1.1.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 21ms, Maximum = 48ms, Moyenne = 36ms
```
***
##    II. Exploration locale en duo (avec Leo Sery)
![](https://i.imgur.com/tRewpSK.gif)

##   1. Modification d'adresse IP :
*    vérifiez à l'aide de commandes que vos changements ont pris effet:
        #### Commande:`ipconfig`
        #### Résultat:
```
Carte Ethernet Ethernet :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::2d8d:fd20:d28a:8b21%23
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.1.2
   Masque de sous-réseau. . . . . . . . . : 255.255.255.252
   Passerelle par défaut. . . . . . . . . : 192.168.1.1
```
*    Utilisez ping pour tester la connectivité entre les deux machines:
        #### Commande:`ping 192.168.1.1`
        #### Résultat:
```
PS C:\Users\mattf> ping 192.168.1.1

Envoi d’une requête 'Ping'  192.168.1.1 avec 32 octets de données :
Réponse de 192.168.1.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.1.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.1.1 : octets=32 temps<1ms TTL=128
Réponse de 192.168.1.1 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 192.168.1.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```
*    Affichez et consultez votre table ARP:
        #### Commande:`arp -a`
        #### Résultat:
```
PS C:\Users\mattf> arp -a

Interface : 10.33.0.102 --- 0x5
  Adresse Internet      Adresse physique      Type
  10.33.3.253           00-12-00-40-4c-bf     dynamique
  10.33.3.255           ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.10.1 --- 0xc
  Adresse Internet      Adresse physique      Type
  192.168.10.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.1.1.1 --- 0x12
  Adresse Internet      Adresse physique      Type
  10.1.1.255            ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.1.2 --- 0x17
  Adresse Internet      Adresse physique      Type
  192.168.1.1           08-97-98-a4-1c-77     dynamique
  192.168.1.3           ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 169.254.123.85 --- 0x19
  Adresse Internet      Adresse physique      Type
  169.254.255.255       ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.25.1 --- 0x1a
  Adresse Internet      Adresse physique      Type
  192.168.25.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```
*    test de la connectivité à internet:
        #### Commande:`ping 1.1.1.1`
        #### Résultat(léo):
 
```
ping  1.1.1.1

Pinging 1.1.1.1 with 32 bytes of data:
Request timed out.
Reply from 1.1.1.1: bytes=32 time=17ms TTL=57
Reply from 1.1.1.1: bytes=32 time=16ms TTL=57
Reply from 1.1.1.1: bytes=32 time=17ms TTL=57

Ping statistics for 1.1.1.1:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 16ms, Maximum = 17ms, Average = 16ms
```
*    ping + tracert:
        #### Commande:`tracert 1.1.1.1`
        #### Résultat(léo):
```
tracert 1.1.1.1

Tracing route to one.one.one.one [1.1.1.1]
over a maximum of 30 hops:

  1     1 ms    <1 ms    <1 ms  DESKTOP-DG9JKP6 [192.168.1.2]
  2     *        *        *     Request timed out.
  3     3 ms     3 ms     2 ms  10.33.3.253
  4    35 ms     5 ms     4 ms  10.33.10.254
  5     2 ms     2 ms     2 ms  reverse.completel.net [92.103.174.137]
  6     8 ms     8 ms     8 ms  92.103.120.182
  7    23 ms    22 ms    23 ms  172.19.130.117
  8    30 ms    30 ms    32 ms  46.218.128.74
  9    26 ms    29 ms    18 ms  equinix-paris.cloudflare.com [195.42.144.143]
 10    17 ms    17 ms    17 ms  one.one.one.one [1.1.1.1]

Trace complete.
```
***
##   2. Petit chat privé : 
*    sur le PC client:
        #### Commande:`nc.exe 192.168.1.1 8888`
        #### Résultat:
```
C:\Users\mattf\Desktop>nc.exe 192.168.1.1 8888
zae
leo
allo
...
```
*    pour aller un peu plus loin:
        *    **le serveur peut préciser sur quelle IP écouter, et ne pas répondre sur les autres:**
        #### Commande:`nc.exe -l -p 9999 192.168.1.37`
        #### Résultat:
        ```
        C:\Users\mattf\Desktop>nc.exe -l -p 9999 192.168.1.37
        invalid connection to [192.168.1.2] from (UNKNOWN)[192.168.1.1] 21434
        ```
        *    **accepter uniquement les connexions internes à la machine en écoutant sur 127.0.0.1:**
        #### Commande:`nc.exe -l -p 9999 127.0.0.1`
        #### Résultat:
```
C:\Users\mattf\Desktop>nc.exe -l -p 9999 127.0.0.1
invalid connection to [192.168.1.2] from (UNKNOWN) [192.168.1.1] 24357
```
***
##   3. Firewall : 
*    Autoriser les ping:
        #### Commande:`netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol=icmpv4:8,any dir=in action=allow`
        #### Résultat:
```
PS C:\WINDOWS\system32> netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol=icmpv4:8,any dir=in action=allow
Ok.

PS C:\WINDOWS\system32> ping 192.168.1.2

Envoi d’une requête 'Ping'  192.168.1.2 avec 32 octets de données :
Réponse de 192.168.1.2 : octets=32 temps<1ms TTL=64
Réponse de 192.168.1.2 : octets=32 temps<1ms TTL=64
Réponse de 192.168.1.2 : octets=32 temps<1ms TTL=64
Réponse de 192.168.1.2 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 192.168.1.2:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```
*    Autoriser nc sur un port spécifique:
        *    **Ouverture du port :**
                #### Commande:`netsh advfirewall firewall add rule name= "Open Port 15801" dir=in action=allow protocol=TCP localport=15801`
                #### Résultat: `Ok`
        *    **Test Netcat :**
                #### Commande:`nc.exe -l -p 15801 192.168.1.1`
                #### Résultat:
```
C:\Users\mattf\Desktop>nc.exe -l -p 15801 192.168.1.1
f
fsdf
dsf
sdf
^C
C:\Users\mattf\Desktop>
```

***

##    III. Manipulations d'autres outils/protocoles côté client
##   1. DHCP (sur mon wifi non celui d'Ynov):

###    **Exploration du DHCP, depuis votre PC:**
#### Commande:`ipconfig /all`
#### Résultat
```
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . : numericable.fr
   Description. . . . . . . . . . . . . . : Killer(R) Wi-Fi 6 AX1650x 160MHz Wireless Network Adapter (200NGW)
   Adresse physique . . . . . . . . . . . : 50-EB-71-D6-4E-8F
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.0.18(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Bail obtenu. . . . . . . . . . . . . . : jeudi 16 septembre 2021 14:19:49
   Bail expirant. . . . . . . . . . . . . : vendredi 17 septembre 2021 14:20:55
   Passerelle par défaut. . . . . . . . . : 192.168.0.1
   Serveur DHCP . . . . . . . . . . . . . : 192.168.0.1
   Serveurs DNS. . .  . . . . . . . . . . : 89.2.0.1
                                       89.2.0.2
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```
L'adresse IP du DHCP de mon réseau WiFi est : `192.168.0.1`
La date d'expiration de mon bail DHCP est le vendredi 17 septembre a 14h20 et 55 secondes.

***

##   2. DNS:

*    Adresse IP du serveur DNS:
        #### Commande:`ipconfig /all`
        #### Résultat:
```
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . : numericable.fr
   Description. . . . . . . . . . . . . . : Killer(R) Wi-Fi 6 AX1650x 160MHz Wireless Network Adapter (200NGW)
   Adresse physique . . . . . . . . . . . : 50-EB-71-D6-4E-8F
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.0.18(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Bail obtenu. . . . . . . . . . . . . . : jeudi 16 septembre 2021 14:19:49
   Bail expirant. . . . . . . . . . . . . : vendredi 17 septembre 2021 14:20:55
   Passerelle par défaut. . . . . . . . . : 192.168.0.1
   Serveur DHCP . . . . . . . . . . . . . : 192.168.0.1
   Serveurs DNS. . .  . . . . . . . . . . : 89.2.0.1
                                       89.2.0.2
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```
L'adresse IP du serveur DNS est 89.2.0.1

*    Utilisation de nslookup:
        *    **Trouver des IP à partir d'un nom de domaine:**
                #### Commande:`nslookup google.com;nslookup ynov.com`
                #### Résultat:
                    PS C:\WINDOWS\system32> nslookup google.com;nslookup ynov.com
                    Serveur :   ns1.numericable.net
                    Address:  89.2.0.1

                    Réponse ne faisant pas autorité :
                    Nom :    google.com
                    Addresses:  2a00:1450:4007:81a::200e
                              142.250.201.174

                    Serveur :   ns1.numericable.net
                    Address:  89.2.0.1

                    Réponse ne faisant pas autorité :
                    Nom :    ynov.com
                    Address:  92.243.16.143
                On sais maintenant que l'adresse IPv4 de google est 142.250.201.174 et celle de Ynov 92.243.16.143 .Google à aussi une IPv6 qui est 2a00:1450:4007:81a::200e
        *    **déterminer l'adresse IP du serveur à qui vous venez d'effectuer ces requêtes:**
                L'adresse IP du serveur a qui j'ai fais la requête est 89.2.0.1
        *   **Trouver des noms de domaines à partir d'une IP:**
            #### Commande:`nslookup 78.74.21.21;nslookup 92.146.54.88`
            #### Résultat:
                    PS C:\WINDOWS\system32> nslookup 78.74.21.21;nslookup 92.146.54.88
                    Serveur :   ns1.numericable.net
                    Address:  89.2.0.1

                    Nom :    host-78-74-21-21.homerun.telia.com
                    Address:  78.74.21.21

                    Serveur :   ns1.numericable.net
                    Address:  89.2.0.1

                    Nom :    apoitiers-654-1-167-88.w92-146.abo.wanadoo.fr
                    Address:  92.146.54.88

        *    On sais maintenant que le nom de domaine de l'adresse 78.74.21.21 est host-78-74-21-21.homerun.telia.com et celui de 92.146.54.88 est apoitiers-654-1-167-88.w92-146.abo.wanadoo.fr.
***

##    IV. Wireshark
##    Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence:

*    Un ping entre vous et la passerelle:
    ![](https://i.imgur.com/nYKpUuN.png)
*    un netcat entre vous et votre mate, branché en RJ45
    ![](https://i.imgur.com/PGLixi2.png)
*   une requête DNS. Identifiez dans la capture le serveur DNS à qui vous posez la question
    ![](https://i.imgur.com/6YKBmeZ.png)
