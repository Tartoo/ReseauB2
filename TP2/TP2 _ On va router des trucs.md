#    **TP2 : On va router des trucs     FLAMENT**

##    **I. ARP**
###    1. Echange ARP : 
-    Générer des requêtes ARP:
        -    effectuer un ping d'une machine à l'autre:
                #### Commande:`ping 10.2.1.12 | ping 10.2.1.11`
                #### Resultat:
                    [tarto@node1 /]$ ping 10.2.1.12
                    PING 10.2.1.12 (10.2.1.12) 56(84) bytes of data.
                    64 bytes from 10.2.1.12: icmp_seq=1 ttl=64 time=0.290 ms
                    4 bytes from 10.2.1.12: icmp_seq=2 ttl=64 time=0.287 ms
                    64 bytes from 10.2.1.12: icmp_seq=3 ttl=64 time=0.201 ms
                    --- 10.2.1.12 ping statistics ---
                    12 packets transmitted, 12 received, 0% packet loss, time 11267ms
                    rtt min/avg/max/mdev = 0.201/0.398/0.722/0.161 ms
                    -----------------------------------------------------------------------------
                    [tarto@node2 /]$ ping 10.2.1.11
                    PING 10.2.1.11 (10.2.1.11) 56(84) bytes of data.
                    64 bytes from 10.2.1.11: icmp_seq=1 ttl=64 time=0.275 ms
                    64 bytes from 10.2.1.11: icmp_seq=2 ttl=64 time=0.346 ms
                    64 bytes from 10.2.1.11: icmp_seq=3 ttl=64 time=0.164 ms
                    --- 10.2.1.11 ping statistics ---
                    4 packets transmitted, 4 received, 0% packet loss, time 3079ms
                    rtt min/avg/max/mdev = 0.160/0.236/0.346/0.079 ms
        -    observer les tables ARP des deux machines:
                #### Commande:`ip neigh show`
                #### Resultat:
                    tarto@node1 /]$ ip neigh show
                    10.2.1.1 dev enp0s8 lladdr 0a:00:27:00:00:2d REACHABLE
                    10.2.1.12 dev enp0s8 lladdr 08:00:27:1a:d0:d9 STALE 
                    -----------------------------------------------------------------------------
                    [tarto@node2 /]$ ip neigh show
                    10.2.1.11 dev enp0s8 lladdr 08:00:27:2c:37:ec STALE
                    10.2.1.1 dev enp0s8 lladdr 0a:00:27:00:00:2d REACHABLE
        -    repérer l'adresse MAC de node1 dans la table ARP de node2 et vice-versa:
        `L'adresse MAC de node1 est 08:00:27:2c:37:ec et celle de node2 est 08:00:27:1a:d0:d9*`
        -  prouvez que l'info est correcte (que l'adresse MAC que vous voyez dans la table est bien celle de la machine correspondante)  
            #### Commande:`ip a
            #### Resultat:
                [tarto@node1 /]$ ip a
                3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
                link/ether 08:00:27:2c:37:ec brd ff:ff:ff:ff:ff:ff
                inet 10.2.1.11/24 brd 10.2.1.255 scope global noprefixroute enp0s8
                ---------------------------------------------------------------------------------
                [tarto@node2 ~]$ ip a
                3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
                link/ether 08:00:27:1a:d0:d9 brd ff:ff:ff:ff:ff:ff
                inet 10.2.1.12/24 brd 10.2.1.255 scope global noprefixroute enp0s8
On voit que les adresses MAC correspondent à celles vu avec la table ARP.

***

###    2. Analyse de trames :
-    Capture de trames:
        #### Commande:`sudo tcpdump -i enp0s8 -w tp2_arp.pcap`
        
-    Vidage des tables ARP + ping:
        #### Commande:`sudo ip neigh flush all | ping 10.2.1.12 | ping 10.2.1.11`

-    Visualisation de la capture sur WireShark:
        -    Mise en évidence des trames ARP:
                | ordre | type trame  | source                   | destination                |
                |-------|-------------|--------------------------|----------------------------|
                | 1     | Requête ARP | node1: `80:00:27:1a:d0:d9` | Broadcast `00:00:00:00:00`
                | 2     | Réponse ARP | node2: `80:00:27:2c:37:ec` | node1:`80:00:27:1a:d0:d9`   |


***

##    **II. Routage**
###    1. Mise en place du routage :
*    Activer le routage sur le noeud router.net2.tp2:
        #### Commandes:`sudo firewall-cmd --add-masquerade --zone=public --permanent`
        #### Résultat:
            [tarto@router ~]$ sudo firewall-cmd --add-masquerad --zone=public --permanent
            success
*    Ajouter les routes statiques nécessaires pour que node1.net1.tp2 et marcel.net2.tp2 puissent se ping:
        #### Commandes:`sudo vim route-enp0s8 | ip r s | ping 10.2.2.12 | ping 10.2.1.11`
        #### Résultats:
            [tarto@node1 /]$ ip r s
            default via 192.168.1.254 dev enp0s8 proto static metric 100
            10.2.1.0/24 dev enp0s8 proto kernel scope link src 10.2.1.11 metric 100
            10.2.2.0/24 via 10.2.1.254 dev enp0s8 proto static metric 100
            192.168.1.254 dev enp0s8 proto static scope link metric 100 
            ---------------------------------------------------------------------------
            [tarto@marcel /]$ ip r s
            default via 192.168.1.254 dev enp0s8 proto static metric 100
            10.2.1.0/24 via 10.2.2.254 dev enp0s8 proto static metric 100
            10.2.2.0/24 dev enp0s8 proto kernel scope link src 10.2.2.12 metric 100
            192.168.1.254 dev enp0s8 proto static scope link metric 100
            ---------------------------------------------------------------------------
            [tarto@node1 /]$ ping 10.2.2.12
            PING 10.2.2.12 (10.2.2.12) 56(84) bytes of data.
            64 bytes from 10.2.2.12: icmp_seq=1 ttl=63 time=0.963 ms
            64 bytes from 10.2.2.12: icmp_seq=2 ttl=63 time=0.709 ms
            64 bytes from 10.2.2.12: icmp_seq=3 ttl=63 time=0.542 ms
            ^C
            --- 10.2.2.12 ping statistics ---
            3 packets transmitted, 3 received, 0% packet loss, time 2041ms
            rtt min/avg/max/mdev = 0.542/0.738/0.963/0.173 ms
            ---------------------------------------------------------------------------
            [tarto@marcel /]$ ping 10.2.1.11
            PING 10.2.1.11 (10.2.1.11) 56(84) bytes of data.
            64 bytes from 10.2.1.11: icmp_seq=1 ttl=63 time=0.668 ms
            64 bytes from 10.2.1.11: icmp_seq=2 ttl=63 time=0.683 ms
            64 bytes from 10.2.1.11: icmp_seq=3 ttl=63 time=0.662 ms
            ^C
            --- 10.2.1.11 ping statistics ---
            3 packets transmitted, 3 received, 0% packet loss, time 2026ms
            rtt min/avg/max/mdev = 0.662/0.671/0.683/0.008 ms

***

###    2. Analyse de trames :
*    Analyse des échanges ARP:
        *    videz les tables ARP des trois noeuds:
                #### Commandes:`sudo ip neigh flush all`
        *    effectuez un ping de node1.net1.tp2 vers marcel.net2.tp2:
                #### Commandes:`ping 10.2.2.12`
                #### Résultats:
                    [tarto@node1 /]$ ping 10.2.2.12
                    PING 10.2.2.12 (10.2.2.12) 56(84) bytes of data.
                    64 bytes from 10.2.2.12: icmp_seq=1 ttl=63 time=1.51 ms
                    64 bytes from 10.2.2.12: icmp_seq=2 ttl=63 time=0.698 ms
                    64 bytes from 10.2.2.12: icmp_seq=3 ttl=63 time=0.538 ms
                    64 bytes from 10.2.2.12: icmp_seq=4 ttl=63 time=0.715 ms
                    ^C
                    --- 10.2.2.12 ping statistics ---
                    4 packets transmitted, 4 received, 0% packet loss, time 3081ms
                    rtt min/avg/max/mdev = 0.538/0.865/1.510/0.379 ms
        *    regardez les tables ARP des trois noeuds:
                #### Commandes:`ip neigh show`
                #### Résultats:
                    [tarto@node1 /]$ ip neigh show
                    10.2.1.254 dev enp0s8 lladdr 08:00:27:c8:6b:a8 STALE
                    10.2.1.1 dev enp0s8 lladdr 0a:00:27:00:00:40 DELAY
                    -----------------------------------------------------------------------------
                    [tarto@marcel /]$ ip neigh show
                    10.2.2.254 dev enp0s8 lladdr 08:00:27:ff:7c:5f STALE
                    10.2.2.1 dev enp0s8 lladdr 0a:00:27:00:00:45 DELAY
                    -----------------------------------------------------------------------------
                    [tarto@router /]$ ip neigh show
                    10.2.1.11 dev enp0s8 lladdr 08:00:27:4b:cc:00 STALE
                    10.2.1.1 dev enp0s8 lladdr 0a:00:27:00:00:40 DELAY
                    10.2.2.12 dev enp0s9 lladdr 08:00:27:04:f0:d0 STALE
        *    essayez de déduire un peu les échanges ARP qui ont eu lieu:
            Il y a eu des requêtes ARP, des réceptions et des réponses
        *    répétez l'opération précédente avec tcpdump:
                | Ordre | Type Trame  | IP Source | MAC Source                | IP Destination | MAC Destination |
                | ----- | ----------- | --------- | ------------------------- | -------------- | --------------- |
                | 1     | Requête ARP | x         | node1 `08:00:27:4b:cc:00` | x | Broadcast `ff:ff:ff:ff:ff`   |
                | 2     | Réponse ARP | x         | router `08:00:27:c8:6b:a8` | x | node1 `08:00:27:4b:cc:00` |
                | 3     | Requête ARP | x         | router `08:00:27:c8:6b:a8` | x | Broadcast `ff:ff:ff:ff:ff` |
                | 4     | Réponse ARP | x         | marcel `08:00:27:ff:7c:5f` | x | router `08:00:27:c8:6b:a8` |
                | 5     | Ping        | 10.2.1.12 | node1 `08:00:27:4b:cc:00` | 10.2.2.12 | marcel `08:00:27:ff:7c:5f` |
                | 6     | Pong        | 10.2.2.12 | marcel `08:00:27:ff:7c:5f` | 10.2.1.12 | node1 `08:00:27:4b:cc:00` |

