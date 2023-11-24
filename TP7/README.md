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

# IV. VPN

### 🌞 Installer le serveur Wireguard sur vpn.tp7.b1

```
[toto@vpn ~]$ sudo modprobe wireguard
[sudo] password for toto:
[toto@vpn ~]$ echo wireguard | sudo tee /etc/modules-load.d/wireguard.conf
wireguard
[toto@vpn ~]$ sudo nano /etc/sysctl.conf
[toto@vpn ~]$ sudo sysctl -p
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1
[toto@vpn ~]$ sudo dnf install wireguard-tools -y
Rocky Linux 9 - BaseOS                                        8.4 kB/s | 4.1 kB     00:00
Rocky Linux 9 - BaseOS                                        2.4 MB/s | 2.2 MB     00:00
Rocky Linux 9 - AppStream                                      13 kB/s | 4.5 kB     00:00
Rocky Linux 9 - AppStream                                     5.2 MB/s | 7.4 MB     00:01
Rocky Linux 9 - Extras                                        4.5 kB/s | 2.9 kB     00:00
Rocky Linux 9 - Extras                                         19 kB/s |  13 kB     00:00
Dependencies resolved.
==============================================================================================
 Package                    Architecture   Version                    Repository         Size
==============================================================================================
Installing:
 wireguard-tools            x86_64         1.0.20210914-2.el9         appstream         115 k
Upgrading:
 systemd                    x86_64         252-18.el9                 baseos            3.9 M
 systemd-libs               x86_64         252-18.el9                 baseos            652 k
 systemd-pam                x86_64         252-18.el9                 baseos            261 k
 systemd-rpm-macros         noarch         252-18.el9                 baseos             50 k
 systemd-udev               x86_64         252-18.el9                 baseos            1.8 M
Installing dependencies:
 systemd-resolved           x86_64         252-18.el9                 baseos            361 k

Transaction Summary
==============================================================================================
Install  2 Packages
Upgrade  5 Packages

Total download size: 7.1 M
Downloading Packages:
(1/7): wireguard-tools-1.0.20210914-2.el9.x86_64.rpm          782 kB/s | 115 kB     00:00
(2/7): systemd-rpm-macros-252-18.el9.noarch.rpm               761 kB/s |  50 kB     00:00
(3/7): systemd-pam-252-18.el9.x86_64.rpm                      3.0 MB/s | 261 kB     00:00
(4/7): systemd-resolved-252-18.el9.x86_64.rpm                 1.1 MB/s | 361 kB     00:00
(5/7): systemd-libs-252-18.el9.x86_64.rpm                     3.3 MB/s | 652 kB     00:00
(6/7): systemd-udev-252-18.el9.x86_64.rpm                     3.3 MB/s | 1.8 MB     00:00
(7/7): systemd-252-18.el9.x86_64.rpm                          5.5 MB/s | 3.9 MB     00:00
----------------------------------------------------------------------------------------------
Total                                                         4.7 MB/s | 7.1 MB     00:01
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                      1/1
  Upgrading        : systemd-libs-252-18.el9.x86_64                                      1/12
  Running scriptlet: systemd-libs-252-18.el9.x86_64                                      1/12
  Upgrading        : systemd-rpm-macros-252-18.el9.noarch                                2/12
  Upgrading        : systemd-pam-252-18.el9.x86_64                                       3/12
  Running scriptlet: systemd-252-18.el9.x86_64                                           4/12
  Upgrading        : systemd-252-18.el9.x86_64                                           4/12
  Running scriptlet: systemd-252-18.el9.x86_64                                           4/12
  Running scriptlet: systemd-resolved-252-18.el9.x86_64                                  5/12
  Installing       : systemd-resolved-252-18.el9.x86_64                                  5/12
  Running scriptlet: systemd-resolved-252-18.el9.x86_64                                  5/12
  Installing       : wireguard-tools-1.0.20210914-2.el9.x86_64                           6/12
  Upgrading        : systemd-udev-252-18.el9.x86_64                                      7/12
  Running scriptlet: systemd-udev-252-18.el9.x86_64                                      7/12
  Running scriptlet: systemd-udev-252-13.el9_2.x86_64                                    8/12
  Cleanup          : systemd-udev-252-13.el9_2.x86_64                                    8/12
  Running scriptlet: systemd-udev-252-13.el9_2.x86_64                                    8/12
  Cleanup          : systemd-pam-252-13.el9_2.x86_64                                     9/12
  Cleanup          : systemd-252-13.el9_2.x86_64                                        10/12
  Running scriptlet: systemd-252-13.el9_2.x86_64                                        10/12
  Cleanup          : systemd-rpm-macros-252-13.el9_2.noarch                             11/12
  Cleanup          : systemd-libs-252-13.el9_2.x86_64                                   12/12
  Running scriptlet: systemd-libs-252-13.el9_2.x86_64                                   12/12
  Verifying        : systemd-resolved-252-18.el9.x86_64                                  1/12
  Verifying        : wireguard-tools-1.0.20210914-2.el9.x86_64                           2/12
  Verifying        : systemd-udev-252-18.el9.x86_64                                      3/12
  Verifying        : systemd-udev-252-13.el9_2.x86_64                                    4/12
  Verifying        : systemd-rpm-macros-252-18.el9.noarch                                5/12
  Verifying        : systemd-rpm-macros-252-13.el9_2.noarch                              6/12
  Verifying        : systemd-pam-252-18.el9.x86_64                                       7/12
  Verifying        : systemd-pam-252-13.el9_2.x86_64                                     8/12
  Verifying        : systemd-libs-252-18.el9.x86_64                                      9/12
  Verifying        : systemd-libs-252-13.el9_2.x86_64                                   10/12
  Verifying        : systemd-252-18.el9.x86_64                                          11/12
  Verifying        : systemd-252-13.el9_2.x86_64                                        12/12

Upgraded:
  systemd-252-18.el9.x86_64                  systemd-libs-252-18.el9.x86_64
  systemd-pam-252-18.el9.x86_64              systemd-rpm-macros-252-18.el9.noarch
  systemd-udev-252-18.el9.x86_64
Installed:
  systemd-resolved-252-18.el9.x86_64         wireguard-tools-1.0.20210914-2.el9.x86_64

Complete!
```

