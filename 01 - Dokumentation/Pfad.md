# Pfadangaben – Lokal & Im Netz

## Lokal

### Verzeichnisstruktur

C:\Daten\Bilder
C:\Daten\CSS
C:\Daten\index.html
C:\Daten\Bilder\Blume.jpg
C:\Daten\Bilder\test.html
C:\Daten\CSS\main.css


### Fragen & Antworten

- **Absoluter Pfad von der Datei `main.css`:**  
  `C:\Daten\CSS\main.css`

- **Relativer Pfad von `index.html` zu `Blume.jpg`:**  
  `Bilder/Blume.jpg`

- **Absoluter Pfad von `main.css` zu `Blume.jpg`:**  
  `C:\Daten\Bilder\Blume.jpg`

- **Relativer Pfad von `main.css` zu `Blume.jpg`:**  
  `../Bilder/Blume.jpg`

- **Relativer Pfad von `test.html` zu `Blume.jpg`:**  
  `Blume.jpg`

---

## Im Netz

### Angaben
- **Domain:** `ihreadresse.ch`  
- **Lokaler Root-Pfad:** `/srv/var/www/htdocs`  
- **Document Root:** `/htdocs`

### Dateien

- **Ordner 1:** `wp-content/uploads/2022/5/Dokument.pdf`  
- **Ordner 2:** `wp-content/plugins/neon/files/download.php`

### Fragen & Antworten

- **Lokaler & absoluter Pfad auf `Dokument.pdf`:**  
  `/srv/var/www/htdocs/wp-content/uploads/2022/5/Dokument.pdf`

- **Lokaler & absoluter Pfad auf `download.php`:**  
  `/srv/var/www/htdocs/wp-content/plugins/neon/files/download.php`

- **URL von `Dokument.pdf`:**  
  `https://ihreadresse.ch/wp-content/uploads/2022/5/Dokument.pdf`

- **URL von `download.php`:**  
  `https://ihreadresse.ch/wp-content/plugins/neon/files/download.php`

- **Relativer Pfad in `download.php` zu `Dokument.pdf`:**  
  `../../../uploads/2022/5/Dokument.pdf`
