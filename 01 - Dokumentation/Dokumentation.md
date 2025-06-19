## Phase 1 – Projektstart

### Ziel des Projekts

Migration einer bestehenden WordPress-Website (ohne Plugins) von einem Quellserver ([https://m158.geekz.ch/](https://m158.geekz.ch/)) auf eine eigene neue AWS-Infrastruktur mittels Docker.

### Anforderungen

* Kein Plugin-Import
* Migration der Inhalte (Posts, Seiten, Medien)
* Eigenes Setup mit EC2, Apache, PHP, MariaDB
* Kein fertiges Image, alles manuell über Docker bzw. Shell
* Schrittweise Umsetzung in 5 Phasen mit Screenshots

### Projektumgebung

* 3 EC2-Instanzen:

  * Webserver (Apache + PHP + WordPress)
  * Datenbank (MariaDB)
  * PhpMyAdmin (Testing + Zugriff)
* Verbindung via SSH mit SSH-Key `SSH.pem`
* Manuelles Aufsetzen über Shell
![image](https://github.com/user-attachments/assets/a03e95d6-0dc2-4426-8106-fa23e0964979)

### Tools & Technologien

* AWS EC2 (Ubuntu 22.04)
* Apache2
* PHP 8.1
* MariaDB
* WordPress (neuste Version)
* PhpMyAdmin
* SCP für Dateiübertragungen
---
Phase 2: Backup & Export

Mit FileZilla die WordPress-Dateien vom Quellserver heruntergeladen

<img width="585" alt="file zilla übertragung" src="https://github.com/user-attachments/assets/d57cdac9-4146-46b5-bb8f-5dd9c48bedf9" />

Die Datenbankdump-Datei wp_m158_db.sql gefunden oder extrahiert
![image](https://github.com/user-attachments/assets/78aade93-677c-4e60-8fb5-65e87cd32d2e)

Dateien lokal auf dem Laptop gespeichert in einem Ordner M158

Verzeichnis enthält z. B. wp-content, wp-config.php, index.php, wp_m158_db.sql usw.

Export geprüft und auf Vollständigkeit kontrolliert
