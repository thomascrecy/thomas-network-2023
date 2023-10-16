# TP 1 Réseau

## I. Exploration locale en solo

### 1. Affichage d'informations sur la pile TCP/IP locale

🌞 Affichez les infos des cartes réseau de votre PC :

```
PS C:\Users\crecy\Documents\Dev> ipconfig /all

Carte réseau sans fil Wi-Fi : 🌞

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Realtek 8852CE WiFi 6E PCI-E NIC
   Adresse physique . . . . . . . . . . . : 40-1A-58-4B-44-84 🌞
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::bad5:94d9:fc9e:ddbb%14(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.48.154(préféré) 🌞
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : mercredi 11 octobre 2023 13:24:02
   Bail expirant. . . . . . . . . . . . . : jeudi 12 octobre 2023 13:23:46
   Passerelle par défaut. . . . . . . . . : 10.33.51.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.51.254
   IAID DHCPv6 . . . . . . . . . . . : 121641560
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2C-41-53-FB-BC-0F-F3-D6-21-FF
   Serveurs DNS. . .  . . . . . . . . . . : 10.33.10.2
                                       8.8.8.8
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé

Carte Ethernet Connexion réseau Bluetooth : 🌞

   Statut du média. . . . . . . . . . . . : Média déconnecté
   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Bluetooth Device (Personal Area Network)
   Adresse physique . . . . . . . . . . . : 40-1A-58-4B-44-85 🌞
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
```
🌞 Affichez votre gateway

```
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Realtek 8852CE WiFi 6E PCI-E NIC
   Adresse physique . . . . . . . . . . . : 40-1A-58-4B-44-84
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::bad5:94d9:fc9e:ddbb%14(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.48.154(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : mercredi 11 octobre 2023 13:24:02
   Bail expirant. . . . . . . . . . . . . : jeudi 12 octobre 2023 13:23:46
   Passerelle par défaut. . . . . . . . . : 10.33.51.254 🌞
   Serveur DHCP . . . . . . . . . . . . . : 10.33.51.254
   IAID DHCPv6 . . . . . . . . . . . : 121641560
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2C-41-53-FB-BC-0F-F3-D6-21-FF
   Serveurs DNS. . .  . . . . . . . . . . : 10.33.10.2
                                       8.8.8.8
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```
🌞 Déterminer la MAC de la passerelle
```
PS C:\Users\crecy\Documents\Dev> arp /a

Interface : 10.33.48.154 --- 0xe
  Adresse Internet      Adresse physique      Type
  10.33.48.14           e4-b3-18-48-36-68     dynamique
  10.33.51.254          7c-5a-1c-cb-fd-a4     dynamique 🌞
  10.33.51.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  224.0.1.60            01-00-5e-00-01-3c     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  255.255.255.255       ff-ff-ff-ff-ff-ff     statique

Interface : 192.168.56.1 --- 0x37
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  224.0.0.252           01-00-5e-00-00-fc     statique
  224.0.1.60            01-00-5e-00-01-3c     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```
