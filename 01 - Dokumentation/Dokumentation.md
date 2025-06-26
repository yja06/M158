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

Sicherheitsgruppe:
![image](https://github.com/user-attachments/assets/f6efd238-e93c-41bb-9e4f-3f7072bcaa54)

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
* 
<img width="874" alt="wp config" src="https://github.com/user-attachments/assets/51ab356b-492a-4f90-ac67-fe52d87b70ac" />
<img width="648" alt="worpress letzte config" src="https://github.com/user-attachments/assets/9104b0aa-b606-4275-ab52-e83ce5f4cc01" />
<img width="947" alt="WORDPRESS POST" src="https://github.com/user-attachments/assets/7bd066b8-899a-460e-b3e6-61471a10328f" />
<img width="948" alt="wordpress geht" src="https://github.com/user-attachments/assets/29278720-9873-48ee-a96f-600679c86014" />
<img width="944" alt="WORDPRESS ANMELDEN" src="https://github.com/user-attachments/assets/9849788f-4b49-4dd4-b02a-9f86ac56b114" />
<img width="946" alt="webserver erreichbar" src="https://github.com/user-attachments/assets/d7652162-bfd0-4222-a033-136aa801db9b" />


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
<img width="738" alt="von web auf db zugreifen" src="https://github.com/user-attachments/assets/495d1783-19e2-46f9-b252-577abc208723" />

# Phase 5 – Inbetriebnahme & DNS-Anbindung

## ✅ Ziel dieser Phase

- WordPress unter benutzerfreundlichem DNS-Namen erreichbar machen  
- HTTPS aktivieren  
- Funktion über DNS oder lokal simulieren  
- Abschlusskontrolle und Test der Migration

---

## 🔧 1. DNS-Konfiguration (lokal via hosts-Datei)

Da kein öffentlicher DNS verfügbar war, wurde der DNS-Name lokal über die `hosts`-Datei umgesetzt:

```plaintext
44.202.0.227    cms.yenulprojekt.ch
```

<img width="944" alt="https geht" src="https://github.com/user-attachments/assets/6efbdd6c-34bc-4f7e-9392-83dcd9d991a3" />
<img width="959" alt="dns funktioniert" src="https://github.com/user-attachments/assets/f035d8db-8945-4daf-ab07-5902a36a7f30" />

