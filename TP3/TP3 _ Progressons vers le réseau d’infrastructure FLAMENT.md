# TP3 : Progressons vers le réseau d'infrastructure FLAMENT

# I. (mini)Architecture réseau

## 1. Adressage

## 2. Routeur

**Vous pouvez d'ores-et-déjà créer le routeur. Pour celui-ci, vous me prouverez que :**

- il a bien une IP dans les 3 réseaux, l'IP que vous avez choisie comme IP de passerelle

        [tarto@router ~]$ ip a
        1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
            link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
            inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
            inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
        2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
            link/ether 08:00:27:d3:1f:35 brd ff:ff:ff:ff:ff:ff
            inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 82629sec preferred_lft 82629sec
            inet6 fe80::a00:27ff:fed3:1f35/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
        3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
            link/ether 08:00:27:52:f6:ed brd ff:ff:ff:ff:ff:ff
            inet 10.3.0.190/26 brd 10.3.0.191 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
            inet6 fe80::a00:27ff:fe52:f6ed/64 scope link
       valid_lft forever preferred_lft forever
        4: enp0s9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
            link/ether 08:00:27:d5:7b:c5 brd ff:ff:ff:ff:ff:ff
            inet 10.3.0.126/25 brd 10.3.0.127 scope global noprefixroute enp0s9
       valid_lft forever preferred_lft forever
            inet6 fe80::a00:27ff:fed5:7bc5/64 scope link
       valid_lft forever preferred_lft forever
        5: enp0s10: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
            link/ether 08:00:27:20:b8:51 brd ff:ff:ff:ff:ff:ff
            inet 10.3.0.206/28 brd 10.3.0.207 scope global noprefixroute enp0s10
       valid_lft forever preferred_lft forever
            inet6 fe80::a00:27ff:fe20:b851/64 scope link
       valid_lft forever preferred_lft forever
       -----------------------------------------------------------------------------------
       [tarto@router ~]$ ip r s
        default via 10.0.2.2 dev enp0s3 proto dhcp metric 101
        10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 101
        10.3.0.0/25 dev enp0s9 proto kernel scope link src 10.3.0.126 metric 105
        10.3.0.128/26 dev enp0s8 proto kernel scope link src 10.3.0.190 metric 104
        10.3.0.192/28 dev enp0s10 proto kernel scope link src 10.3.0.206 metric 106

- il a un accès internet
    
        [tarto@router ~]$ ping 8.8.8.8 -c 2
        PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
        64 bytes from 8.8.8.8: icmp_seq=1 ttl=114 time=18.6 ms
        64 bytes from 8.8.8.8: icmp_seq=2 ttl=114 time=18.3 ms

        --- 8.8.8.8 ping statistics ---
        2 packets transmitted, 2 received, 0% packet loss, time 1002ms
        rtt min/avg/max/mdev = 18.309/18.461/18.614/0.204 ms

- il a de la résolution de noms

        [tarto@router ~]$ ping google.com -c 2
        PING google.com (142.250.179.110) 56(84) bytes of data.
        64 bytes from par21s20-in-f14.1e100.net (142.250.179.110): icmp_seq=1 ttl=114 time=18.2 ms
        64 bytes from par21s20-in-f14.1e100.net (142.250.179.110): icmp_seq=2 ttl=114 time=18.9 ms

        --- google.com ping statistics ---
        2 packets transmitted, 2 received, 0% packet loss, time 1002ms
        rtt min/avg/max/mdev = 18.248/18.576/18.905/0.355 ms
        
- il porte le nom `router.tp3`

        [tarto@router ~]$ cat /etc/hostname
        router.tp3
        
- n'oubliez pas d'activer le routage sur la machine

        [tarto@router ~]$ sudo firewall-cmd --list-all
        public (active)
          target: default
          icmp-block-inversion: no
          interfaces: enp0s10 enp0s3 enp0s8 enp0s9
          sources:
          services: cockpit dhcpv6-client ssh
          ports:
          protocols:
          masquerade: no
          forward-ports:
          source-ports:
          icmp-blocks:
          rich rules:
        ----------------------------------------------------------------------------------
        [tarto@router ~]$ sudo firewall-cmd --get-active-zone
        public
          interfaces: enp0s10 enp0s3 enp0s8 enp0s9
        ----------------------------------------------------------------------------------
        [tarto@router ~]$ sudo firewall-cmd --add-masquerade --zone=public --permanent
        success
# II. Services d'infra

## 1. Serveur DHCP

**Mettre en place une machine qui fera office de serveur DHCP** dans le réseau `client1`. Elle devra :