🌞 Trouvez comment afficher les informations sur une carte IP (change selon l'OS)  

Panneau de configuration\Réseau et Internet\Connexions réseau

## 2. Modifications des informations
A. Modification d'adresse IP (part 1)  

🌞 Utilisez l'interface graphique de votre OS pour changer d'adresse IP :  
Propriétés - Protocole internet version 4 - Propriétés - Utiliser l'adresse IP suivante

🌞 Il est possible que vous perdiez l'accès internet.

## II. Exploration locale en duo

🌞 Modifiez l'IP des deux machines pour qu'elles soient dans le même réseau  
IP : 10.10.10.99  
Masque : 255.255.255.0

🌞 Vérifier à l'aide d'une commande que votre IP a bien été changée

```
PS C:\Users\crecy\Documents\Dev> ipconfig /all

Carte Ethernet Ethernet :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Realtek Gaming GbE Family Controller
   Adresse physique . . . . . . . . . . . : BC-0F-F3-D6-21-FF
   DHCP activé. . . . . . . . . . . . . . : Non
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::2e09:3206:17de:a638%9(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.10.10.99(préféré) 🌞
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :
   IAID DHCPv6 . . . . . . . . . . . : 129765363
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2C-41-53-FB-BC-0F-F3-D6-21-FF
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
   ```

🌞 Vérifier que les deux machines se joignent
```
PS C:\Users\crecy\Documents\Dev> ping 10.10.10.12

Envoi d’une requête 'Ping'  10.10.10.12 avec 32 octets de données :
Réponse de 10.10.10.12 : octets=32 temps=3 ms TTL=128
Réponse de 10.10.10.12 : octets=32 temps=7 ms TTL=128
Réponse de 10.10.10.12 : octets=32 temps=1 ms TTL=128
Réponse de 10.10.10.12 : octets=32 temps=4 ms TTL=128

Statistiques Ping pour 10.10.10.12:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 1ms, Maximum = 7ms, Moyenne = 3ms
```
🌞 Déterminer l'adresse MAC de votre correspondant
```
Interface : 10.10.10.99 --- 0x9
  Adresse Internet      Adresse physique      Type
  10.10.10.12           bc-ec-a0-11-ad-7b     dynamique 🌞
  10.10.10.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  224.0.0.251           01-00-5e-00-00-fb     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
```
🌞 sur le PC serveur
```
PS C:\Users\crecy\Desktop\netcat-1.11> .\nc.exe -l -p 8888
kkkk
ca va ou quoi
vkjdshjhgsgsg
```
🌞 Visualiser la connexion en cours
```
PS C:\Users\crecy\Desktop\netcat-1.11> netstat -a -n -b | Select-String "8888"

  TCP    10.10.10.99:8888       10.10.10.12:50357      ESTABLISHED
```
🌞 Pour aller un peu plus loin  
Sur le pc client

```
PS C:\Users\yanic> netstat -a -n -b | select-string 8888

    TCP    10.10.10.12:51946      10.10.10.99:8888       ESTABLISHED


    PS C:\Users\yanic> netstat -a -n -b | select-string 8888 -context 0,1

  TCP    10.10.10.12:51946      10.10.10.99:8888       ESTABLISHED
    [nc.exe]
```
🌞 Activez et configurez votre firewall  
Création d'une règle qui s'applique à tous les programmes, protocole type ICMPv4, autorise la connexion de toutes les adresses IP. Permet de recevoir des pings, même avec le pare feu activé.

## 6. Utilisation d'un des deux comme gateway
🌞Tester l'accès internet  
```
PS C:\Users\crecy> ipconfig

Carte Ethernet Ethernet :

   Suffixe DNS propre à la connexion. . . :
   Adresse IPv6 de liaison locale. . . . .: fe80::2e09:3206:17de:a638%9
   Adresse IPv4. . . . . . . . . . . . . .: 192.168.137.1
   Masque de sous-réseau. . . . . . . . . : 255.255.255.0
   Passerelle par défaut. . . . . . . . . :
```
🌞 Prouver que la connexion Internet passe bien par l'autre PC
```
PS C:\Users\crecy> tracert 192.168.137.12

Détermination de l’itinéraire vers 192.168.137.12 avec un maximum de 30 sauts.

  1     8 ms     3 ms     2 ms  192.168.137.12

Itinéraire déterminé.
```

## III. Manipulations d'autres outils/protocoles côté client

### 1. DHCP

🌞Exploration du DHCP, depuis votre PC
```
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Realtek 8852CE WiFi 6E PCI-E NIC
   Adresse physique . . . . . . . . . . . : 40-1A-58-4B-44-84
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::bad5:94d9:fc9e:ddbb%14(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.48.154(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : mercredi 11 octobre 2023 15:10:29
   Bail expirant. . . . . . . . . . . . . : jeudi 12 octobre 2023 15:10:29 🌞
   Passerelle par défaut. . . . . . . . . : 10.33.51.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.51.254 🌞
   IAID DHCPv6 . . . . . . . . . . . : 121641560
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2C-41-53-FB-BC-0F-F3-D6-21-FF
   Serveurs DNS. . .  . . . . . . . . . . : 10.33.10.2
                                       8.8.8.8
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```

### 2. DNS

🌞** Trouver l'adresse IP du serveur DNS que connaît votre ordinateur**
```
Carte réseau sans fil Wi-Fi :

   Suffixe DNS propre à la connexion. . . :
   Description. . . . . . . . . . . . . . : Realtek 8852CE WiFi 6E PCI-E NIC
   Adresse physique . . . . . . . . . . . : 40-1A-58-4B-44-84
   DHCP activé. . . . . . . . . . . . . . : Oui
   Configuration automatique activée. . . : Oui
   Adresse IPv6 de liaison locale. . . . .: fe80::bad5:94d9:fc9e:ddbb%14(préféré)
   Adresse IPv4. . . . . . . . . . . . . .: 10.33.48.154(préféré)
   Masque de sous-réseau. . . . . . . . . : 255.255.252.0
   Bail obtenu. . . . . . . . . . . . . . : mercredi 11 octobre 2023 15:10:29
   Bail expirant. . . . . . . . . . . . . : jeudi 12 octobre 2023 15:10:29 
   Passerelle par défaut. . . . . . . . . : 10.33.51.254
   Serveur DHCP . . . . . . . . . . . . . : 10.33.51.254 
   IAID DHCPv6 . . . . . . . . . . . : 121641560
   DUID de client DHCPv6. . . . . . . . : 00-01-00-01-2C-41-53-FB-BC-0F-F3-D6-21-FF
   Serveurs DNS. . .  . . . . . . . . . . : 10.33.10.2 🌞
                                       8.8.8.8
   NetBIOS sur Tcpip. . . . . . . . . . . : Activé
```
🌞 Utiliser, en ligne de commande l'outil nslookup (Windows, MacOS) ou dig (GNU/Linux, MacOS) pour faire des requêtes DNS à la main
```
PS C:\Users\crecy> nslookup google.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    google.com
Addresses:  2a00:1450:4007:819::200e
          142.250.178.142
```
```
PS C:\Users\crecy> nslookup ynov.com
Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::681a:be9
          2606:4700:20::681a:ae9
          2606:4700:20::ac43:4ae2
          172.67.74.226
          104.26.11.233
          104.26.10.233
```
```
PS C:\Users\crecy> nslookup 231.34.113.12
Serveur :   dns.google
Address:  8.8.8.8

*** dns.google ne parvient pas à trouver 231.34.113.12 : Non-existent domain
```
```
PS C:\Users\crecy> nslookup 78.34.2.17
Serveur :   dns.google
Address:  8.8.8.8

Nom :    cable-78-34-2-17.nc.de
Address:  78.34.2.17
```
## IV. Wireshark
🌞 Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence :
