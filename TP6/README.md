# TP6 : Un LAN maîtrisé meo ?

## II. Serveur DNS

### 🌞 Dans le rendu, je veux

### un cat des 3 fichiers de conf (fichier principal + deux fichiers de zone)
```
[toto@dns ~]$ sudo cat /etc/named.conf
//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

options {
        listen-on port 53 { 127.0.0.1; any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { localhost; any; };
        allow-query-cache { localhost; any; };

        recursion yes;

        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";
        geoip-directory "/usr/share/GeoIP";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "tp6.b1" IN {
     type master;
     file "tp6.b1.db";
     allow-update { none; };
     allow-query {any; };
};

zone "1.4.10.in-addr.arpa" IN {
     type master;
     file "tp6.b1.rev";
     allow-update { none; };
     allow-query { any; };
};


include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```

```
$TTL 86400
@ IN SOA dns.tp6.b1. admin.tp6.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

@ IN NS dns.tp6.b1.

dns       IN A 10.6.1.101
john      IN A 10.6.1.11
```

```
[toto@dns ~]$ sudo cat /var/named/tp6.b1.rev
$TTL 86400
@ IN SOA dns.tp6.b1. admin.tp6.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns.tp6.b1.

; Reverse lookup
101 IN PTR dns.tp6.b1.
11 IN PTR john.tp6.b1.
```

### un systemctl status named qui prouve que le service tourne bien
```
[toto@dns ~]$ sudo systemctl status named
● named.service - Berkeley Internet Name Domain (DNS)
     Loaded: loaded (/usr/lib/systemd/system/named.service; enabled; prese>
     Active: active (running) since Fri 2023-11-17 15:45:42 CET; 8s ago
    Process: 1879 ExecStartPre=/bin/bash -c if [ ! "$DISABLE_ZONE_CHECKING>
    Process: 1881 ExecStart=/usr/sbin/named -u named -c ${NAMEDCONF} $OPTI>
   Main PID: 1882 (named)
      Tasks: 4 (limit: 4674)
     Memory: 16.5M
        CPU: 33ms
     CGroup: /system.slice/named.service
             └─1882 /usr/sbin/named -u named -c /etc/named.conf

Nov 17 15:45:42 dns.tp6.b1 named[1882]: network unreachable resolving './D>
Nov 17 15:45:42 dns.tp6.b1 named[1882]: network unreachable resolving './N>
Nov 17 15:45:42 dns.tp6.b1 named[1882]: zone 1.0.0.0.0.0.0.0.0.0.0.0.0.0.0>
Nov 17 15:45:42 dns.tp6.b1 named[1882]: zone localhost/IN: loaded serial 0
Nov 17 15:45:42 dns.tp6.b1 named[1882]: zone localhost.localdomain/IN: loa>
Nov 17 15:45:42 dns.tp6.b1 named[1882]: all zones loaded
Nov 17 15:45:42 dns.tp6.b1 systemd[1]: Started Berkeley Internet Name Doma>
Nov 17 15:45:42 dns.tp6.b1 named[1882]: running
Nov 17 15:45:42 dns.tp6.b1 named[1882]: managed-keys-zone: Initializing au>
Nov 17 15:45:42 dns.tp6.b1 named[1882]: resolver priming query complete
lines 1-22/22 (END)
^C
```

### une commande ss qui prouve que le service écoute bien sur un port
```
[toto@dns ~]$ sudo ss -antp
State    Recv-Q   Send-Q     Local Address:Port       Peer Address:Port    Process
LISTEN   0        4096           127.0.0.1:953             0.0.0.0:*        users:(("named",pid=1882,fd=24))
LISTEN   0        128              0.0.0.0:22              0.0.0.0:*        users:(("sshd",pid=678,fd=3))
LISTEN   0        10             127.0.0.1:53              0.0.0.0:*        users:(("named",pid=1882,fd=17))
LISTEN   0        10            10.6.1.101:53              0.0.0.0:*        users:(("named",pid=1882,fd=21))
ESTAB    0        52            10.6.1.101:22            10.6.1.10:54191    users:(("sshd",pid=1284,fd=4),("sshd",pid=1279,fd=4))
LISTEN   0        4096               [::1]:953                [::]:*        users:(("named",pid=1882,fd=25))
LISTEN   0        128                 [::]:22                 [::]:*        users:(("sshd",pid=678,fd=4))
LISTEN   0        10                 [::1]:53                 [::]:*        users:(("named",pid=1882,fd=23))
```

### 🌞 Ouvrez le bon port dans le firewall

