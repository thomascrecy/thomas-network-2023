# TP7 : Do u secure

## I. Setup LAN

```
[toto@john ~]$ ping google.com
PING google.com (142.250.75.238) 56(84) bytes of data.
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=1 ttl=114 time=13.0 ms
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=2 ttl=114 time=13.5 ms
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=3 ttl=114 time=13.5 ms
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=4 ttl=114 time=12.2 ms
64 bytes from par10s41-in-f14.1e100.net (142.250.75.238): icmp_seq=5 ttl=114 time=11.6 ms
^C
--- google.com ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4007ms
rtt min/avg/max/mdev = 11.614/12.761/13.517/0.743 ms
```

## II. SSH

### 1. Fingerprint

### 🌞 Effectuez une connexion SSH en vérifiant le fingerprint

```
PS C:\Users\crecy> ssh toto@10.7.1.11
The authenticity of host '10.7.1.11 (10.7.1.11)' can't be established.
ED25519 key fingerprint is 🌞SHA256:wK6ydiTmUECEg8wZVFIrhy/LZJmcxeQpta75gVUb/X4.
This host key is known by the following other names/addresses:
    C:\Users\crecy/.ssh/known_hosts:10: 10.3.1.11
    C:\Users\crecy/.ssh/known_hosts:13: 10.3.1.12
    C:\Users\crecy/.ssh/known_hosts:14: 10.3.2.12
    C:\Users\crecy/.ssh/known_hosts:15: 10.5.1.11
    C:\Users\crecy/.ssh/known_hosts:16: 10.5.1.254
    C:\Users\crecy/.ssh/known_hosts:17: 10.5.1.12
    C:\Users\crecy/.ssh/known_hosts:18: 10.6.1.11
    C:\Users\crecy/.ssh/known_hosts:19: 10.6.1.254
    (8 additional names omitted)
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

```
[toto@john ~]$ sudo ssh-keygen -l -f /etc/ssh/ssh_host_ed25519_key
[sudo] password for toto:
256 🌞SHA256:wK6ydiTmUECEg8wZVFIrhy/LZJmcxeQpta75gVUb/X4 /etc/ssh/ssh_host_ed25519_key.pub (ED25519)
```

## 2. Conf serveur SSH

### 🌞 Consulter l'état actuel

```
[toto@router ~]$ ss -antp
State    Recv-Q   Send-Q     Local Address:Port       Peer Address:Port    Process
LISTEN   0        128              0.0.0.0:22              0.0.0.0:*
ESTAB    0        52            10.7.1.254:22            10.7.1.10:53672
LISTEN   0        128                 [::]:22                 [::]:*
```

### 🌞 Modifier la configuration du serveur SSH

```
[toto@router ~]$ sudo cat /etc/ssh/sshd_config
[...]
Port 22000
#AddressFamily any
ListenAddress 10.7.1.254
#ListenAddress ::
```

### 🌞 Prouvez que le changement a pris effet

```
[toto@router ~]$ ss -antp
State    Recv-Q   Send-Q     Local Address:Port       Peer Address:Port    Process
LISTEN   0        128           10.7.1.254:22000           0.0.0.0:*
ESTAB    0        52            10.7.1.254:22            10.7.1.10:53672
```

### 🌞 Effectuer une connexion SSH sur le nouveau port

```
PS C:\Users\crecy> ssh toto@10.7.1.254 -p 22000
toto@10.7.1.254's password:
Last login: Thu Nov 23 15:32:52 2023 from 10.7.1.10
```

## 3. Connexion par clé

### 🌞 Générer une paire de clés

```
PS C:\Users\crecy> ls .\.ssh\


    Répertoire : C:\Users\crecy\.ssh


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        23/11/2023     14:22           3381 id_rsa
-a----        23/11/2023     14:22            739 id_rsa.pub
-a----        23/11/2023     15:12           4818 known_hosts
-a----        23/10/2023     16:29           2624 known_hosts.old

