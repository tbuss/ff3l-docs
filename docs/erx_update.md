# Edgerouter ERX Update

---





## Problem

Der Edgerouter ERX muss beim Update von auf gluon-v2025.1.1 (z.B. von gluon-v2023.2.4) auf ein neues Flash-Layout angepasst werden.
Der Grund ist, dass das Flash-Layout bisher in 2 Teile für den Kernel geteilt wurde und jetzt ein großer Teil benötigt wird.

!!! Beim Ändern des Layouts geht die Configuration verloren uns man muss Zugriff auf den Router im Config mode haben.


https://github.com/oszilloskop/UBNT_ERX_Gluon_Factory-Image/blob/master/README.md#gluon-auf-ubnt-edgerouter-x-und-x-sfp



---

## Vorab check

Das Kommando ```gluon-info``` sagt euch die Firmware-Version:
```sh
Firmware release:    v2023.2.4+001
```

Mit `cat /proc/mtd` bekommt ihr das Flash layout:
```sh
dev:    size   erasesize  name
mtd0: 00080000 00020000 "u-boot"
mtd1: 00060000 00020000 "u-boot-env"
mtd2: 00060000 00020000 "factory"
mtd3: 00300000 00020000 "kernel1"
mtd4: 00300000 00020000 "kernel2"
mtd5: 0f7c0000 00020000 "ubi"
```

**mtd3** und **mtd4** sind die beiden Teile (vor Version 2025.1.1)


---

## Sicherung der config
Wir sichern den Key (1) oder die gesamte Config (2) für die spätere Wiederherstellung.

### 1. Nur den Key sichern
Den privaten Key sichern mit `uci show | grep secret`

```sh
fastd.mesh_vpn.secret='HIER_STEHT_EUER_64_ZEICHEN_KEY'
```
Kopiert euch diesen Key für später irgendwo hin.

### 2. Die komplette Config sichern
Einfach alles sichern mit. Zuerst einen Export auf dem Router machen:

```sh
cd /tmp
uci export > backup.uci
```

Dann die Datei "backup.uci" auf euren Rechner, die geht beim nächsten Schritt (Migration) verloren!

Von eurem Rechner mit SSH-Zugang in einem Terminal z.B. so:

```sh
scp -O root@[fdc7:3c9d:ff31:5:EURE:IPV6:ADDR:ESSE]:/tmp/backup.uci /tmp/backup.uci
root@fdc7:3c9d:ff31:5:ae8b:a9ff:febd:302a's password: 
backup.uci                                                                          100%   20KB   1.5MB/s   00:00 

```
Das kopiert die Remote erstellte Datei in euer lokales /tmp Verzeichnis.


## Die Migration ausführen

Hier wird die Migration ausgeführt und der Router ist schließlich jungfräulich - und nach dem Reboot im Config mode. 

Hier stehr mehr dazu: https://github.com/darkxst/erx-migration

Auf dem Router also ausführen:

```sh
cd /tmp
wget https://raw.githubusercontent.com/darkxst/erx-migration/refs/heads/main/ubnt_erx_migrate.sh
wget https://raw.githubusercontent.com/darkxst/erx-migration/refs/heads/main/ubnt_erx_stage2.sh
```

Jetzt das image downloaden und umbenennen. Hinweis: Hier das aktuelle (-> Firmware-Homepage) und richtige verwenden (EdgeRouter X != Edgerouter X SFP)!

```sh
wget https://freifunk-firmware.de/images-release/stable/sysupgrade/gluon-ff3l-v2025.1.1%2B002-ubiquiti-edgerouter-x-sysupgrade.bin
mv gluon-ff3l-v2025.1.1%2B002-ubiquiti-edgerouter-x-sysupgrade.bin sysupgrade.img
```

Jetzt das Script ausführbar machen und ausführen:

```sh
chmod +x ubnt_erx_migrate.sh
./ubnt_erx_migrate.sh
```
Hinweis: Hier wird auch das "stage2"-Script ausgeführt.

Das Gerät rebootet...

Nach dem Reboot braucht ihr eine Verbindung zum Config mode.
Also: LAN-Kabel in eth1 und eine IP im Bereich 192.168.1.0/24 (z.B. 192.168.1.100)

Im Browser solltet ihr unter http://192.168.1.1 die Konfiguration sehen.
- Hier solltet ihr den Remotezugriff wieder einstellen (Entweder Passwort setzen oder einen SSH-Key)


## Wiederherstellen der Config/des Keys

Wir stellen nur den Key - oder eben die ganze Config wieder her.

### 1. Nur den Key setzen

```sh
uci set fastd.mesh_vpn.secret='HIER_STEHT_EUER_64_ZEICHEN_KEY'
uci commit fastd
```

### 2. Die komplette Config wiederherstellen

Am lokalen Rechner im Terminal:

```sh
scp -O /tmp/backup.uci root@192.168.1.1:/tmp/backup.uci
```
Das kopiert die lokale (vorher kopierte) Datei auf eurem Router im /tmp Verzeichnis.

*Hinweis:* Wenn es Probleme mit dem Host key gibt, dann kann dieser mit ```ssh-keygen -R 192.168.1.1``` gelöscht werden (oder manuell aus der Datei ~/.ssh/known_hosts gelöscht werden).


An der SSH-Sitzung auf dem Router:

```sh
cd /tmp
cat backup.uci | uci import
```

Auf der Configurationsseite (im Browser) kann die Config überprüft werden. Knotenposition/Hostname usw. sollten wieder stimmen.


**Wichtig:**
Hier muss noch die Version des neuen Flash-Layouts überschrieben werden, da der router sonst denkt, er sei nicht migriert. 
(Das haben wir mit dem Import auf den alten Wert gesetzt)

```sh
uci set system.@system[0].compat_version=2.0
uci commit
```


### Neustarten

Mit ```reboot``` oder über die Weboberfläche kann rebootet werden. LAN-Kabel wieder umstecken, der Router sollte funktionieren wie vorher.


## Überprüfung (optional)

Das Kommando ```gluon-info``` sagt euch die Firmware-Version:
```sh
Firmware release:               v2025.1.1+002
```

Mit `cat /proc/mtd` bekommt ihr das Flash layout:
```sh
dev:    size   erasesize  name
mtd0: 00080000 00020000 "u-boot"
mtd1: 00060000 00020000 "u-boot-env"
mtd2: 00060000 00020000 "factory"
mtd3: 00600000 00020000 "kernel"
mtd4: 0f7c0000 00020000 "ubi"
```