***

###    3. Accès internet :
*    Donnez un accès internet à vos machines:
        #### Commandes:`sudo vim /etc/sysconfig/network-scripts/route-enp0s8 | ip route show | ping 8.8.8.8`
        #### Résultats:
            [tarto@node1 /]$ ip rout show
            default via 10.0.2.15 dev enp0s8 proto static metric 100
            10.0.2.15 dev enp0s8 proto static scope link metric 100
            10.2.1.0/24 dev enp0s8 proto kernel scope link src 10.2.1.11 metric 100
            10.2.2.0/24 via 10.2.1.254 dev enp0s8 proto static metric 100
            ---------------------------------------------------------------------------------
            [tarto@marcel /]$ ip route show
            default via 10.0.2.15 dev enp0s8 proto static metric 100
            10.0.2.15 dev enp0s8 proto static scope link metric 100
            10.2.1.0/24 via 10.2.2.254 dev enp0s8 proto static metric 100
            10.2.2.0/24 dev enp0s8 proto kernel scope link src 10.2.2.12 metric 100
            ---------------------------------------------------------------------------------
            [tarto@node1 /]$ ping 8.8.8.8
            PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
            64 bytes from 8.8.8.8: icmp_seq=1 ttl=114 time=55.6 ms
            64 bytes from 8.8.8.8: icmp_seq=2 ttl=114 time=39.2 ms
            64 bytes from 8.8.8.8: icmp_seq=4 ttl=114 time=33.9 ms
            ^C
            --- 8.8.8.8 ping statistics ---
            4 packets transmitted, 3 received, 25% packet loss, time 3047ms
            rtt min/avg/max/mdev = 33.884/42.864/55.550/9.225 ms 
            ---------------------------------------------------------------------------------
            [tarto@marcel /]$ ping 8.8.8.8
            PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
            64 bytes from 8.8.8.8: icmp_seq=2 ttl=114 time=30.2 ms
            64 bytes from 8.8.8.8: icmp_seq=3 ttl=114 time=49.9 ms
            64 bytes from 8.8.8.8: icmp_seq=4 ttl=114 time=40.7 ms
            From 10.2.2.12 icmp_seq=1 Destination Host Unreachable
            ^C
            --- 8.8.8.8 ping statistics ---
            4 packets transmitted, 3 received, +1 errors, 25% packet loss, time 3026ms
            rtt min/avg/max/mdev = 30.249/40.288/49.939/8.046 ms, pipe 4