- porter le nom `dhcp.client1.tp3`

        [tarto@client1 ~]$ cat /etc/hostname
        dhcp.client1.tp3


📁 **Fichier `dhcpd.conf`**

---

🖥️ **VM marcel.client1.tp3**

🌞 **Mettre en place un client dans le réseau `client1`**

- de son p'tit nom `marcel.client1.tp3`
- la machine récupérera une IP dynamiquement grâce au serveur DHCP
- ainsi que sa passerelle et une adresse d'un DNS utilisable

🌞 **Depuis `marcel.client1.tp3`**

    [tarto@marcel ~]$ cat /etc/sysconfig/network-scripts/ifcfg-enp0s8
    BOOTPROTO=dhcp
    NAME=enp0s8
    DEVICE=enp0s8
    ONBOOT=yes
    -------------------------------------------------------------------------------------------
    [tarto@marcel ~]$ ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
    2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:a2:8b:cd brd ff:ff:ff:ff:ff:ff
    inet 10.3.0.131/26 brd 10.3.0.191 scope global dynamic noprefixroute enp0s8
       valid_lft 593sec preferred_lft 593sec
    inet6 fe80::a00:27ff:fea2:8bcd/64 scope link
       valid_lft forever preferred_lft forever
    -------------------------------------------------------------------------------------------
    [tarto@marcel ~]$ ip r s
    default via 10.3.0.190 dev enp0s8 proto dhcp metric 100
    default via 10.3.0.130 dev enp0s8 proto static metric 100
    10.3.0.128/26 dev enp0s8 proto kernel scope link src 10.3.0.131 metric 100
    
- prouver qu'il a un accès internet + résolution de noms, avec des infos récupérées par votre DHCP

        [tarto@marcel ~]$ ping 8.8.8.8 -c 2
        PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
        64 bytes from 8.8.8.8: icmp_seq=1 ttl=113 time=342 ms
        64 bytes from 8.8.8.8: icmp_seq=2 ttl=113 time=211 ms

        --- 8.8.8.8 ping statistics ---
        2 packets transmitted, 2 received, 0% packet loss, time 1001ms
        rtt min/avg/max/mdev = 211.351/276.481/341.611/65.130 ms
        ----------------------------------------------------------------------------------
        [tarto@marcel ~]$ ping ynov.com -c 2
        PING ynov.com (92.243.16.143) 56(84) bytes of data.
        64 bytes from xvm-16-143.dc0.ghst.net (92.243.16.143): icmp_seq=2 ttl=51 time=29.4 ms

        --- ynov.com ping statistics ---
        2 packets transmitted, 1 received, 50% packet loss, time 1051ms
        rtt min/avg/max/mdev = 29.371/29.371/29.371/0.000 ms
        
- à l'aide de la commande `traceroute`, prouver que `marcel.client1.tp3` passe par `router.tp3` pour sortir de son réseau

        [tarto@marcel ~]$ traceroute 8.8.8.8
        traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
         1  _gateway (10.3.0.190)  1.178 ms  1.133 ms  1.090 ms


## 2. Serveur DNS

### A. Our own DNS server

[...]

### B. SETUP copain

🖥️ **VM dns1.server1.tp3**

> Nous avons dans ce TP 2 zones à gérer : `server1.tp3`, `server2.tp3`. J'avoue j'suis pas gentil, ça fait beaucoup de fichiers direct. Ca fait manipuler :D

🌞 **Mettre en place une machine qui fera office de serveur DNS**

- dans le réseau `server1`
- de son p'tit nom `dns1.server1.tp3`
- il faudra lui ajouter un serveur DNS public connu, afin qu'il soit capable de résoudre des noms publics comme `google.com`
  - conf classique avec le fichier `/etc/resolv.conf` ou les fichiers de conf d'interface
- comme pour le DHCP, on part sur "rocky linux dns server" on Google pour les détails de l'install du serveur DNS
  - le paquet que vous allez installer devrait s'appeler **`bind` : c'est le nom du serveur DNS le plus utilisé au monde**
- il y aura plusieurs fichiers de conf :
  - un fichier de conf principal `named.conf`

```[tarto@dns1 ~]$ sudo cat /etc/named.conf
//
// named.conf
//

options {
        listen-on port 53 { 127.0.0.1; 10.3.0.2; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { any; };

        recursion yes;
        dnssec-enable yes;
        dnssec-validation yes;
        managed-keys-directory "/var/named/dynamic";
        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";
        include "/etc/crypto-policies/back-ends/bind.config";
};
logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};
zone "." IN {
        type hint;
        file "named.ca";
};

zone "server1.tp3" IN {
        type master;                           # type of zone
        file "/var/named/server1.tp3.forward"; # location of forward zone file
        allow-update { none; };
};
```
  - des fichiers de zone "forward"
    - permet d'indiquer une correspondance nom -> IP
    - un fichier par zone forward

