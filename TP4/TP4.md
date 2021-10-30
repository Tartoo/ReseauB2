# TP4 : Vers un réseau d'entreprise

# Sommaire

- [TP4 : Vers un réseau d'entreprise](#tp4--vers-un-réseau-dentreprise)
- [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
  - [Checklist VM Linux](#checklist-vm-linux)
- [I. Dumb switch](#i-dumb-switch)
  - [1. Topologie 1](#1-topologie-1)
  - [2. Adressage topologie 1](#2-adressage-topologie-1)
  - [3. Setup topologie 1](#3-setup-topologie-1)
- [II. VLAN](#ii-vlan)
  - [1. Topologie 2](#1-topologie-2)
  - [2. Adressage topologie 2](#2-adressage-topologie-2)
    - [3. Setup topologie 2](#3-setup-topologie-2)
- [III. Routing](#iii-routing)
  - [1. Topologie 3](#1-topologie-3)
  - [2. Adressage topologie 3](#2-adressage-topologie-3)
  - [3. Setup topologie 3](#3-setup-topologie-3)
- [IV. NAT](#iv-nat)
  - [1. Topologie 4](#1-topologie-4)
  - [2. Adressage topologie 4](#2-adressage-topologie-4)
  - [3. Setup topologie 4](#3-setup-topologie-4)

# I. Dumb switch

## 1. Topologie 1

![](https://i.imgur.com/Lflm9B8.png)

## 2. Adressage topologie 1

| Node  | IP            |
|-------|---------------|
| `pc1` | `10.1.1.1/24` |
| `pc2` | `10.1.1.2/24` |

## 3. Setup topologie 1

🌞 **Commençons simple**

- définissez les IPs statiques sur les deux VPCS

```
PC1> ip 10.1.1.1 255.255.255.0
Checking for duplicate address...
PC1 : 10.1.1.1 255.255.255.0

PC1>
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20006
RHOST:PORT  : 127.0.0.1:20007
MTU         : 1500

PC2> ip 10.1.1.2/24
Checking for duplicate address...
PC2 : 10.1.1.2 255.255.255.0

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```
- `ping` un VPCS depuis l'autre

```
PC1> ping 10.1.1.2 -c 2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=15.917 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=4.123 ms
```

> Jusque là, ça devrait aller. Noter qu'on a fait aucune conf sur le switch. Tant qu'on ne fait rien, c'est une bête multiprise.

# II. VLAN

**Le but dans cette partie va être de tester un peu les *VLANs*.**

On va rajouter **un troisième client** qui, bien que dans le même réseau, sera **isolé des autres grâce aux *VLANs***.

**Les *VLANs* sont une configuration à effectuer sur les *switches*.** C'est les *switches* qui effectuent le blocage.

Le principe est simple :

- déclaration du VLAN sur tous les switches
  - un VLAN a forcément un ID (un entier)
  - bonne pratique, on lui met un nom
- sur chaque switch, on définit le VLAN associé à chaque port
  - genre "sur le port 35, c'est un client du VLAN 20 qui est branché"


## 1. Topologie 2

![](https://i.imgur.com/Tp1uQTV.png)

## 2. Adressage topologie 2

| Node  | IP            | VLAN |
|-------|---------------|------|
| `pc1` | `10.1.1.1/24` | 10   |
| `pc2` | `10.1.1.2/24` | 10   |
| `pc3` | `10.1.1.3/24` | 20   |

### 3. Setup topologie 2

🌞 **Adressage**

- définissez les IPs statiques sur tous les VPCS

```
PC3> ip 10.1.1.3/24
Checking for duplicate address...
PC3 : 10.1.1.3 255.255.255.0

PC3> sh ip

NAME        : PC3[1]
IP/MASK     : 10.1.1.3/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20042
RHOST:PORT  : 127.0.0.1:20043
MTU         : 1500
```
- vérifiez avec des `ping` que tout le monde se ping

```
PC1> ping 10.1.1.2 -c 2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=14.270 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=4.814 ms

PC2> ping 10.1.1.3 -c 2

84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=8.361 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=5.299 ms

PC3> ping 10.1.1.1 -c 2

84 bytes from 10.1.1.1 icmp_seq=1 ttl=64 time=7.158 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=64 time=12.500 ms
```

🌞 **Configuration des VLANs**

- référez-vous [à la section VLAN du mémo Cisco](../../cours/memo/memo_cisco.md#8-vlan)
- déclaration des VLANs sur le switch `sw1`

```
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#
Switch(config)#vlan 10
Switch(config-vlan)#name admins
Switch(config-vlan)#exit
Switch(config)#vlan 20
Switch(config-vlan)#name guest
Switch(config-vlan)#exit
Switch(config)#exit
Switch#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/0, Gi0/1, Gi0/2, Gi0/3
                                                Gi1/0, Gi1/1, Gi1/2, Gi1/3
                                                Gi2/0, Gi2/1, Gi2/2, Gi2/3
                                                Gi3/0, Gi3/1, Gi3/2, Gi3/3
10   admins                           active
20   guest                            active
[...]
```
- ajout des ports du switches dans le bon VLAN (voir [le tableau d'adressage de la topo 2 juste au dessus](#2-adressage-topologie-2))
  - ici, tous les ports sont en mode *access* : ils pointent vers des clients du réseau

```
Switch#conf t
Switch(config)#interface Gi0/0
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit

Switch(config)#interface Gi0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit

Switch(config)#interface Gi0/2
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-if)#exit
```

🌞 **Vérif**

- `pc1` et `pc2` doivent toujours pouvoir se ping

```
PC1> ping 10.1.1.2 -c 2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=2.603 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=3.002 ms

PC2> ping 10.1.1.1 -c 2

84 bytes from 10.1.1.1 icmp_seq=1 ttl=64 time=5.011 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=64 time=5.015 ms
```
- `pc3` ne ping plus personne

```
PC3> ping 10.1.1.1

host (10.1.1.1) not reachable

PC3> ping 10.1.1.2

host (10.1.1.2) not reachable
```

# III. Routing

Dans cette partie, on va donner un peu de sens aux VLANs :

- un pour les serveurs du réseau
  - on simulera ça avec un p'tit serveur web
- un pour les admins du réseau
- un pour les autres random clients du réseau

Cela dit, il faut que tout ce beau monde puisse se ping, au moins joindre le réseau des serveurs, pour accéder au super site-web.

**Bien que bloqué au niveau du switch à cause des VLANs, le trafic pourra passer d'un VLAN à l'autre grâce à un routeur.**

Il assurera son job de routeur traditionnel : router entre deux réseaux. Sauf qu'en plus, il gérera le changement de VLAN à la volée.

## 1. Topologie 3



## 2. Adressage topologie 3

Les réseaux et leurs VLANs associés :

| Réseau    | Adresse       | VLAN associé |
|-----------|---------------|--------------|
| `clients` | `10.1.1.0/24` | 11           |
| `servers` | `10.2.2.0/24` | 12           |
| `routers` | `10.3.3.0/24` | 13           |

L'adresse des machines au sein de ces réseaux :

| Node               | `clients`       | `admins`        | `servers`       |
|--------------------|-----------------|-----------------|-----------------|
| `pc1.clients.tp4`  | `10.1.1.1/24`   | x               | x               |
| `pc2.clients.tp4`  | `10.1.1.2/24`   | x               | x               |
| `adm1.admins.tp4`  | x               | `10.2.2.1/24`   | x               |
| `web1.servers.tp4` | x               | x               | `10.3.3.1/24`   |
| `r1`               | `10.1.1.254/24` | `10.2.2.254/24` | `10.3.3.254/24` |

## 3. Setup topologie 3

🖥️ VM `web1.servers.tp4`, déroulez la [Checklist VM Linux](#checklist-vm-linux) dessus

🌞 **Adressage**

- définissez les IPs statiques sur toutes les machines **sauf le *routeur***

🌞 **Configuration des VLANs**

- référez-vous au [mémo Cisco](../../cours/memo/memo_cisco.md#8-vlan)
- déclaration des VLANs sur le switch `sw1`
- ajout des ports du switches dans le bon VLAN (voir [le tableau d'adressage de la topo 2 juste au dessus](#2-adressage-topologie-2))

```
Switch#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi1/0, Gi1/1, Gi1/2, Gi1/3
                                                Gi2/0, Gi2/1, Gi2/2, Gi2/3
                                                Gi3/0, Gi3/1, Gi3/2, Gi3/3
11   clients                          active    Gi0/0, Gi0/1
12   admins                           active    Gi0/2
13   servers                          active    Gi0/3
```
- il faudra ajouter le port qui pointe vers le *routeur* comme un *trunk* : c'est un port entre deux équipements réseau (un *switch* et un *routeur*)

```
Switch#conf t
Switch(config)#interface Gi1/0
Switch(config-if)#switchport trunk encapsulation dot1q
Switch(config-if)#switchport trunk allowed vlan add 11,12,13
Switch(config-if)#switchport mode trunk
Switch(config-if)#exit
Switch(config)#exit
Switch#show interface trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi1/0       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Gi1/0       1-4094

Port        Vlans allowed and active in management domain
Gi1/0       1,11-13

Port        Vlans in spanning tree forwarding state and not pruned
Gi1/0       1,11-13
```

---

🌞 **Config du *routeur***

- attribuez ses IPs au *routeur*
  - 3 sous-interfaces, chacune avec son IP et un VLAN associé

```
R1#conf t
R1(config)#interface f0/0.10
R1(config-subif)#encapsulation dot1Q 11
R1(config-subif)#ip addr 10.1.1.254 255.255.255.0
R1(config-subif)#exit

R1(config)#interface f0/0.20
R1(config-subif)#encapsulation dot1Q 12
R1(config-subif)#ip addr 10.2.2.254 255.255.255.0
R1(config-subif)#exit

R1(config)#interface f0/0.30
R1(config-subif)#encapsulation dot1Q 13
R1(config-subif)#ip addr 10.3.3.254 255.255.255.0
R1(config-subif)#exit
```

🌞 **Vérif**

- tout le monde doit pouvoir ping le routeur sur l'IP qui est dans son réseau

```
PC1> ping 10.1.1.254 -c 2

84 bytes from 10.1.1.254 icmp_seq=1 ttl=255 time=8.249 ms
84 bytes from 10.1.1.254 icmp_seq=2 ttl=255 time=14.370 ms

PC2>  ping 10.1.1.254 -c 2

84 bytes from 10.1.1.254 icmp_seq=1 ttl=255 time=8.991 ms
84 bytes from 10.1.1.254 icmp_seq=2 ttl=255 time=7.864 ms

adm1> ping 10.2.2.254 -c 2

84 bytes from 10.2.2.254 icmp_seq=1 ttl=255 time=12.582 ms
84 bytes from 10.2.2.254 icmp_seq=2 ttl=255 time=11.004 ms

[tarto@web ~]$ ping 10.3.3.254 -c 2
PING 10.3.3.254 (10.3.3.254) 56(84) bytes of data.
64 bytes from 10.3.3.254: icmp_seq=1 ttl=255 time=13.8 ms
64 bytes from 10.3.3.254: icmp_seq=2 ttl=255 time=10.7 ms

--- 10.3.3.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 10.705/12.233/13.762/1.532 ms
```
- en ajoutant une route vers les réseaux, ils peuvent se ping entre eux
  - ajoutez une route par défaut sur les VPCS

```
PC1> ip 10.1.1.1/24 10.1.1.254
Checking for duplicate address...
PC1 : 10.1.1.1 255.255.255.0 gateway 10.1.1.254

PC2> ip 10.1.1.2/24 10.1.1.254
Checking for duplicate address...
PC2 : 10.1.1.2 255.255.255.0 gateway 10.1.1.254

adm1> ip 10.2.2.1/24 10.2.2.254
Checking for duplicate address...
adm1 : 10.2.2.1 255.255.255.0 gateway 10.2.2.254
```
  - ajoutez une route par défaut sur la machine virtuelle

```
[tarto@web ~]$ sudo ip route add default via 10.3.3.254
[tarto@web ~]$ cat /etc/sysconfig/network
GATEWAY=10.3.3.254
```
  - testez des `ping` entre les réseaux

```
PC1> ping 10.2.2.1 -c 2

84 bytes from 10.2.2.1 icmp_seq=1 ttl=63 time=31.912 ms
84 bytes from 10.2.2.1 icmp_seq=2 ttl=63 time=20.019 ms

PC2> ping 10.3.3.1 -c 2

84 bytes from 10.3.3.1 icmp_seq=1 ttl=63 time=21.858 ms
84 bytes from 10.3.3.1 icmp_seq=2 ttl=63 time=22.392 ms
```

# IV. NAT

On va ajouter une fonctionnalité au routeur : le NAT.

On va le connecter à internet (simulation du fait d'avoir une IP publique) et il va faire du NAT pour permettre à toutes les machines du réseau d'avoir un accès internet.

![](https://i.imgur.com/LT9bcY4.png)


## 1. Topologie 4

![](https://i.imgur.com/IDHyC9s.png)

## 2. Adressage topologie 4

Les réseaux et leurs VLANs associés :

| Réseau    | Adresse       | VLAN associé |
|-----------|---------------|--------------|
| `clients` | `10.1.1.0/24` | 11           |
| `servers` | `10.2.2.0/24` | 12           |
| `routers` | `10.3.3.0/24` | 13           |

L'adresse des machines au sein de ces réseaux :

| Node               | `clients`       | `admins`        | `servers`       |
|--------------------|-----------------|-----------------|-----------------|
| `pc1.clients.tp4`  | `10.1.1.1/24`   | x               | x               |
| `pc2.clients.tp4`  | `10.1.1.2/24`   | x               | x               |
| `adm1.admins.tp4`  | x               | `10.2.2.1/24`   | x               |
| `web1.servers.tp4` | x               | x               | `10.3.3.1/24`   |
| `r1`               | `10.1.1.254/24` | `10.2.2.254/24` | `10.3.3.254/24` |

## 3. Setup topologie 4

🌞 **Ajoutez le noeud Cloud à la topo**

- branchez à `eth1` côté Cloud
- côté routeur, il faudra récupérer un IP en DHCP (voir [le mémo Cisco](../../cours/memo/memo_cisco.md))

```
R1#conf t
R1(config)#interface f1/0
R1(config-if)#ip address dhcp
R1(config-if)#no shut
R1(config-if)#exit
R1(config)#exit
R1#show ip int br
[...]
FastEthernet1/0            10.0.3.16       YES DHCP   up                    up
```
- vous devriez pouvoir `ping 1.1.1.1`

```
R1#ping 1.1.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 12/34/56 ms
```

🌞 **Configurez le NAT**

- référez-vous [à la section NAT du mémo Cisco](../../cours/memo/memo_cisco.md#7-configuration-dun-nat-simple)

```
R1#config t
R1(config)#int f0/0
R1(config-if)#ip nat inside
R1(config-if)#exit

R1(config)#int f1/0
R1(config-if)#ip nat outside
R1(config-if)#exit

R1(config)#access-list 1 permit any
R1(config)#ip nat inside source list 1 interface f1/0 overload
R1(config)#exit
```

🌞 **Test**

  - sur les VPCS
```
PC1> ip dns 1.1.1.1
PC2> ip dns 1.1.1.1
adm1> ip dns 1.1.1.1

PC1> ping google.com -c 2
google.com resolved to 216.58.206.238

84 bytes from 216.58.206.238 icmp_seq=1 ttl=112 time=30.542 ms
84 bytes from 216.58.206.238 icmp_seq=2 ttl=112 time=55.283 ms

PC2> ping google.com -c 2
google.com resolved to 142.250.179.110

84 bytes from 142.250.179.110 icmp_seq=1 ttl=114 time=41.848 ms
84 bytes from 142.250.179.110 icmp_seq=2 ttl=114 time=38.271 ms

adm1> ping google.com -c 2
google.com resolved to 216.58.204.110

84 bytes from 216.58.204.110 icmp_seq=1 ttl=113 time=41.963 ms
84 bytes from 216.58.204.110 icmp_seq=2 ttl=113 time=56.426 ms
```
  - sur la machine Linux
```
[tarto@web ~]$ cat /etc/resolv.conf
nameserver 1.1.1.1

[tarto@web ~]$ ping -c 2 google.com
PING google.com (142.250.201.174) 56(84) bytes of data.
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=1 ttl=112 time=42.2 ms
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=2 ttl=112 time=39.3 ms

--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 39.269/40.715/42.162/1.460 ms
```

# V. Add a building

On a acheté un nouveau bâtiment, faut tirer et configurer un nouveau switch jusque là-bas.

On va en profiter pour setup un serveur DHCP pour les clients qui s'y trouvent.

## 1. Topologie 5

![Topo 5](./pics/topo5.png)

## 2. Adressage topologie 5

Les réseaux et leurs VLANs associés :

| Réseau    | Adresse       | VLAN associé |
|-----------|---------------|--------------|
| `clients` | `10.1.1.0/24` | 11           |
| `servers` | `10.2.2.0/24` | 12           |
| `routers` | `10.3.3.0/24` | 13           |

L'adresse des machines au sein de ces réseaux :

| Node                | `clients`       | `admins`        | `servers`       |
|---------------------|-----------------|-----------------|-----------------|
| `pc1.clients.tp4`   | `10.1.1.1/24`   | x               | x               |
| `pc2.clients.tp4`   | `10.1.1.2/24`   | x               | x               |
| `pc3.clients.tp4`   | DHCP            | x               | x               |
| `pc4.clients.tp4`   | DHCP            | x               | x               |
| `pc5.clients.tp4`   | DHCP            | x               | x               |
| `dhcp1.clients.tp4` | `10.1.1.253/24` | x               | x               |
| `adm1.admins.tp4`   | x               | `10.2.2.1/24`   | x               |
| `web1.servers.tp4`  | x               | x               | `10.3.3.1/24`   |
| `r1`                | `10.1.1.254/24` | `10.2.2.254/24` | `10.3.3.254/24` |

## 3. Setup topologie 5

Vous pouvez partir de la topologie 4. 

🌞  **Vous devez me rendre le `show running-config` de tous les équipements**

- de tous les équipements réseau
  - le routeur
  - les 3 switches

> N'oubliez pas les VLANs sur tous les switches.

🖥️ **VM `dhcp1.client1.tp4`**, déroulez la [Checklist VM Linux](#checklist-vm-linux) dessus

🌞  **Mettre en place un serveur DHCP dans le nouveau bâtiment**

- il doit distribuer des IPs aux clients dans le réseau `clients` qui sont branchés au même switch que lui
- sans aucune action manuelle, les clients doivent...
  - avoir une IP dans le réseau `clients`
  - avoir un accès au réseau `servers`
  - avoir un accès WAN
  - avoir de la résolution DNS

> Réutiliser les serveurs DHCP qu'on a monté dans les autres TPs.

🌞  **Vérification**

- un client récupère une IP en DHCP
- il peut ping le serveur Web
- il peut ping `8.8.8.8`
- il peut ping `google.com`

> Faites ça sur n'importe quel VPCS que vous venez d'ajouter : `pc3` ou `pc4` ou `pc5`.