```

### 🌞 Déposer la clé publique sur une VM

Refait avec Router puisque déjà fait avec John pendant le cours
```
crecy@Thomas MINGW64 ~
$ ssh-copy-id toto@10.7.1.254
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/c/Users/crecy/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
toto@10.7.1.254's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'toto@10.7.1.254'"
and check to make sure that only the key(s) you wanted were added.
```

### 🌞 Connectez-vous en SSH à la machine

```
PS C:\Users\crecy> ssh toto@10.7.1.254
Last login: Thu Nov 23 15:33:13 2023 from 10.7.1.10
[toto@router ~]$
```

## C. Changement de fingerprint

### 🌞 Supprimer les clés sur la machine router.tp7.b1

```
[toto@router ~]$ ls /etc/ssh/
moduli  ssh_config  ssh_config.d  sshd_config  sshd_config.d
```

### 🌞 Regénérez les clés sur la machine router.tp7.b1

```
[toto@router ~]$ sudo ssh-keygen -A
ssh-keygen: generating new host keys: RSA DSA ECDSA ED25519
```

### 🌞 Tentez une nouvelle connexion au serveur

```
PS C:\Users\crecy> ssh toto@10.7.1.254
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:7waar9t+eF7xCHY9PLj0rzs7KY5BdsPQxaFnAxIYG/w.
Please contact your system administrator.
Add correct host key in C:\\Users\\crecy/.ssh/known_hosts to get rid of this message.
Offending ED25519 key in C:\\Users\\crecy/.ssh/known_hosts:27
Host key for 10.7.1.254 has changed and you have requested strict checking.
Host key verification failed.
PS C:\Users\crecy> ssh toto@10.7.1.254
The authenticity of host '10.7.1.254 (10.7.1.254)' can't be established.
ED25519 key fingerprint is SHA256:7waar9t+eF7xCHY9PLj0rzs7KY5BdsPQxaFnAxIYG/w.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.7.1.254' (ED25519) to the list of known hosts.
Last login: Thu Nov 23 15:44:11 2023 from 10.7.1.10
```

## III. Web sécurisé

### 0. Setup

### 🌞 Montrer sur quel port est disponible le serveur web

```
[toto@web ~]$ ss -antp
State    Recv-Q   Send-Q     Local Address:Port       Peer Address:Port    Process
LISTEN   0        511              0.0.0.0:80              0.0.0.0:*
LISTEN   0        128              0.0.0.0:22              0.0.0.0:*
ESTAB    0        52             10.7.1.12:22            10.7.1.10:54784
LISTEN   0        511                 [::]:80                 [::]:*
LISTEN   0        128                 [::]:22                 [::]:*
```

## 1. Setup HTTPS

### 🌞 Générer une clé et un certificat sur web.tp7.b1

```
[toto@web ~]$ openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout server.key -out server.crt
......+.+...+...............+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.+.....+.+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*......+.+.....+...+.......+........................+.....+......+...+...+.............+..+...+.......+.....+....+...+...........+.+........+......+.+......+.....+.......+...+...+..+.......+...+..+.+......+...+.....+......+.......+..+...+....+.....+..........+...........+...+.........+......+.+.....+......+.......+...+...+......+.....+...+...+.............+.....+......+...+......+...............+.+.....+.+...........................+..+...+.......+...............+......+...+..+..........+.........+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
..+....+.....+...+..........+...+..+...+...+......+.+..............+.+...+.....+....+..+...+..........+..+.+...+.........+..+......+.........+....+..+....+......+..+.+.....+.+...+..+..........+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.................+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.....+...........+.+...+...........+.........+.+...+......+.........+.....+.+..+....+...+.....+......+.+.........+.....+....+............+.....+.+............+.....+...+......................+...+..+...+............+.............+.....+.............+.....+.+........+.+.................+.......+......+..+............+...+.........+......+...+....+...+.....+...+.........+.+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:
State or Province Name (full name) []:
Locality Name (eg, city) [Default City]:
Organization Name (eg, company) [Default Company Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (eg, your name or your server's hostname) []:
Email Address []:
[toto@web ~]$
[toto@web ~]$ sudo mv server.key /etc/pki/tls/private/web.tp7.b1.key
[toto@web ~]$ sudo mv server.crt /etc/pki/tls/private/web.tp7.b1.crt
[toto@web ~]$ sudo chown nginx:nginx /etc/pki/tls/private/web.tp7.b1.key
[toto@web ~]$ sudo chown nginx:nginx /etc/pki/tls/certs/web.tp7.b1.crt
chown: cannot access '/etc/pki/tls/certs/web.tp7.b1.crt': No such file or directory
[toto@web ~]$ sudo mv server.crt /etc/pki/tls/private/web.tp7.b1.crt
mv: cannot stat 'server.crt': No such file or directory
[toto@web ~]$ sudo mv /etc/pki/tls/private/web.tp7.b1.crt /etc/pki/tls/certs/web.tp7.
b1.crt
[toto@web ~]$ sudo chown nginx:nginx /etc/pki/tls/certs/web.tp7.b1.crt
[toto@web ~]$ sudo chmod 0400 /etc/pki/tls/private/web.tp7.b1.key
[toto@web ~]$ sudo chmod 0444 /etc/pki/tls/certs/web.tp7.b1.crt
[toto@web ~]$
```

### 🌞 Modification de la conf de NGINX

```
[toto@web ~]$ sudo cat /etc/nginx/conf.d/site_web_nul.conf
server {
    # on change la ligne listen
    listen 10.7.1.12:443 ssl;

    # et on ajoute deux nouvelles lignes
    ssl_certificate /etc/pki/tls/certs/web.tp7.b1.crt;
    ssl_certificate_key /etc/pki/tls/private/web.tp7.b1.key;

    server_name www.site_web_nul.b1;
    root /var/www/site_web_nul

}
```

### 🌞 Conf firewall

```
[toto@web ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 443/tcp 80/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

### 🌞 Redémarrez NGINX

```
[toto@web ~]$ sudo systemctl restart nginx
[toto@web ~]$ systemctl status nginx.service
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset: disabled)
     Active: active (running) since Thu 2023-11-23 16:24:25 CET; 6s ago
    Process: 1413 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
    Process: 1414 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
    Process: 1415 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
   Main PID: 1416 (nginx)
      Tasks: 2 (limit: 4674)
     Memory: 2.3M
        CPU: 55ms
     CGroup: /system.slice/nginx.service
             ├─1416 "nginx: master process /usr/sbin/nginx"
             └─1417 "nginx: worker process"

Nov 23 16:24:24 web.tp7.b1 systemd[1]: Starting The nginx HTTP and reverse proxy server...
Nov 23 16:24:25 web.tp7.b1 nginx[1414]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Nov 23 16:24:25 web.tp7.b1 nginx[1414]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Nov 23 16:24:25 web.tp7.b1 systemd[1]: Started The nginx HTTP and reverse proxy server.
```

### 🌞 Prouvez que NGINX écoute sur le port 443/tcp

```
[toto@web ~]$ ss -antp
State       Recv-Q      Send-Q           Local Address:Port            Peer Address:Port       Process
LISTEN      0           511                    0.0.0.0:80                   0.0.0.0:*
LISTEN      0           511                  10.7.1.12:443                  0.0.0.0:*
LISTEN      0           128                    0.0.0.0:22                   0.0.0.0:*
ESTAB       0           52                   10.7.1.12:22                 10.7.1.10:54784
LISTEN      0           511                       [::]:80                      [::]:*
LISTEN      0           128                       [::]:22                      [::]:*
```

### 🌞 Visitez le site web en https

```
[toto@john ~]$ curl -k https://10.7.1.12
<h1>MEOW</h1>
```