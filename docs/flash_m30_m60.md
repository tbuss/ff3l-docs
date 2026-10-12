# Aquila M30 und M60 flashen
---

## Einleitung
Die beien D-Link Aquila M30 oder M60 sind super Router, die teilweise für etwa 30 Euro (M30) oder 45 Euro (M60 im 2er Pack) neu zu bekommen sind. 

Allerdings erfordern ein zweistufiges Flashen der Firmware, was aber durchaus machbar ist. 

Die Firmware für beide Geräte ist bei FF3L jeweils hier zu finden:

- [**D-Link⁣ Aquila Pro AI M30**](https://freifunk-firmware.de/?q=D-Link%E2%81%A3%20Aquila%C2%A0Pro%C2%A0AI%C2%A0M30%E2%81%A3%20a1%E2%81%A3) 
- [**D-Link⁣ Aquila Pro AI M60**](https://freifunk-firmware.de/?q=D-Link%E2%81%A3%20Aquila%C2%A0Pro%C2%A0AI%C2%A0M60%E2%81%A3%20a1%E2%81%A3)



### Anleitung bei Freifunk Aachen

Die [Anleitung bei **Freifunk Aachen**](https://freifunk-aachen.de/2026/08/17/installationsanleitung-d-link-m30/) erklärt es bereits ganz gut. 

Der Unterschied zu hier ist: Ich beschreibe M30 und M60 und gehe auf das Problem mit dem Recovery Image bei FF3L ein.

### Problem mit unserem Recovery Image (FF3L)
Ich habe mehrere Male versucht, das Recovery-Image von unserer Firmware-Seite zu verwenden, aber der Router war nach dem Reboot nie unter 192.168.1.1 ansprechbar. Auch hat er keine IP verteilt. Das habe ich mit dem M30 und dem M60 selbst getestet.

Versuchsweise habe ich das Recovery direkt von Gluon verwendet - das hat dann sofort funktioniert.

_Ich weiß noch nicht wieso das so ist, werde aber versuchen es herauszufinden._


## Flashen

1. Am PC einen Chromiumbasierten Browser verwenden (z.B. Chrome, Edge, etc.)
   
2. **OpenWrt Recovery** Firmware herunterladen. _(Von der OpenWrt Seite)_

      - Für den M30 diese: [dlink_aquila-pro-ai-e30-a1-squashfs-recovery.bin](https://downloads.openwrt.org/releases/25.12.5/targets/mediatek/filogic/openwrt-25.12.5-mediatek-filogic-dlink_aquila-pro-ai-e30-a1-squashfs-recovery.bin)
      - Für den M60 diese: [dlink_aquila-pro-ai-m60-a1-squashfs-recovery.bin](https://downloads.openwrt.org/releases/25.12.5/targets/mediatek/filogic/openwrt-25.12.5-mediatek-filogic-dlink_aquila-pro-ai-m60-a1-squashfs-recovery.bin)
  
3. **Sysupgrade Firmware** herunterladen. _(Von eurer Community)_

      - Für den M30 bei FF3L z.B. diese Datei: [gluon-ff3l-v2025.1.1+002-d-link-aquila-pro-ai-m60-a1-sysupgrade.bin](https://freifunk-firmware.de/?q=D-Link%E2%81%A3%20Aquila%C2%A0Pro%C2%A0AI%C2%A0M30%E2%81%A3%20a1%E2%81%A3)
      - Für den M60 bei FF3L z.B. diese Datei: [gluon-ff3l-v2025.1.1+002-d-link-aquila-pro-ai-m60-a1-sysupgrade.bin](https://freifunk-firmware.de/?q=D-Link%E2%81%A3%20Aquila%C2%A0Pro%C2%A0AI%C2%A0M60%E2%81%A3%20a1%E2%81%A3)

4. Computer und Router über einen der weißen LAN-Ports mit einem LAN-Kabel verbinden
   
5. Die IP des PC auf 192.168.200.10 und Subnetzmaske 255.255.255.0 einstellen
   
6. Mit einem spitzen Gegenstand wie einer Büroklammer oder einem Simkartentool den Resetknopf auf der Rückseite des M30/M60 drücken und gleichzeitig den M30/M60 anschalten/einstecken.
Die Büroklammer ein paar Sekunden halten bis die Haupt-LED rot blinkt.

1. Am PC den Browser öffnen und http://192.168.200.1/ eingeben
   Wenn sich ein ganz einfaches Recovery-Tool öffnet, stimmt alles.

2. Hier jetzt die **OpenWrt Recovery** Firmware hochladen

3.  Nach geglücktem Upload der Firmware müssen die Netzwerkeinstellungen aus Schritt 5. wieder rückgängig gemacht werden und auf automatisch bzw DHCP stehen damit folgende Schritte funktionieren.

4.    Das Gerät startet mit OpenWrt-Firmware. Am PC den Browser öffnen und http://192.168.1.1 eingeben.
 Hier kann nun per grafischem User Interface die **Sysupgrade Firmware** eurer Community auf dem Gerät installiert werden.

1.    Nach dem Reboot kann der Router im Browser unter http://192.168.1.1 konhfiguriert werden. 