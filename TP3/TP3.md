# TP3 : On va router des trucs

## I. ARP

## 1. Echange ARP
### 🌞Générer des requêtes ARP
```
[toto@localhost ~]$ ping 10.3.1.12
PING 10.3.1.12 (10.3.1.12) 56(84) bytes of data.
64 bytes from 10.3.1.12: icmp_seq=1 ttl=64 time=2.11 ms
64 bytes from 10.3.1.12: icmp_seq=2 ttl=64 time=2.24 ms
64 bytes from 10.3.1.12: icmp_seq=3 ttl=64 time=3.90 ms
64 bytes from 10.3.1.12: icmp_seq=4 ttl=64 time=2.38 ms
64 bytes from 10.3.1.12: icmp_seq=5 ttl=64 time=1.74 ms
64 bytes from 10.3.1.12: icmp_seq=6 ttl=64 time=1.62 ms
64 bytes from 10.3.1.12: icmp_seq=7 ttl=64 time=1.16 ms
^C
--- 10.3.1.12 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6017ms
rtt min/avg/max/mdev = 1.158/2.163/3.904/0.808 ms
```
🌞 Table ARP John
```
[toto@localhost ~]$ ip neigh show
10.3.1.12 dev enp0s3 lladdr 🌞08:00:27:cd:7f:3d🌞 STALE
10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:49 DELAY
```
🌞 Table ARP Marcel
```
[toto@localhost ~]$ ip neigh show
10.3.1.1 dev enp0s3 lladdr 0a:00:27:00:00:49 DELAY
10.3.1.11 dev enp0s3 lladdr 🌞08:00:27:9b:1e:88🌞 STALE
```
🌞 Adresse MAC de Marcel
```
[toto@localhost ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 🌞08:00:27:cd:7f:3d🌞 brd ff:ff:ff:ff:ff:ff
    inet 10.3.1.12/24 brd 10.3.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fecd:7f3d/64 scope link
       valid_lft forever preferred_lft forever
```
## 2. Analyse de trames
## 🌞Analyse de trames

```
[toto@localhost ~]$ sudo tcpdump
dropped privs to tcpdump
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on enp0s3, link-type EN10MB (Ethernet), snapshot length 262144 bytes
16:40:21.523730 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 2700396405:2700396465, ack 2863952920, win 501, length 60
16:40:21.527633 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 60:96, ack 1, win 501, length 36
16:40:21.528457 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [.], ack 96, win 8191, length 0
16:40:21.528727 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 96:204, ack 1, win 501, length 108
16:40:21.529649 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 204:240, ack 1, win 501, length 36
16:40:21.530477 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [.], ack 240, win 8191, length 0
16:40:21.530670 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 240:300, ack 1, win 501, length 60
16:40:21.531549 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 300:368, ack 1, win 501, length 68
16:40:21.532649 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [.], ack 368, win 8190, length 0
16:40:21.532989 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 368:436, ack 1, win 501, length 68
16:40:21.570421 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 436:1608, ack 1, win 501, length 1172
16:40:21.571456 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [.], ack 1608, win 8195, length 0
16:40:21.571456 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [P.], seq 1:37, ack 1608, win 8195, length 36
16:40:21.613180 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [.], ack 37, win 501, length 0
16:40:21.673241 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 1608:2084, ack 37, win 501, length 476
16:40:21.714752 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [.], ack 2084, win 8193, length 0
16:40:21.777793 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 2084:2344, ack 37, win 501, length 260
16:40:21.819480 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [.], ack 2344, win 8192, length 0
16:40:21.882073 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 2344:2604, ack 37, win 501, length 260
16:40:21.923959 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [.], ack 2604, win 8191, length 0
16:40:21.984707 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 2604:2864, ack 37, win 501, length 260
16:40:22.026337 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [.], ack 2864, win 8190, length 0
16:40:22.089619 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 2864:3124, ack 37, win 501, length 260
16:40:22.131809 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [.], ack 3124, win 8195, length 0
16:40:22.193467 IP localhost.localdomain.ssh > 10.3.1.1.64584: Flags [P.], seq 3124:3384, ack 37, win 501, length 260
16:40:22.234778 IP 10.3.1.1.64584 > localhost.localdomain.ssh: Flags [.], ack 3384, win 8194, length 0
^C
26 packets captured
30 packets received by filter
0 packets dropped by kernel
```

## ROUTAGE

# 🌞Ajouter les routes statiques nécessaires pour que john et marcel puissent se ping

ca marche pas, j'ai bien fait les routes mais je ne peux pas faire un ping entre marcel et john 