```
[tarto@dns1 ~]$ sudo cat /var/named/server1.tp3.forward
@       IN SOA dns1.serveur1.tp3 toto.mail (
                                2014030801      ; serial
                                3600      ; refresh
                                1800      ; retry
                                604800      ; expire
                                86400 )    ; minimum

; define nameservers
        IN  NS  dns1.serveur1.tp3.
;
; DNS Server IP addresses and hostnames
dns1 IN  A   10.3.0.2
;
;client records
routeur IN A 10.3.0.126
```

🌞 **Tester le DNS depuis `marcel.client1.tp3`**

- définissez **manuellement** l'utilisation de votre serveur DNS
    
        [tarto@marcel ~]$ cat /etc/resolv.conf
        # Generated by NetworkManager
        search lan
        nameserver 10.3.0.2
        
- essayez une résolution de nom avec `dig`
  - une résolution de nom classique
    - `dig <NOM>` pour obtenir l'IP associée à un nom
    - on teste la zone forward

```
[tarto@marcel ~]$ dig router.server1.tp3

; <<>> DiG 9.11.26-RedHat-9.11.26-4.el8_4 <<>> router.server1.tp3
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 50610
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: d5402354453ff08ef3f8be64616c7c8a73d9b032b0d22178 (good)
;; QUESTION SECTION:
;router.server1.tp3.            IN      A

;; AUTHORITY SECTION:
server1.tp3.            86400   IN      SOA     dns1.serveur1.tp3.server1.tp3. toto.mail.server1.tp3. 2014030801 3600 1800 604800 86400

;; Query time: 0 msec
;; SERVER: 10.3.0.2#53(10.3.0.2)
;; WHEN: Sun Oct 17 21:42:03 CEST 2021
;; MSG SIZE  rcvd: 139
```
- prouvez que c'est bien votre serveur DNS qui répond pour chaque `dig`

Par exemple, vous devriez pouvoir exécuter les commandes suivantes :

```bash
$ dig marcel.client1.tp3
$ dig dhcp.client1.tp3
```

> Pour rappel, avec `dig`on peut préciser directement sur la ligne de commande à quel serveur poser la question avec  le caractère `@`. Par exemple `dig google.com @8.8.8.8` permet de préciser explicitement qu'on veut demander à `8.8.8.8` à quelle IP se trouve `google.com`.

🌞 Configurez l'utilisation du serveur DNS sur TOUS vos noeuds

- les serveurs, on le fait à la main
- les clients, c'est fait *via* DHCP

        [tarto@router ~]$ cat /etc/resolv.conf
        nameserver 10.3.0.2
        
        [tarto@dhcp ~]$ cat /etc/resolv.conf
        nameserver 10.3.0.2
        
⚠️**A partir de maintenant, vous n'utiliserez plus DU TOUT les IPs pour communiquer entre vos machines, mais uniquement leurs noms.**

## 3. Get deeper

On va affiner un peu la configuration des outils mis en place.

### A. DNS forwarder

🌞 **Affiner la configuration du DNS**

- faites en sorte que votre DNS soit désormais aussi un forwarder DNS
- c'est à dire que s'il ne connaît pas un nom, il ira poser la question à quelqu'un d'autre

> Hint : c'est la clause `recursion` dans le fichier `/etc/named.conf`. Et c'est déjà activé par défaut en fait.

🌞 Test !

- vérifier depuis `marcel.client1.tp3` que vous pouvez résoudre des noms publics comme `google.com` en utilisant votre propre serveur DNS (commande `dig`)

```[tarto@marcel ~]$ dig google.com

; <<>> DiG 9.11.26-RedHat-9.11.26-4.el8_4 <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 11892
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 4, ADDITIONAL: 9

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 9ef181ba056cb676ad4e31ae616c7fd5129990dfd6d75310 (good)
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             300     IN      A       216.58.198.206

;; AUTHORITY SECTION:
google.com.             172800  IN      NS      ns2.google.com.
google.com.             172800  IN      NS      ns1.google.com.
google.com.             172800  IN      NS      ns3.google.com.
google.com.             172800  IN      NS      ns4.google.com.

;; ADDITIONAL SECTION:
ns2.google.com.         172800  IN      A       216.239.34.10
ns1.google.com.         172800  IN      A       216.239.32.10
ns3.google.com.         172800  IN      A       216.239.36.10
ns4.google.com.         172800  IN      A       216.239.38.10
ns2.google.com.         172800  IN      AAAA    2001:4860:4802:34::a
ns1.google.com.         172800  IN      AAAA    2001:4860:4802:32::a
ns3.google.com.         172800  IN      AAAA    2001:4860:4802:36::a
ns4.google.com.         172800  IN      AAAA    2001:4860:4802:38::a

;; Query time: 91 msec
;; SERVER: 10.3.0.2#53(10.3.0.2)
;; WHEN: Sun Oct 17 21:56:06 CEST 2021
;; MSG SIZE  rcvd: 331
```
- pour que ça fonctionne, il faut que `dns1.server1.tp3` soit lui-même capable de résoudre des noms, avec `1.1.1.1` par exemple

