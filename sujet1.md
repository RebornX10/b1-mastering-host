# MASTERING HOST
--- 
## Self-footprinting

### Host OS
OS/Version/Architecture/Nom_machine
```bash
parrot% uname -r -s -v -i -o
Linux 5.4.0-3parrot1-amd64 SMP Parrot 5.4.13-3parrot2 (2020-02-01) unknown GNU/Linux
parrot% hostname
parrot
```
Modele du CPU/Quantité RAM/Model RAM
```bash
parrot% sudo lshw
     *-memory
          description: System Memory
          physical id: 8
          slot: System board or motherboard
          size: 8GiB
        *-bank:0             #SLOT DE RAM 0
             description: SODIMM DDR4 Synchronous Unbuffered (Unregistered) 2400 MHz (0.4 ns)
             product: 8ATF1G64HZ-2G3H1
             vendor: Micron
             physical id: 0
             serial: 00000000
             slot: ChannelA-DIMM0
             size: 8GiB
             width: 64 bits
             clock: 2400MHz (0.4ns)
        *-bank:1            #SLOT DE RAM 1
             description: [empty]
             physical id: 1
             slot: ChannelA-DIMM1
     *-cpu
          description: CPU
          product: Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
```

### Devices
here we go again?
Proc: 
```bash
sudo lshw -short | grep processor
/0/12                     processor      Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
```
 - il s'agit d'un processeur i7, c'est a dire le moyen-haut de gamme pour consommateur/particulier
 - 8550U
     - 8 => huitieme generation
     - 550 modele
     - U => processeur basse consommation pour laptop
     - Micro-architecture: Whiskey Lake
```bash

parrot%
[...]
     *-cpu
          description: CPU
          product: Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
          vendor: Intel Corp.
          physical id: 12
          bus info: cpu@0
          version: Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
          serial: To Be Filled By O.E.M.
          slot: U3E1
          size: 3197MHz
          capacity: 4005MHz
          width: 64 bits
          clock: 100MHz
            [...]
            [...]
            [...]
          configuration: cores=4 enabledcores=4 threads=8
parrot%lshw -short 

H/W path            Device      Class       Description
=======================================================
/0/100/1f.3               multimedia     Sunrise Point-LP HD Audio    # ENCEINTE
/0/100/17/0    /dev/sda   disk           128GB SanDisk SD8SN8U1       # DISK PRINCIPALE

```

DiSKsss dure

```bash
parrot% lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 119.2G  0 disk
├─sda1   8:1    0    16M  0 part
├─sda2   8:2    0  85.8G  0 part            # WINDOWS
├─sda3   8:3    0     1G  0 part
├─sda4   8:4    0   523M  0 part
├─sda5   8:5    0   200M  0 part /boot/efi  # motherboard <=> OS
├─sda6   8:6    0   366M  0 part /boot      # all needed files for boot are in this directory
└─sda7   8:7    0  31.4G  0 part /          # root
sdb      8:16   0 931.5G  0 disk
├─sdb1   8:17   0   499M  0 part
├─sdb2   8:18   0   100M  0 part
├─sdb3   8:19   0    16M  0 part
├─sdb4   8:20   0 243.6G  0 part            # WINDOWS
└─sdb5   8:21   0 118.2G  0 part /disk2     # just a 2nd disk
parrot%                                        
```

 - `sda` est le disque dur principale ou toutes les docs du user seront.
 - `sdb` disk 2

### Network

```bash
parrot% ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether b4:6b:fc:9f:07:5e brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.19/24 brd 192.168.1.255 scope global dynamic noprefixroute wlan0
       valid_lft 81871sec preferred_lft 81871sec
    inet6 2a01:cb19:675:df00:cf2b:d7d4:225b:51f8/64 scope global dynamic noprefixroute
       valid_lft 1780sec preferred_lft 580sec
    inet6 fe80::d366:334e:93c4:b7e1/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: vboxnet0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 0a:00:27:00:00:00 brd ff:ff:ff:ff:ff:ff
4: vboxnet1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 0a:00:27:00:00:01 brd ff:ff:ff:ff:ff:ff
```
- ``vboxnet0`` Host-only de Vbox
- ``wlan0`` acces wifi au router
- ``vboxnet1`` Host-only de Vbox
- ``lo`` loopback/localhost

Port
```bash
parrot% ss -lntu
Netid     State       Recv-Q      Send-Q                               Local Address:Port           Peer Address:Port     Process
udp       UNCONN      0           0                                          0.0.0.0:4500                0.0.0.0:*
udp       UNCONN      0           0                                          0.0.0.0:500                 0.0.0.0:*
udp       UNCONN      0           0                                                *:4500                      *:*
udp       UNCONN      0           0                                                *:500                       *:*
udp       UNCONN      0           0                [fe80::d366:334e:93c4:b7e1]%wlan0:546                    [::]:*
tcp       LISTEN      0           128                                        0.0.0.0:22                  0.0.0.0:*
tcp       LISTEN      0           5                                        127.0.0.1:631                 0.0.0.0:*
tcp       LISTEN      0           511                                      127.0.0.1:6463                0.0.0.0:*
tcp       LISTEN      0           128                                           [::]:22                     [::]:*
tcp       LISTEN      0           5                                            [::1]:631                    [::]:*        
```
C'est l'heure de lister les ports!:
     - 500 IKE internet key exchange
     - 22 = SSH bah pour ssh-er
     - 631 = Internet Printing Protocol
     - 6463 = Discord RPC
     - 546 = DHCPv6
     
### USERS
```bash
parrot%    cut -d: -f1 /etc/passwd
root
daemon
bin
sys
sync
games
man
lp
mail
news
uucp
proxy
www-data
backup
list
irc
gnats
nobody
systemd-timesync
systemd-network
systemd-resolve
_apt
mysql
shellinabox
strongswan
ntp
messagebus
arpwatch
Debian-exim
uuidd
[...]
[...]
dnsmasq
sslh
redis
postgres
usbmux
rtkit
inetsim
sshd
nm-openvpn
nm-openconnect
pulse
avahi
[...]
dradis
beef-xss
geoclue
lightdm
king-phisher
sam
systemd-coredump
tcpdump
Debian-gdm
morty
```

les USERs ici sont ``root`` (full admin) et ``user`` (moi) (et tout le reste bien sur)

### Processes

```bash
parrot% top
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
user      1050  0.0  0.4  21380  9268 ?        Ss   16:37   0:00 /lib/systemd/systemd --user
root       453  0.0  0.3  19556  7360 ?        Ss   16:36   0:00 /lib/systemd/systemd-logind
root       452  0.0  0.3 242076  8092 ?        Ssl  16:36   0:00 /usr/lib/accountsservice/accounts-daemon
root        40  0.0  0.0      0     0 ?        S    16:36   0:00 [watchdogd]
```
tout les processus root sont lancés par le root
et 

--- 
## Scripting