*    donnez leur aussi l'adresse d'un serveur DNS qu'ils peuvent utiliser:
        #### Commandes:`sudo vim /etc/sysconfig/network-scripts/ifcfg-enp0s8`
        #### Résultats:
            [tarto@node1 /]$ dig google.com

            ; <<>> DiG 9.11.26-RedHat-9.11.26-4.el8_4 <<>> google.com
            ;; global options: +cmd
            ;; Got answer:
            ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 40865
            ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

            ;; OPT PSEUDOSECTION:
            ; EDNS: version: 0, flags:; udp: 512
            ;; QUESTION SECTION:
            ;google.com.                    IN      A

            ;; ANSWER SECTION:
            google.com.             35      IN      A       142.250.75.238

            ;; Query time: 32 msec
            ;; SERVER: 8.8.8.8#53(8.8.8.8)
            ;; WHEN: Sat Sep 25 11:44:35 CEST 2021
            ;; MSG SIZE  rcvd: 55
            ---------------------------------------------------------------------------------
            [tarto@marcel /]$ dig google.com

            ; <<>> DiG 9.11.26-RedHat-9.11.26-4.el8_4 <<>> google.com
            ;; global options: +cmd
            ;; Got answer:
            ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51528
            ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

            ;; OPT PSEUDOSECTION:
            ; EDNS: version: 0, flags:; udp: 512
            ;; QUESTION SECTION:
            ;google.com.                    IN      A

            ;; ANSWER SECTION:
            google.com.             144     IN      A       216.58.214.78

            ;; Query time: 46 msec
            ;; SERVER: 8.8.8.8#53(8.8.8.8)
            ;; WHEN: Sat Sep 25 11:45:13 CEST 2021
            ;; MSG SIZE  rcvd: 55
*    Analyse de trames: 
        
        | Ordre | Type Trame | IP Source | MAC Source | IP Destination | MAC Destination |
        | ----- | ----------- | --------- | ------------------------- | -------------- | --------------- |
        | 1 | ping | 10.2.1.12 | node1 `08:00:27:4b:cc:00` | 8.8.8.8 | `08:00:27:c8:6b:a8` |
        | 2 | pong | 8.8.8.8 | `08:00:27:c8:6b:a8` | 10.2.1.11 | node1 `08:00:27:4b:cc:00` |

***

##    **II. DHCP (pas complet :c)**

###    1. Mise en place du serveur DHCP :
*    #### **Commandes:**`sudo dnf -y install dhcp-server | sudo vim /etc/dhcp/dhcpd.conf | sudo firewall-cmd --add-service=dhcp | sudo firewall-cmd --runtime-to-permanent`
*    #### **Résultats:**`
            default-lease-time 600;
            max-lease-time 7200;
            authoritative;

            subnet 10.2.1.0 netmask 255.255.255.0 {
                    range dynamic-bootp 10.2.1.13 10.2.1.254;
                    option broadcast-address 10.2.1.255;
                    option routers 10.2.1.11;
            }
            ---------------------------------------------------------------------------------
            [tarto@node1 ~]$ sudo firewall-cmd --add-service=dhcp
            success
            [tarto@node1 ~]$ sudo firewall-cmd --runtime-to-permanent
            success
            
***

###    2. Récupération d'adresse IP:
*    #### **Commandes:**`sudo vim /etc/sysconfig/network-scripts/ifcfg-enp0s8 | ip a`
*    #### **Résultats:**
            [tarto@node2 ~]$ cat /etc/sysconfig/network-scripts/ifcfg-enp0s3
            NAME=enp0s3
            DEVICE=enp0s3

            BOOTPROTO=dhcp
            ONBOOT=yes
            ---------------------------------------------------------------------------------
            [tarto@node2 ~]$ ip a
            1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
                link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
                inet 127.0.0.1/8 scope host lo
                   valid_lft forever preferred_lft forever
                inet6 ::1/128 scope host
                   valid_lft forever preferred_lft forever
            2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
                    link/ether 08:00:27:7e:d9:34 brd ff:ff:ff:ff:ff:ff
                    inet 10.2.1.50/24 brd 10.2.1.255 scope global dynamic noprefixroute enp0s3
                       valid_lft 350sec preferred_lft 350sec
                inet6 fe80::a00:27ff:fe2a:49f2/64 scope link
                   valid_lft forever preferred_lft forever
 
***

###    3. Amélioration du serveur:
*    ajoutez de la configuration à votre DHCP pour qu'il donne aux clients, en plus de leur IP : 
        #### **Commandes:**`sudo vim /etc/dhcp/dhcpd.conf`
        #### **Résultats:**
            [tarto@node1 ~]$ sudo cat /etc/dhcp/dhcpd.conf
            #
            # DHCP Server Configuration file.
            #   see /usr/share/doc/dhcp-server/dhcpd.conf.example
            #   see dhcpd.conf(5) man page
            #
            default-lease-time 600;
            max-lease-time 7200;
            authoritative;
            subnet 10.2.1.0 netmask 255.255.255.0 {
                range dynamic-bootp 10.2.1.50 10.2.1.254;
                option broadcast-address 10.2.1.255;
                option routers 10.2.1.11;
                option domain-name-servers 1.1.1.1;
            }