### B. On revient sur la conf du DHCP

🖥️ **VM johnny.client1.tp3**

🌞 **Affiner la configuration du DHCP**

- faites en sorte que votre DHCP donne désormais l'adresse de votre serveur DNS aux clients
- créer un nouveau client `johnny.client1.tp3` qui récupère son IP, et toutes les nouvelles infos, en DHCP

```
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:69:1e:12 brd ff:ff:ff:ff:ff:ff
    inet 10.3.0.140/26 brd 10.3.0.191 scope global dynamic noprefixroute enp0s8
       valid_lft 491sec preferred_lft 491sec
    inet6 fe80::a00:27ff:fe69:1e12/64 scope link
       valid_lft forever preferred_lft forever
       
[tarto@johny ~]$ ip r s
default via 10.3.0.190 dev enp0s8 proto dhcp metric 100
default via 10.3.0.130 dev enp0s8 proto static metric 100
10.3.0.128/26 dev enp0s8 proto kernel scope link src 10.3.0.140 metric 100

[tarto@johny ~]$ cat /etc/resolv.conf
# Generated by NetworkManager
search lan
nameserver 10.3.0.2
```

---

# Entracte

[...]

---

# III. Services métier

## 1. Serveur Web

🖥️ **VM web1.server2.tp3**

🌞 **Setup d'une nouvelle machine, qui sera un serveur Web, une belle appli pour nos clients**