### 🌞 Générer la paire de clé du serveur sur vpn.tp7.b1

```
[toto@vpn ~]$ wg genkey | sudo tee /etc/wireguard/server.key
mFEoTgryYXvCdNHBUy6TtMvF/jpS4EmMxYmBny6SEkc=
[toto@vpn ~]$ sudo chmod 0400 /etc/wireguard/server.key
[toto@vpn ~]$ sudo cat /etc/wireguard/server.key | wg pubkey | sudo tee /etc/wireguard/server.pub
kMWWrKucJtb44yW8hmz5k30NUns29BCzvoudmqr5OVc=
```

### 🌞 Générer la paire de clé du client sur vpn.tp7.b1

```
[toto@vpn ~]$ sudo mkdir -p /etc/wireguard/clients
[toto@vpn ~]$ wg genkey | sudo tee /etc/wireguard/clients/john.key
uL/suabQfa8oN+s3qrDf/Ey/uXj2BuSX9rAoEIEuzGc=
[toto@vpn ~]$ sudo chmod 0400 /etc/wireguard/server.key
[toto@vpn ~]$ sudo cat /etc/wireguard/clients/john.key | wg pubkey | tee /etc/wireguard/clients/john.pub
tee: /etc/wireguard/clients/john.pub: Permission denied
OEx7FINTjESQG5rYkx8MxrUMO0B7xTmO6D6knYDibXI=
```

### 🌞 Création du fichier de config du serveur sur vpn.tp7.b1

