# debian-mediacentre
My mediacentre configuration for Debian

## Add deb-multimedia repositories
deb http://mirror.optus.net/deb-multimedia/ stable main
deb-src http://mirror.optus.net/deb-multimedia/ stable main

deb ftp://mirror.optus.net/deb-multimedia/ stable main
deb-src ftp://mirror.optus.net/deb-multimedia/ stable main

#### This information was retrieved from https://www.linuxserver.io/2017/06/24/the-perfect-media-server-2017/
## Install Network File Sharing
apt install samba nfs-kernel-server
## Edit /etc/samba/smb.conf and configure it to needs with the following as a template
[global]
  workgroup = KTZ
  server string = epsilon
  security = user
  guest ok = yes
  map to guest = Bad Password

  log file = /var/log/samba/%m.log
  max log size = 50
  printcap name = /dev/null
  load printers = no

# Samba Shares
[home]
  comment = alex home folder
  path = /home/alex
  browseable = yes
  read only = no
  guest ok = no

[opt]
  comment = opt directory
  path = /opt
  browseable = yes
  read only = no
  guest ok = yes

[storage]
  comment = Storage on epsilon
  path = /mnt/storage
  browseable = yes
  read only = no
  guest ok = yes
  
## restart samba
systemctl restart smbd
  
## Edit /etc/exports and use the following as a template
/mnt/storage/movies       *(ro,sync,no_root_squash)
/mnt/storage/tv           *(ro,sync,no_root_squash)

## Run the following commands
exportfs -ra
systemctl restart nfs-kernel-server