- réseau `server2`
- hello `web1.server2.tp3` !
- [📝**checklist**📝](#checklist)
- vous utiliserez le serveur web que vous voudrez, le but c'est d'avoir un serveur web fast, pas d'y passer 1000 ans :)
  - réutilisez [votre serveur Web du TP1 Linux](https://gitlab.com/it4lik/b2-linux-2021/-/tree/main/tp/1#2-cr%C3%A9ation-de-service)
  - ou montez un bête NGINX avec la page d'accueil (ça se limite à un `dnf install` puis `systemctl start nginx`)

        [tarto@web1 ~]$ sudo systemctl status nginx
        ● nginx.service - The nginx HTTP and reverse proxy server
           Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
           Active: active (running) since Sun 2021-10-17 21:59:50 CEST; 2s ago
           [...]
           
  - ou ce que vous voulez, du moment que c'est fast
  - **dans tous les cas, n'oubliez pas d'ouvrir le port associé dans le firewall** pour que le serveur web soit joignable

> Une bête page HTML fera l'affaire. On est pas là pour faire du design. Et vous prenez pas la tête pour l'install, appelez-moi vite s'il faut, on est pas en système ni en Linux non plus. On est en réseau, on veut juste un serveur web derrière un port :)

🌞 **Test test test et re-test**

- testez que votre serveur web est accessible depuis `marcel.client1.tp3`
  - utilisez la commande `curl` pour effectuer des requêtes HTTP
    
        [tarto@marcel ~]$ curl 10.3.0.194/28
        <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

        <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
          <head>
          [...]
---

[...]

## 2. Partage de fichiers

### A. L'introduction wola

[...]

### B. Le setup wola

🖥️ **VM nfs1.server2.tp3**

🌞 **Setup d'une nouvelle machine, qui sera un serveur NFS**

- réseau `server2`
- son nooooom : `nfs1.server2.tp3` !
- je crois que vous commencez à connaître la chanson... Google "nfs server rocky linux"
  - [ce lien me semble être particulièrement simple et concis](https://www.server-world.info/en/note?os=Rocky_Linux_8&p=nfs&f=1)
- **vous partagerez un dossier créé à cet effet : `/srv/nfs_share/`**

        [tarto@nfs1 ~]$ cat /etc/exports
        /srv/nfs_share/ 10.3.0.0/24(rw,no_root_squash)

🌞 **Configuration du client NFS**

- effectuez de la configuration sur `web1.server2.tp3` pour qu'il accède au partage NFS

```
[tarto@web1 ~]$ sudo mount -t nfs nfs1.server2.tp3:/srv/nfs_share /srv/nfs
[tarto@web1 ~]$ df -hT
Filesystem                      Type      Size  Used Avail Use% Mounted on
devtmpfs                        devtmpfs  892M     0  892M   0% /dev
tmpfs                           tmpfs     909M     0  909M   0% /dev/shm
tmpfs                           tmpfs     909M  8.5M  901M   1% /run
tmpfs                           tmpfs     909M     0  909M   0% /sys/fs/cgroup
/dev/mapper/rl-root             xfs       6.2G  1.9G  4.3G  31% /
/dev/sda1                       xfs      1014M  234M  781M  24% /boot
tmpfs                           tmpfs     182M     0  182M   0% /run/user/1000
nfs1.server2.tp3:/srv/nfs_share nfs4      6.2G  1.9G  4.4G  30% /srv/nfs
```
- le partage NFS devra être monté dans `/srv/nfs/`
- [sur le même site, y'a ça](https://www.server-world.info/en/note?os=Rocky_Linux_8&p=nfs&f=2)

🌞 **TEEEEST**

- tester que vous pouvez lire et écrire dans le dossier `/srv/nfs` depuis `web1.server2.tp3`
- vous devriez voir les modifications du côté de  `nfs1.server2.tp3` dans le dossier `/srv/nfs_share/`

        [tarto@web1 ~]$ sudo touch /srv/nfs/test
        [tarto@nfs1 ~]$ sudo ls /srv/nfs_share/
        test

# IV. Un peu de théorie : TCP et UDP

Bon bah avec tous ces services, on a de la matière pour bosser sur TCP et UDP. P'tite partie technique pure avant de conclure.

🌞 **Déterminer, pour chacun de ces protocoles, s'ils sont encapsulés dans du TCP ou de l'UDP :**

- SSH
- HTTP
- DNS
- NFS

📁 **Captures réseau `tp3_ssh.pcap`, `tp3_http.pcap`, `tp3_dns.pcap` et `tp3_nfs.pcap`**

> **Prenez le temps** de réfléchir à pourquoi on a utilisé TCP ou UDP pour transmettre tel ou tel protocole. Réfléchissez à quoi servent chacun de ces protocoles, et de ce qu'on a besoin qu'ils réalisent.

🌞 **Expliquez-moi pourquoi je ne pose pas la question pour DHCP.**

🌞 **Capturez et mettez en évidence un *3-way handshake***

📁 **Capture réseau `tp3_3way.pcap`**

# V. El final

🌞 **Bah j'veux un schéma.**

![](https://i.imgur.com/9TA4va0.png)


| Nom du réseau | Adresse du réseau | Masque        | Nombre de clients possibles | Adresse passerelle | [Adresse broadcast](../../cours/lexique/README.md#adresse-de-diffusion-ou-broadcast-address) |
|---------------|-------------------|---------------|-----------------------------|--------------------|----------------------------------------------------------------------------------------------|
| `client1`     | `10.3.0.128`        | `255.255.255.192` | 62                           | `10.3.0.129`         | `10.3.0.191`                                                                                   |
| `server1`     | `10.3.0.0`        | `255.255.255.128` | 126                           | `10.3.0.1`         | `10.3.0.127`                                                                                   |
| `server2`     | `10.3.0.192`        | `255.255.255.240` | 14                           | `10.3.0.193`         | `10.3.0.207`                                                                                   |

| Nom machine          | Adresse IP `client1` | Adresse IP `server1` | Adresse IP `server2` | Adresse de passerelle |
|----------------------|----------------------|----------------------|----------------------|-----------------------|
| `router.tp3`         | `10.3.0.190/26`       | `10.3.0.126/25`      | `10.3.0.206/28`       | Carte NAT             |
| `dhcp.client1.tp3`   | `10.3.0.130/26`        | x                    | x                    | `10.3.0.129/26`        |
| `marcel.client1.tp3` | `dynamique`          | x                    | x                    | `10.3.0.129/26`        |
| `dns1.server1.tp3`   |  x                   | `10.3.0.2/25`      | x                    | `10.3.0.1/25`       |
| `johnny.client1.tp3` | `dynamique`          | x                    | x                    | `10.3.0.129/26`        |
| `web1.server2.tp3`   |  x                   | x                    | `10.3.0.194/28`       | `10.3.0.193/28`        |
| `nfs1.server2.tp3`   |  x                   | x                    | `10.3.0.195/28`       | `10.3.0.193/28`        |