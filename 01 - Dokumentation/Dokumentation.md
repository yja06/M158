## Phase 1 – Projektstart

### Ziel des Projekts

Migration einer bestehenden WordPress-Website (ohne Plugins) von einem Quellserver ([https://m158.geekz.ch/](https://m158.geekz.ch/)) auf eine eigene neue AWS-Infrastruktur mittels Docker.

**Vorgehen:**

* Projekt in 5 Phasen aufgeteilt
* GitHub-Repository erstellt
* Bewertungsraster analysiert
* Quellumgebung: WordPress ohne Plugins, FTP-Zugang
* Ich habe später im Projekt elasticIPs erstellt, weil die IPs im Verlaufe des Projektes immer geändert wurden, welches mich sehr störte.

---

## Phase 2 – Architektur & Infrastruktur

### EC2-Instanzen:

* **Webserver (wp-server)** – Apache, PHP-FPM, WordPress
* **Datenbankserver (db-server)** – MariaDB
* **Adminer (admin-server)** – zur DB-Verwaltung (optional)
![image](https://github.com/user-attachments/assets/a03e95d6-0dc2-4426-8106-fa23e0964979)

### Sicherheit:

* SSH-Key Zugriff
* Security Groups: Port 22, 80, 443, 3306 gezielt freigegeben

Sicherheitsgruppe:
![image](https://github.com/user-attachments/assets/f6efd238-e93c-41bb-9e4f-3f7072bcaa54)

### Kommunikation:

* wp-server ↔ db-server über private IP
* Kein Public DB-Zugriff
---
## Phase 3 – Migration & Einrichtung

### 1. Datenmigration:

* Inhalte von `m158.geekz.ch` über Filezilla gesichert (Dateien & SQL-Dump)
* `wp-content` übernommen

<img width="585" alt="file zilla übertragung" src="https://github.com/user-attachments/assets/d57cdac9-4146-46b5-bb8f-5dd9c48bedf9" />

Die Datenbankdump-Datei wp_m158_db.sql gefunden oder extrahiert
![image](https://github.com/user-attachments/assets/78aade93-677c-4e60-8fb5-65e87cd32d2e)

Dateien lokal auf dem Laptop gespeichert in einem Ordner M158

Verzeichnis enthält z. B. wp-content, wp-config.php, index.php, wp_m158_db.sql usw.

Export geprüft und auf Vollständigkeit kontrolliert

### 2. Datenbank:

* MariaDB auf db-server installiert und gestartet
* Datenbank `wp_m158` importiert:

```bash
mysql -u root -p < wp_m158.sql
```

* Benutzer `wpuser` mit Remote-Rechten angelegt:

```sql
CREATE USER 'wpuser'@'%' IDENTIFIED BY '12344';
GRANT ALL PRIVILEGES ON wp_m158.* TO 'wpuser'@'%';
```

<img width="664" alt="image" src="https://github.com/user-attachments/assets/5a1b9fc0-8ab2-4293-98a5-10a0ac77f9c7" />


Mein User:
![image](https://github.com/user-attachments/assets/62ae3a0f-ecc8-4cf3-b8c0-dfea38a86fde)

---

Userrechte:
![image](https://github.com/user-attachments/assets/a8968d18-d681-4c68-89e5-09ddb1b73844)

---

root User:
![image](https://github.com/user-attachments/assets/03d07953-af66-4f82-ab0d-82878cbaf34b)

---

### 3. Webserver & PHP:

* Apache + PHP-FPM installiert:

```bash
sudo apt install apache2 php php-fpm libapache2-mod-php mariadb-client
```

* PHP über FPM eingebunden:

```bash
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php*-fpm
sudo systemctl restart apache2
```

* Datei `php.info.php` unter `/var/www/html/` bereitgestellt zur Prüfung

![image](https://github.com/user-attachments/assets/6dc2277a-698f-4641-8588-adaaf22020f4)

### 4. WordPress:

* WordPress-Dateien nach `/var/www/html/` kopiert
* `wp-config.php` angepasst:

```php
define( 'DB_NAME', 'wp_m158' );
define( 'DB_USER', 'wpuser' );
define( 'DB_PASSWORD', '12344' );
define( 'DB_HOST', '172.31.86.168' );
```

* HTTPS über self-signed Zertifikat aktiviert:

```bash
sudo a2enmod ssl
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2
```
<img width="874" alt="wp config" src="https://github.com/user-attachments/assets/51ab356b-492a-4f90-ac67-fe52d87b70ac" />
<img width="648" alt="worpress letzte config" src="https://github.com/user-attachments/assets/9104b0aa-b606-4275-ab52-e83ce5f4cc01" />
<img width="947" alt="WORDPRESS POST" src="https://github.com/user-attachments/assets/7bd066b8-899a-460e-b3e6-61471a10328f" />
<img width="948" alt="wordpress geht" src="https://github.com/user-attachments/assets/29278720-9873-48ee-a96f-600679c86014" />


---
## Phase 4 – Inbetriebnahme & Test

### Getestet:

* `https://54.166.7.177` → lädt WordPress Startseite
* Login über `/wp-admin`
* Datenbankverbindung erfolgreich
* php.info.php zeigt `FPM/FastCGI`

### WordPress-Einstellungen:

* Startseite auf statische Seite umgestellt
* Beispielseite sichtbar

### Screenshots erstellt von:

* Webseite mit DNS
* php.info.php
* Apache/SSL-Status
* wp-config.php

![image](https://github.com/user-attachments/assets/ee648415-a307-4de7-b702-489d3555ee16)

---

Die WordPress-Seite funktioniert wie auf dem Quellsystem. Keine Fehler aufgetreten. Die Migration war erfolgreich.
<img width="738" alt="von web auf db zugreifen" src="https://github.com/user-attachments/assets/495d1783-19e2-46f9-b252-577abc208723" />

---
<img width="944" alt="WORDPRESS ANMELDEN" src="https://github.com/user-attachments/assets/9849788f-4b49-4dd4-b02a-9f86ac56b114" />
<img width="946" alt="webserver erreichbar" src="https://github.com/user-attachments/assets/d7652162-bfd0-4222-a033-136aa801db9b" />

---

## FTP Server

![image](https://github.com/user-attachments/assets/2c55c3ba-7731-46fe-b871-615436a40fb0)

---

## Phase 5 – DNS & Abschluss

### DNS-Anbindung:

* Da keine öffentliche Domain verfügbar war → `hosts`-Datei lokal verwendet

```plaintext
44.202.0.227    cms.yenulprojekt.ch
```
Hostdatei abgeändert
![image](https://github.com/user-attachments/assets/8fd453ab-175d-4e4b-aa8b-01448d6eecb3)

Meine default.conf
![image](https://github.com/user-attachments/assets/e6d06e31-227f-423f-9527-494f49dc6f46)

Pfad unter Windows:

```
C:\Windows\System32\drivers\etc\hosts
```

Meine wp-config.php
![image](https://github.com/user-attachments/assets/b2762183-816b-4a20-9f04-a52747548a83)

---

### SSL:

* Selbstsigniertes Zertifikat aktiviert
* Zugriff über `https://cms.yenulprojekt.ch` funktioniert

<img width="944" alt="https geht" src="https://github.com/user-attachments/assets/6efbdd6c-34bc-4f7e-9392-83dcd9d991a3" />
<img width="959" alt="dns funktioniert" src="https://github.com/user-attachments/assets/f035d8db-8945-4daf-ab07-5902a36a7f30" />

### HTTP → HTTPS Redirect:

* In `/etc/apache2/sites-available/000-default.conf`:
* 
![image](https://github.com/user-attachments/assets/9d7af0e5-ac0d-44d6-9ee8-5aeb972c9280)

```apache
Redirect permanent / https://cms.yenulprojekt.ch/
```

Danach Apache neu geladen:

```bash
sudo systemctl reload apache2
```

### Tests:

| Testfall                      | Ergebnis        |
| ----------------------------- | --------------- |
| Aufruf via DNS (`cms...`)     | ✅ Erfolgreich   |
| HTTPS aktiv (SSL-Zertifikat)  | ✅ Mit Warnung   |
| HTTP Redirect auf HTTPS       | ✅ Erfolgreich   |
| Startseite sichtbar           | ✅ „Sample Page“ |
| Login via `/wp-admin` möglich | ✅ Erfolgreich   |
| Server API = FPM/FastCGI      | ✅ Ja            |

---

PHP-FPM Integration mit Apache (Server API: FPM/FastCGI)

### Ziel
Damit die Bewertung vollständig ist, musste PHP über **FPM/FastCGI** angebunden werden und nicht über das Standardmodul `apache2handler`.

---

### PHP Health Check
![image](https://github.com/user-attachments/assets/bd505873-5b26-4c44-83c5-36759a0a33ca)

---

Webseite Migration funktioniert:
![image](https://github.com/user-attachments/assets/443eb4d7-5a20-4133-ae9e-0fd1db32aac8)
![image](https://github.com/user-attachments/assets/3fa43293-7af3-41d6-9c04-1125efeff1b7)
![image](https://github.com/user-attachments/assets/80c68a48-164c-4af6-b280-fa5eca06f005)
![image](https://github.com/user-attachments/assets/d3b4d80e-1004-4b9d-8a9c-6731de49892d)
![image](https://github.com/user-attachments/assets/01978611-64dd-4546-a7ee-0525144a5be0)
![image](https://github.com/user-attachments/assets/78e604b9-0935-47e8-9aa9-6be53dd767f2)
![image](https://github.com/user-attachments/assets/5ac16d57-0c40-4efc-8710-e79455401a55)

---

PhpMyAdmin funktioniert:
![image](https://github.com/user-attachments/assets/338ee652-4b8d-4906-ba57-6f12b9022d5e)
![image](https://github.com/user-attachments/assets/3e376387-2028-4494-a17f-0705a9271238)