```
[toto@dns ~]$ sudo firewall-cmd --add-port=53/tcp --permanent
success
```

## 3. Test

### 🌞 Sur la machine john.tp6.b1

```
[toto@john ~]$ dig john.tp6.b1

; <<>> DiG 9.16.23-RH <<>> john.tp6.b1
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 20565
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 073a336077f7cccc0100000065578768746af3e8364744b0 (good)
;; QUESTION SECTION:
;john.tp6.b1.                   IN      A

;; ANSWER SECTION:
john.tp6.b1.            86400   IN      A       10.6.1.11

;; Query time: 4 msec
;; SERVER: 10.6.1.101#53(10.6.1.101)
;; WHEN: Fri Nov 17 16:31:48 CET 2023
;; MSG SIZE  rcvd: 84

```

```
[toto@john ~]$ dig dns.tp6.b1

; <<>> DiG 9.16.23-RH <<>> dns.tp6.b1
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32901
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 3720be1c1d7ca1d801000000655787d9b5bb83e653fc3ec4 (good)
;; QUESTION SECTION:
;dns.tp6.b1.                    IN      A

;; ANSWER SECTION:
dns.tp6.b1.             86400   IN      A       10.6.1.101

;; Query time: 3 msec
;; SERVER: 10.6.1.101#53(10.6.1.101)
;; WHEN: Fri Nov 17 16:33:42 CET 2023
;; MSG SIZE  rcvd: 83

```

```
[toto@john ~]$ dig www.ynov.com

; <<>> DiG 9.16.23-RH <<>> www.ynov.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 46992
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 6ea0ec10b3b0853601000000655787ec81f14e956d67c817 (good)
;; QUESTION SECTION:
;www.ynov.com.                  IN      A

;; ANSWER SECTION:
www.ynov.com.           300     IN      A       172.67.74.226
www.ynov.com.           300     IN      A       104.26.10.233
www.ynov.com.           300     IN      A       104.26.11.233

;; Query time: 324 msec
;; SERVER: 10.6.1.101#53(10.6.1.101)
;; WHEN: Fri Nov 17 16:34:00 CET 2023
;; MSG SIZE  rcvd: 117
```

### 🌞 Sur votre PC

```
PS C:\Users\crecy> nslookup john.tp6.b1 10.6.1.101
Serveur :   UnKnown
Address:  10.6.1.101

Nom :    john.tp6.b1
Address:  10.6.1.11

```

## III. Serveur DHCP

### 🌞 Installer un serveur DHCP

```
[toto@dhcp ~]$ sudo cat /etc/dhcp/dhcpd.conf
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#
# create new
# specify domain name
option domain-name     "tp6.b1";
# default lease time
default-lease-time 600;
# max lease time
max-lease-time 7200;
# this DHCP server to be declared valid
authoritative;
# specify network address and subnetmask
subnet 10.6.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.6.1.13 10.6.1.37;
    # specify gateway
    option routers 10.6.1.254;
    # specify dns server
    option domain-name-servers 10.6.1.101;
}
```
### 🌞 Test avec john.tp6.b1

```
[toto@john ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:67:c9:20 brd ff:ff:ff:ff:ff:ff
    inet 10.6.1.13/24 brd 10.6.1.255 scope global dynamic noprefixroute enp0s3
       valid_lft 455sec preferred_lft 455sec
    inet6 fe80::a00:27ff:fe67:c920/64 scope link
       valid_lft forever preferred_lft forever
```

```
[toto@john ~]$ ip route show
default via 10.6.1.254 dev enp0s3 proto dhcp src 10.6.1.13 metric 100
10.6.1.0/24 dev enp0s3 proto kernel scope link src 10.6.1.13 metric 100
```

```
[toto@john ~]$ sudo cat /etc/resolv.conf
[sudo] password for toto:
# Generated by NetworkManager
search tp6.b1
nameserver 10.6.1.101
```

```
[toto@john ~]$ ping google.com
PING google.com (142.250.201.174) 56(84) bytes of data.
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=1 ttl=115 time=11.9 ms
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=2 ttl=115 time=22.6 ms
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=3 ttl=115 time=18.7 ms
^C
--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 11.948/17.754/22.591/4.398 ms
```

### 🌞 Requête web avec john.tp6.b1

```
[toto@john ~]$ curl www.ynov.com
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>302 Found</title>
</head><body>
<h1>Found</h1>
<p>The document has moved <a href="https://www.ynov.com/">here</a>.</p>
<hr>
<address>Apache/2.4.41 (Ubuntu) Server at ynov.com Port 80</address>
</body></html>
```

### 🦈 Capture tp6_web.pcapng

