Linux Wartungs- und Sicherheitsskript

Dieses Skript automatisiert grundlegende Wartungs- und Sicherheitsaufgaben auf Debian- und Ubuntu-basierten Linux-Systemen.

Funktionen

Das Skript führt folgende Schritte aus:

1. Prüft, ob das Skript mit Root-Rechten ausgeführt wird.
2. Aktualisiert die Paketlisten (`apt update`).
3. Prüft, ob UFW (Uncomplicated Firewall) installiert ist.
   - Falls nicht installiert, wird UFW automatisch installiert.
4. Prüft den Firewall-Status.
   - Falls deaktiviert, wird die Firewall aktiviert.
5. Führt ein vollständiges System-Upgrade durch.
   - `apt full-upgrade -y`
6. Führt Systembereinigung durch.
   - `apt autoremove -y`
   - `apt autoclean`
7. Prüft, ob ClamAV installiert ist.
   - Falls nicht installiert, wird ClamAV automatisch installiert.
8. Aktualisiert die Virensignaturen mit `freshclam`.
9. Führt einen vollständigen Virenscan des Systems durch.
10. Speichert das Scan-Ergebnis in einer Logdatei.



Unterstützte Systeme

Getestet bzw. ausgelegt für:

- Debian
- Ubuntu
- Linux Mint
- Pop!_OS
- Kubuntu
- Xubuntu
- Lubuntu

Das Skript verwendet `apt` und ist daher nicht mit Fedora, CentOS, AlmaLinux, Arch Linux oder openSUSE kompatibel.



Installation

Skript speichern

sudo mkdir -p /opt/scripts
sudo nano /opt/scripts/wartung.sh


Den Skriptinhalt einfügen und speichern.

Ausführbar machen

sudo chmod +x /opt/scripts/wartung.sh



Manuelle Ausführung

sudo /opt/scripts/wartung.sh


Automatische Ausführung beim Systemstart

Service-Datei erstellen

sudo nano /etc/systemd/system/wartung.service


Folgenden Inhalt einfügen:

ini
[Unit]
Description=Linux Wartungs- und Sicherheitsskript
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/opt/scripts/wartung.sh
User=root

[Install]
WantedBy=multi-user.target


Service aktivieren

sudo systemctl daemon-reload
sudo systemctl enable wartung.service


Service testen

sudo systemctl start wartung.service


Status prüfen:

sudo systemctl status wartung.service


Log anzeigen:

journalctl -u wartung.service




Virenscan-Protokoll

Das Virenscan-Protokoll wird unter folgendem Pfad gespeichert:


/var/log/clamscan/scan.log


Anzeigen:

sudo cat /var/log/clamscan/scan.log


Live beobachten:

sudo tail -f /var/log/clamscan/scan.log


Hinweise

Laufzeit

Die Ausführungszeit hängt unter anderem ab von:

- Internetgeschwindigkeit
- Anzahl verfügbarer Updates
- CPU-Leistung
- Größe der Festplatte
- Anzahl der Dateien

Ein vollständiger ClamAV-Scan kann mehrere Minuten bis mehrere Stunden dauern.

Firewall

Beim ersten Start von UFW werden die Standardregeln aktiviert:

- Eingehend: blockiert
- Ausgehend: erlaubt

Falls individuelle Firewall-Regeln benötigt werden, sollten diese vor dem Produktiveinsatz eingerichtet werden.



Deinstallation

Systemdienst deaktivieren:

sudo systemctl disable wartung.service


Service-Datei löschen:

sudo rm /etc/systemd/system/wartung.service


Neu laden:

sudo systemctl daemon-reload


Skript entfernen:

sudo rm /opt/scripts/wartung.sh


(c)  Zetitweisebaum

Eigenes Wartungs- und Sicherheitsskript für Debian- und Ubuntu-basierte Linux-Systeme.
