## ☁️ Architekturdiagramm – Soll-Zustand (AWS mit Docker)

Client-Browser
      |
      v
+------------------+
| Route53 (DNS)    |
| wordpress.yenul.ch |
+------------------+
      |
      v
+-----------------------------+
|   EC2-Instance (Linux)      |
|-----------------------------|
| [NGINX Reverse Proxy]       |
| [Docker: WordPress]         |
| [Docker: MariaDB]           |
+-----------------------------+
      |
      v
+--------------------+
|  S3 (Backups)      |
+--------------------+
