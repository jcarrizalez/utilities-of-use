## PASO 1 – Nos hacemos root

```
$ su
```
## PASO 2 – Listamos los dispositivos y particiones con fdisk -l

```
$ fdisk -l
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
## PASO 3 – Desmontamos el pendrive
```
$ umount /dev/sdb1
```
## PASO 4 – Formateamos el pendrive en FAT32
Con el comando mkfs.vfat elegimos el formato FAT y con -F 32 el tipo de formato, si quisiéramos FAT16 sería -F 16.
```
$ mkfs.vfat -F 32 /dev/sdb -I
```
## PASO 5 – Pasamos la imagen al pendrive
```
$ dd if=debian-live-10.8.0-amd64-gnome.iso of=/dev/sdb
```
Un poco de paciencia y esperamos a que finalice la operación y listo, ya tenemos nuestro USB arrancable preparado para funcionar.

## Opción Deluxe
Si queremos que salga una barra de progreso al pasar la ISO al pendrive tendremos que instalar pv y luego ejecutar el comando así:
```
$ apt-get install pv
```
```
$ dd if=debian-live-10.8.0-amd64-gnome.iso |pv| dd of=/dev/sdb
```
```
(1,4 GB) copiados, 398,941 s, 3,5 MB/s
```
Si lo queremos más chulo debemos especificarle a pv el tamaño del ISO en MB así graficará la barra de progreso con el siguiente comando:

```
$ dd if=debian-live-10.8.0-amd64-gnome.iso |pv -s 2355M | dd of=/dev/sdb
```
```
4588160+0 registros escritos
2349137920 bytes (2,3 GB) copiados, 618,708 s, 3,8 MB/s
2,19GiB 0:10:18 [3,62MiB/s] [=================================> ] 99% 
```

Eso es todo..