```
[toto@vpn ~]$ sudo cat /etc/wireguard/wg0.conf
[Interface]
Address = 10.107.1.0/24
SaveConfig = false
PostUp = firewall-cmd --zone=public --add-masquerade
PostUp = firewall-cmd --add-interface=wg0 --zone=public
PostDown = firewall-cmd --zone=public --remove-masquerade
PostDown = firewall-cmd --remove-interface=wg0 --zone=public
ListenPort = 13337
PrivateKey = mFEoTgryYXvCdNHBUy6TtMvF/jpS4EmMxYmBny6SEkc=

[Peer]
PublicKey = OEx7FINTjESQG5rYkx8MxrUMO0B7xTmO6D6knYDibXI=
AllowedIPs = 10.107.1.11/32
```

### 🌞 Démarrez le serveur VPN sur vpn.tp7.b1

```
[toto@vpn ~]$ systemctl status wg-quick@wg0.service
● wg-quick@wg0.service - WireGuard via wg-quick(8) for wg0
     Loaded: loaded (/usr/lib/systemd/system/wg-quick@.service; disabled; preset: disabled)
     Active: active (exited) since Fri 2023-11-24 15:03:34 CET; 1min 20s ago
       Docs: man:wg-quick(8)
             man:wg(8)
             https://www.wireguard.com/
             https://www.wireguard.com/quickstart/
             https://git.zx2c4.com/wireguard-tools/about/src/man/wg-quick.8
             https://git.zx2c4.com/wireguard-tools/about/src/man/wg.8
    Process: 8949 ExecStart=/usr/bin/wg-quick up wg0 (code=exited, status=0/SUCCESS)
   Main PID: 8949 (code=exited, status=0/SUCCESS)
        CPU: 920ms

Nov 24 15:03:31 vpn.tp7.b1 systemd[1]: Starting WireGuard via wg-quick(8) for wg0...
Nov 24 15:03:32 vpn.tp7.b1 wg-quick[8949]: [#] ip link add wg0 type wireguard
Nov 24 15:03:32 vpn.tp7.b1 wg-quick[8949]: [#] wg setconf wg0 /dev/fd/63
Nov 24 15:03:32 vpn.tp7.b1 wg-quick[8949]: [#] ip -4 address add 10.107.1.0/24 dev wg0
Nov 24 15:03:32 vpn.tp7.b1 wg-quick[8949]: [#] ip link set mtu 1420 up dev wg0
Nov 24 15:03:32 vpn.tp7.b1 wg-quick[8949]: [#] firewall-cmd --zone=public --add-masquerade
Nov 24 15:03:33 vpn.tp7.b1 wg-quick[8985]: success
Nov 24 15:03:33 vpn.tp7.b1 wg-quick[8949]: [#] firewall-cmd --add-interface=wg0 --zone=public
Nov 24 15:03:34 vpn.tp7.b1 wg-quick[8996]: success
Nov 24 15:03:34 vpn.tp7.b1 systemd[1]: Finished WireGuard via wg-quick(8) for wg0.
```
```
[toto@vpn ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:b7:66:7a brd ff:ff:ff:ff:ff:ff
    inet 10.7.1.101/24 brd 10.7.1.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:feb7:667a/64 scope link
       valid_lft forever preferred_lft forever
6: wg0: <POINTOPOINT,NOARP,UP,LOWER_UP> mtu 1420 qdisc noqueue state UNKNOWN group default qlen 1000
    link/none
    inet 10.107.1.0/24 scope global wg0
       valid_lft forever preferred_lft forever
```
```
[toto@vpn ~]$ sudo wg show
interface: wg0
  public key: kMWWrKucJtb44yW8hmz5k30NUns29BCzvoudmqr5OVc=
  private key: (hidden)
  listening port: 13337

peer: OEx7FINTjESQG5rYkx8MxrUMO0B7xTmO6D6knYDibXI=
  allowed ips: 10.107.1.11/32
```

### 🌞 Ouvrir le bon port firewall

```
[toto@vpn ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 13337/udp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

### 🌞 On installe les outils Wireguard sur john.tp7.b1

```
[toto@john ~]$ sudo dnf install wireguard-tools
[sudo] password for toto:
Rocky Linux 9 - BaseOS                                                          7.2 kB/s | 4.1 kB     00:00
Rocky Linux 9 - AppStream                                                        11 kB/s | 4.5 kB     00:00
Rocky Linux 9 - Extras                                                          7.7 kB/s | 2.9 kB     00:00
Dependencies resolved.
================================================================================================================
 Package                        Architecture       Version                          Repository             Size
