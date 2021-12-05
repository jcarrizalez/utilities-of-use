## STATUS DEL EQUIPO 
3 flashes: start.elf no encontrado
4 flashes: start.elf no se puede iniciar, por lo que probablemente esté dañado. Alternativamente, la tarjeta no está insertada correctamente o la ranura de la tarjeta no funciona.
7 flashes: kernel.img no encontrado
8 flashes: SDRAM no reconocida. En este caso, su SDRAM probablemente está dañada, o bootcode.bin orstart.elf no se puede leer.

## PASO 1 - Descargamos la imagen a usar

[WEB_RASPBERRYPI](https://www.raspberrypi.com/software/operating-systems/#raspberry-pi-os-32-bit)

Raspberry Pi OS Lite
Fecha de lanzamiento: 30 de octubre de 2021
Versión de Kernel: 5.10
Tamaño: 463 MB

## PASO 2 – Nos hacemos root
```
su
```
## PASO 3 – Listamos los dispositivos y particiones con fdisk -l
```
fdisk -l
```
```
Disco /dev/sdb: 7,3 GiB, 7780433920 bytes, 15196160 sectores

Unidades: sectores de 1 * 512 = 512 bytes
Tamaño de sector (lógico/físico): 512 bytes / 512 bytes
Tamaño de E/S (mínimo/óptimo): 512 bytes / 512 bytes
Tipo de etiqueta de disco: dos
Identificador del disco: 0x46e1cc6c

Device Boot Start End Sectors Size Id Type
/dev/sdb1 128 15196159 15196032 7,3G b W95 FAT32
```
## PASO 4 – Desmontamos el pendrive
```
umount /dev/sdb1
```
## PASO 5 – Formateamos el pendrive en FAT32
Con el comando mkfs.vfat elegimos el formato FAT y con -F 32 el tipo de formato, si quisiéramos FAT16 sería -F 16.
```
mkfs.vfat -F 32 /dev/sdb -I
```
## PASO 6 – Pasamos la imagen al pendrive
```
#apt-get install pv #si no esta instalado en tu pc
dd if=2021-10-30-raspios-bullseye-armhf-lite.img | pv -s 1960M | dd of=/dev/sdb
```
Un poco de paciencia y esperamos a que finalice la operación y listo, ya tenemos nuestro USB arrancable preparado para funcionar.

## PASO 7
via term, con teclado, hacemos login
user:pi
password:raspberry
   * Cambiar password de usuario Pi (recomendado)
## PASO 8
```
nano /etc/default/keyboard;
XKBLAYOUT="latam"
```

```
nano /etc/wpa_supplicant/wpa_supplicant/wpa_supplicant.conf;
```
```
network={
        ssid="VALENTINA 2.4GHz"
        scan_ssid=1
        key_mgmt=WPA-PSK
        psk="MIPASSWORD"
}
```
```
killall wpa_supplicant;
wpa_supplicant -B -c /etc/wpa_supplicant/wpa_supplicant.conf -iwlan0;
```
```
systemctl enable ssh;
reboot;
```
```
auto wlan0
#iface wlan0 inet static
iface wlan0 inet dhcp
                wpa-ssid VALENTINA 2.4GHz
                wpa-psk MIPASSWORD
#wireless-mode managed
#wireless-essid "VALENTINA 2.4GHz"
#wireless-key0 "MIPASSWORD"
#address 192.168.0.100
#netmask 255.255.255.0
#gateway 192.168.0.1
#dnsnameservers 8.8.8.8 8.8.4.4
```
## PASO 9. Instalar paquetes necesarios

```
sudo apt-get install -y \
     aptitude \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common \
     fail2ban \
     ntfs-3g
```


### 10. Instalar firmas GPG del repo de Docker

```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
```

### 11. Agregar repo de Docker

```
sudo echo "deb [arch=armhf] https://download.docker.com/linux/debian \
     $(lsb_release -cs) stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list
```

### 12. Instalar Docker

```
sudo apt-get update && sudo apt-get install -y docker-ce docker-compose
sudo systemctl enable docker && sudo systemctl start docker

```

### 13. Agregar usuario al grupo docker y desloguearse y volverse a loguear

```
sudo usermod -a -G docker pi
#(logout and login)
```

### 14. Crear docker-compose
```
nano docker-compose.yml;
```
```
## docker-compose para correr rtorrent sobre raspberry

version: "2"

services:

  samba:
    image: dperson/samba:rpi
    restart: always
    command: '-u "pi;password" -s "media;/media;yes;no" -s "downloads;/downloads;yes;no"'
    stdin_open: true
    tty: true
    ports:
      - 139:130
      - 445:445
    volumes:
      - /usr/share/zoneinfo/America/Argentina/Mendoza:/etc/localtime
      - /home/pi/media:/media
      - /home/pi/downloads:/downloads

  rtorrent:
    image: pablokbs/rutorrent-armhf
    ports:
      - 80:80
      - 51413:51413
      - 6881:6881/udp
    volumes:
      - /home/pi/torrents-config/rtorrent:/config/rtorrent
      - /home/pi/downloads:/downloads
    restart: always
```


### 15. Iniciar docker-compose
```
docker-compose up -d
```
