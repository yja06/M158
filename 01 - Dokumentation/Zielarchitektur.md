# 🏗️ Zielarchitektur – WordPress CMS auf AWS

## Beschreibung

Die WordPress-Applikation wird in einer cloudbasierten Umgebung auf **AWS** betrieben. Die Architektur setzt sich aus folgenden Hauptkomponenten zusammen:

- **EC2-Instanz für Webserver** (Apache, PHP, WordPress)
- **EC2-Instanz oder gleiche Instanz für Datenbank** (MariaDB)
- **DNS-Zuordnung** über z. B. Route53
- Kommunikation über HTTP(S) zwischen Client und Webserver

## 🔧 Systemübersicht

## 🔧 Systemübersicht

```mermaid
graph TD
    A[Client-Browser] --> B[Apache Webserver (EC2)]
    B --> C[PHP verarbeitet Anfrage]
    C --> D[MariaDB Datenbank (EC2)]
    B --> E[WordPress Files (wp-content)]
    D --> F[WordPress DB (wp_posts, wp_users, etc.)]
