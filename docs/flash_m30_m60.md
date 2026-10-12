# Flash Aquila M30 / M60
---

## Einleitung
Der Aquila M30 oder M60 erfordern ein zweistufiges Flashen der Firmware. 

D-Link⁣ Aquila Pro AI M30⁣ a1⁣ https://freifunk-firmware.de/?q=D-Link%E2%81%A3%20Aquila%C2%A0Pro%C2%A0AI%C2%A0M30%E2%81%A3%20a1%E2%81%A3
D-Link⁣ Aquila Pro AI M60⁣ a1⁣ https://freifunk-firmware.de/?q=D-Link%E2%81%A3%20Aquila%C2%A0Pro%C2%A0AI%C2%A0M60%E2%81%A3%20a1%E2%81%A3

## 

Die Anleitung bei Freifunk Aachen (https://freifunk-aachen.de/2026/08/17/installationsanleitung-d-link-m30/) erklärt es ganz gut. Der Unterschied hier ist: Ich beschreibe M30 und M60 und gehe auf das Problem mit dem Recovery Image bei FF3L ein.


## Ablauf

1. Am PC einen Chromiumbasierten Browser verwenden (z.B. Chrome, Edge, etc.)
2. **OpenWrt Recovery** Firmware herunterladen
   Hinweis: https://freifunk-firmware.de/images-release/stable/other/gluon-ff3l-v2025.1.1%2B002-d-link-aquila-pro-ai-m30-a1-recovery.bin
   1. Für den M30 hier: dlink_aquila-pro-ai-e30-a1-squashfs-recovery.bin (https://downloads.openwrt.org/releases/25.12.5/targets/mediatek/filogic/openwrt-25.12.5-mediatek-filogic-dlink_aquila-pro-ai-e30-a1-squashfs-recovery.bin)
   2. Für den M60 hier: dlink_aquila-pro-ai-m60-a1-squashfs-recovery.bin (https://downloads.openwrt.org/releases/25.12.5/targets/mediatek/filogic/openwrt-25.12.5-mediatek-filogic-dlink_aquila-pro-ai-m60-a1-squashfs-recovery.bin)
3. **Sysupgrade Firmware** herunterladen - von eurer Community. Bei FF3L sind das:
   1. Für den M30 z.B. das gluon-ff3l-v2025.1.1+002-d-link-aquila-pro-ai-m60-a1-sysupgrade.bin von hier: https://freifunk-firmware.de/?q=D-Link%E2%81%A3%20Aquila%C2%A0Pro%C2%A0AI%C2%A0M30%E2%81%A3%20a1%E2%81%A3
   2. Für den M60 z.B. das gluon-ff3l-v2025.1.1+002-d-link-aquila-pro-ai-m60-a1-sysupgrade.bin von hier: https://freifunk-firmware.de/?q=D-Link%E2%81%A3%20Aquila%C2%A0Pro%C2%A0AI%C2%A0M60%E2%81%A3%20a1%E2%81%A3

4. Computer und Router über einen der weißen LAN-Ports mit einem LAN-Kabel verbinden
5. Die IP des PC auf 192.168.200.10 und Subnetzmaske 255.255.255.0 einstellen
6. Mit einem spitzen Gegenstand wie einer Büroklammer oder einem Simkartentool den Resetknopf auf der Rückseite des M30/M60 drücken und gleichzeitig den M30/M60 anschalten/einstecken (Die Büroklammer ein paar Sekunden halten bis die Haupt-LED rot blinkt)
7. Am PC den Browser öffnen und http://192.168.200.1/ eingeben
   Wenn sich ein ganz einfaches Recovery-Tool öffnet, stimmt alles.
8. Hier jetzt die **OpenWrt Recovery** Firmware hochladen

9. Nach geglücktem Upload der Firmware müssen die Netzwerkeinstellungen aus Schritt 5. wieder rückgängig gemacht werden und auf automatisch bzw DHCP stehen damit folgende Schritte funktionieren.

10. Das Gerät startet mit OpenWrt-Firmware. Am PC den Browser öffnen und http://192.168.1.1 eingeben.
 Hier kann nun per grafischem User Interface die **Sysupgrade Firmware** eurer Community auf dem Gerät installiert werden.

11. Nach dem Reboot kann der Router im Browser unter http://192.168.1.1 konhfiguriert werden. 