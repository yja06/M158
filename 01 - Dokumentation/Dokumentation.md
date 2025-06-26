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

---

## Phase 3 – Import & Konfiguration

In dieser Phase haben wir die alte WordPress-Seite auf die neue Zielumgebung übertragen und konfiguriert.

### Datenbank-Import

* Die Datei `wp_m158_db.sql` wurde per `scp` auf die DB-Instanz übertragen.
* Danach erfolgte der Import mit folgendem Befehl:

  ```bash
  mysql -u wpuser -p wp_m158 < /home/ubuntu/wp_m158_db.sql
  ```
* Der Import war erfolgreich.

### WordPress-Dateien kopieren

* Die alten WordPress-Dateien (HTML, PHP, wp-content usw.) wurden per FTP/SCP übertragen.
* Dateien wurden nach `/var/www/html/` auf die Webserver-Instanz kopiert.

### Konfiguration

* `wp-config.php` wurde angepasst:

  * Datenbankname: `wp_m158`
  * Benutzer: `wpuser`
  * Passwort: `wpuser-passwort`
  * DB-Host: IP der Datenbank-Instanz

### Ergebnis

* Nach Abschluss war die Website unter der Public-IP des Webservers erreichbar.
* Beispielanzeige: „Hello world!“ (Standard-Beitrag)

---

## Phase 4: Inbetriebnahme & Testing

In dieser Phase wurde die migrierte WordPress-Seite auf der neuen Zielumgebung getestet und in Betrieb genommen.

### Schritte:

- WordPress im Browser geöffnet (http://<webserver-ip>)
- Admin-Login getestet mit Benutzer `wp_user`, Passwort `12344`
- Verbindung zur Datenbank erfolgreich bestätigt
- Inhalte (z. B. Beiträge, Seiten) auf korrekte Migration geprüft
- Medieninhalte und Layout wurden auf Vollständigkeit überprüft

### Ergebnis:

Die WordPress-Seite funktioniert wie auf dem Quellsystem. Keine Fehler aufgetreten. Die Migration war erfolgreich.
