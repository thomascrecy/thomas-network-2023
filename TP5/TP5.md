# TP5 : TCP, UDP et services réseau

## I. First Steps

🌞 Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP

```
🌞TCP    10.33.73.67:55598🌞      162.159.128.235:443🌞   ESTABLISHED
[Discord.exe]

🌞TCP    10.33.73.67:64151🌞      34.120.22.49:443🌞 
ESTABLISHED
[chrome.exe]

🌞TCP    10.33.73.67:55866🌞      35.186.198.239:443🌞     TIME_WAIT
🌞TCP    10.33.73.67:55867🌞      35.186.224.18:443🌞      ESTABLISHED
[Spotify.exe]

🌞TCP    127.0.0.1:55913🌞        127.0.0.1:55914🌞     
ESTABLISHED
[Steam.exe]

🌞TCP    10.33.73.67:56067🌞      104.19.152.69:443🌞      ESTABLISHED
[SNAP.exe]
```
## II. Setup Virtuel
## 1. SSH

### 🌞 Examinez le trafic dans Wireshark

🦈 tp5_3_way.pcapng

### 🌞 Demandez aux OS

```
  TCP    10.3.1.1:139           0.0.0.0:0              LISTENING
 Impossible d’obtenir les informations de propriétaire
  TCP    10.3.2.1:139           0.0.0.0:0              LISTENING
 Impossible d’obtenir les informations de propriétaire
  TCP    10.5.1.10:139          0.0.0.0:0              LISTENING
 Impossible d’obtenir les informations de propriétaire
  TCP    10.5.1.10:57790        10.5.1.11:22           ESTABLISHED
 [ssh.exe]
```

```
[toto@localhost ~]$ sudo ss -antp
[sudo] password for toto:
State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port  Process
LISTEN  0       128            0.0.0.0:22          0.0.0.0:*      users:(("sshd",pid=681,fd=3))
ESTAB   0       36           10.5.1.11:22        10.5.1.10:57790  users:(("sshd",pid=1444,fd=4),("sshd",pid=1440,fd=4))
LISTEN  0       128               [::]:22             [::]:*      users:(("sshd",pid=681,fd=4))
```

## 2. Routage

### 🌞 Prouvez que

```
[toto@node1 ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=112 time=17.4 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=112 time=16.8 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=112 time=18.0 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=112 time=17.1 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=112 time=18.2 ms
64 bytes from 8.8.8.8: icmp_seq=6 ttl=112 time=18.8 ms
64 bytes from 8.8.8.8: icmp_seq=7 ttl=112 time=17.4 ms
64 bytes from 8.8.8.8: icmp_seq=8 ttl=112 time=17.0 ms
64 bytes from 8.8.8.8: icmp_seq=9 ttl=112 time=18.1 ms
64 bytes from 8.8.8.8: icmp_seq=10 ttl=112 time=18.0 ms
64 bytes from 8.8.8.8: icmp_seq=11 ttl=112 time=19.1 ms
```

```
[toto@node1 ~]$ ping ynov.com
PING ynov.com (104.26.11.233) 56(84) bytes of data.
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=1 ttl=55 time=21.4 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=2 ttl=55 time=22.3 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=3 ttl=55 time=22.4 ms
64 bytes from 104.26.11.233 (104.26.11.233): icmp_seq=4 ttl=55 time=20.9 ms
```

## 3. Serveur Web

### 🌞 Créer le site web

```
[toto@web site_web_nul]$ sudo nano index.html
```

### 🌞 Donner les bonnes permissions

```
[toto@web site_web_nul]$ sudo chown -R nginx:nginx /var/www/site_web_nul
```

### 🌞 Créer un fichier de configuration NGINX pour notre site web

```
[toto@web conf.d]$ sudo nano site_web_nul.conf
```

### 🌞 Démarrer le serveur web !

```
[toto@web ~]$ systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset: disabled)
     Active: active (running) since Fri 2023-11-10 16:05:20 CET; 9s ago
    Process: 1530 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
    Process: 1531 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
    Process: 1532 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
   Main PID: 1533 (nginx)
      Tasks: 2 (limit: 4674)
     Memory: 2.0M
        CPU: 76ms
     CGroup: /system.slice/nginx.service
             ├─1533 "nginx: master process /usr/sbin/nginx"
             └─1534 "nginx: worker process"

Nov 10 16:05:20 web.tp5.b1 systemd[1]: Starting The nginx HTTP and reverse proxy server...
Nov 10 16:05:20 web.tp5.b1 nginx[1531]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Nov 10 16:05:20 web.tp5.b1 nginx[1531]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Nov 10 16:05:20 web.tp5.b1 systemd[1]: Started The nginx HTTP and reverse proxy server.
```

### 🌞 Ouvrir le port firewall

```
[toto@node1 ~]$ curl 10.5.1.12
<h1>MEOW</h1>
```

### 🌞 Visualiser le port en écoute

```
[toto@web ~]$ ss -antp
State        Recv-Q        Send-Q               Local Address:Port               Peer Address:Port        Process
LISTEN       0             128                        0.0.0.0:22                      0.0.0.0:*
LISTEN       0             511                        0.0.0.0:80                      0.0.0.0:*
ESTAB        0             0                        10.5.1.12:80                    10.5.1.10:59004
ESTAB        0             52                       10.5.1.12:22                    10.5.1.10:58793
ESTAB        0             0                        10.5.1.12:80                    10.5.1.10:59005
LISTEN       0             128                           [::]:22                         [::]:*
LISTEN       0             511                           [::]:80                         [::]:*
```

### 🌞 Analyse trafic

🦈 tp5_web.pcapng