================================================================================================================
Installing:
 wireguard-tools                x86_64             1.0.20210914-2.el9               appstream             115 k
Upgrading:
 systemd                        x86_64             252-18.el9                       baseos                3.9 M
 systemd-libs                   x86_64             252-18.el9                       baseos                652 k
 systemd-pam                    x86_64             252-18.el9                       baseos                261 k
 systemd-rpm-macros             noarch             252-18.el9                       baseos                 50 k
 systemd-udev                   x86_64             252-18.el9                       baseos                1.8 M
Installing dependencies:
 systemd-resolved               x86_64             252-18.el9                       baseos                361 k

Transaction Summary
================================================================================================================
Install  2 Packages
Upgrade  5 Packages

Total download size: 7.1 M
Is this ok [y/N]: y
Downloading Packages:
Rocky Linux 9 - BaseOS              195% [======================================================================(1/7): systemd-resolved-252-18.el9.x86_64.rpm                                   1.4 MB/s | 361 kB     00:00
(2/7): wireguard-tools-1.0.20210914-2.el9.x86_64.rpm                            399 kB/s | 115 kB     00:00
(3/7): systemd-rpm-macros-252-18.el9.noarch.rpm                                 659 kB/s |  50 kB     00:00
(4/7): systemd-pam-252-18.el9.x86_64.rpm                                        1.4 MB/s | 261 kB     00:00
(5/7): systemd-libs-252-18.el9.x86_64.rpm                                       3.0 MB/s | 652 kB     00:00
(6/7): systemd-udev-252-18.el9.x86_64.rpm                                       3.0 MB/s | 1.8 MB     00:00
(7/7): systemd-252-18.el9.x86_64.rpm                                            7.2 MB/s | 3.9 MB     00:00
----------------------------------------------------------------------------------------------------------------
Total                                                                           4.2 MB/s | 7.1 MB     00:01
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                        1/1
  Upgrading        : systemd-libs-252-18.el9.x86_64                                                        1/12
  Running scriptlet: systemd-libs-252-18.el9.x86_64                                                        1/12
  Upgrading        : systemd-rpm-macros-252-18.el9.noarch                                                  2/12
  Upgrading        : systemd-pam-252-18.el9.x86_64                                                         3/12
  Running scriptlet: systemd-252-18.el9.x86_64                                                             4/12
  Upgrading        : systemd-252-18.el9.x86_64                                                             4/12
  Running scriptlet: systemd-252-18.el9.x86_64                                                             4/12
  Running scriptlet: systemd-resolved-252-18.el9.x86_64                                                    5/12
  Installing       : systemd-resolved-252-18.el9.x86_64                                                    5/12
  Running scriptlet: systemd-resolved-252-18.el9.x86_64                                                    5/12
  Installing       : wireguard-tools-1.0.20210914-2.el9.x86_64                                             6/12
  Upgrading        : systemd-udev-252-18.el9.x86_64                                                        7/12
  Running scriptlet: systemd-udev-252-18.el9.x86_64                                                        7/12
  Running scriptlet: systemd-udev-252-13.el9_2.x86_64                                                      8/12
  Cleanup          : systemd-udev-252-13.el9_2.x86_64                                                      8/12
  Running scriptlet: systemd-udev-252-13.el9_2.x86_64                                                      8/12
  Cleanup          : systemd-pam-252-13.el9_2.x86_64                                                       9/12
  Cleanup          : systemd-252-13.el9_2.x86_64                                                          10/12
  Running scriptlet: systemd-252-13.el9_2.x86_64                                                          10/12
  Cleanup          : systemd-rpm-macros-252-13.el9_2.noarch                                               11/12
  Cleanup          : systemd-libs-252-13.el9_2.x86_64                                                     12/12
  Running scriptlet: systemd-libs-252-13.el9_2.x86_64                                                     12/12
  Verifying        : systemd-resolved-252-18.el9.x86_64                                                    1/12
  Verifying        : wireguard-tools-1.0.20210914-2.el9.x86_64                                             2/12
  Verifying        : systemd-udev-252-18.el9.x86_64                                                        3/12
  Verifying        : systemd-udev-252-13.el9_2.x86_64                                                      4/12
  Verifying        : systemd-rpm-macros-252-18.el9.noarch                                                  5/12
  Verifying        : systemd-rpm-macros-252-13.el9_2.noarch                                                6/12
  Verifying        : systemd-pam-252-18.el9.x86_64                                                         7/12
  Verifying        : systemd-pam-252-13.el9_2.x86_64                                                       8/12
  Verifying        : systemd-libs-252-18.el9.x86_64                                                        9/12
  Verifying        : systemd-libs-252-13.el9_2.x86_64                                                     10/12
  Verifying        : systemd-252-18.el9.x86_64                                                            11/12
  Verifying        : systemd-252-13.el9_2.x86_64                                                          12/12

Upgraded:
  systemd-252-18.el9.x86_64                systemd-libs-252-18.el9.x86_64     systemd-pam-252-18.el9.x86_64
  systemd-rpm-macros-252-18.el9.noarch     systemd-udev-252-18.el9.x86_64
Installed:
  systemd-resolved-252-18.el9.x86_64                  wireguard-tools-1.0.20210914-2.el9.x86_64

Complete!
```

### 🌞 Supprimez votre route par défaut

```
[toto@john ~]$ curl google.com
curl: (6) Could not resolve host: google.com
[toto@john ~]$ ping google.com
ping: google.com: Name or service not known
```

### 🌞 Configurer un fichier de conf sur john.tp7.b1

```
[Interface]
Address = 10.107.1.11/24
PrivateKey = uL/suabQfa8oN+s3qrDf/Ey/uXj2BuSX9rAoEIEuzGc=

[Peer]
PublicKey = kMWWrKucJtb44yW8hmz5k30NUns29BCzvoudmqr5OVc=
AllowedIPs = 0.0.0.0/0
Endpoint = 10.7.1.101:13337
```

### 🌞 Lancer la connexion VPN

```
[toto@john wireguard]$ wg-quick up ./john.conf
Warning: `/home/toto/wireguard/john.conf' is world accessible
[#] ip link add john type wireguard
[#] wg setconf john /dev/fd/63
[#] ip -4 address add 10.107.1.11/24 dev john
[#] ip link set mtu 1420 up dev john
[#] wg set john fwmark 51820
[#] ip -4 route add 0.0.0.0/0 dev john table 51820
[#] ip -4 rule add not fwmark 51820 table 51820
[#] ip -4 rule add table main suppress_prefixlength 0
[#] sysctl -q net.ipv4.conf.all.src_valid_mark=1
[#] nft -f /dev/fd/63
```

### 🌞 Vérifier la connexion

### Sur John
```
[toto@john ~]$ sudo wg show
interface: john
  public key: OEx7FINTjESQG5rYkx8MxrUMO0B7xTmO6D6knYDibXI=
  private key: (hidden)
  listening port: 39099
  fwmark: 0xca6c

peer: kMWWrKucJtb44yW8hmz5k30NUns29BCzvoudmqr5OVc=
  endpoint: 10.7.1.101:13337
  allowed ips: 0.0.0.0/0
  latest handshake: 45 seconds ago
  transfer: 408 B received, 4.59 KiB sent
```

```
[toto@vpn ~]$ sudo wg show
interface: wg0
  public key: kMWWrKucJtb44yW8hmz5k30NUns29BCzvoudmqr5OVc=
  private key: (hidden)
  listening port: 13337

peer: OEx7FINTjESQG5rYkx8MxrUMO0B7xTmO6D6knYDibXI=
  endpoint: 10.7.1.11:39099
  allowed ips: 10.107.1.11/32
  latest handshake: 1 minute, 7 seconds ago
  transfer: 4.31 KiB received, 500 B sent
```

### ça marche po