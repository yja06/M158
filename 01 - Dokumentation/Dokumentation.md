# WordPress Docker Setup (für EC2)

## ✨ Projektziel

WordPress auf einer EC2-Instanz bereitstellen mit Docker Compose (Webserver + MariaDB)

---

## ⚙ Architekturübersicht

```
[ EC2 Instance (Ubuntu) ]
        |
        |-- docker-compose.yml
        |
        |-- [web]    --> Apache + PHP + WordPress (Port 80)
        |-- [db]     --> MariaDB (Port 3306)
        |
        '-- volumes  --> html/ & db_data/
```

---

## 📊 Verwendete Komponenten

* **Docker Version:** 24.x
* **Docker Compose:** 2.x
* **MariaDB:** latest
* **PHP Apache Image:** php:8.2-apache

---

## 📦 docker-compose.yml

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "80:80"
    depends_on:
      - db
    volumes:
      - ./html:/var/www/html

  db:
    image: mariadb:latest
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

---

## 🔧 Dockerfile

```Dockerfile
FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
    libzip-dev libpng-dev libjpeg-dev libfreetype6-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd mysqli zip

EXPOSE 80
COPY ./html/ /var/www/html/
```

---

## 🚀 Startbefehle

```bash
docker-compose up --build -d
```

---

## 📲 Zugriff auf WordPress

* **URL:** `http://<EC2-PUBLIC-IP>`
* **Admin:** Benutzername & Passwort aus Setup

---

## ✅ Optionale Erweiterungen

* PhpMyAdmin als Container
* HTTPS (Let's Encrypt oder self-signed)
* GitHub CI/CD

---

## 📑 Status

* WordPress erfolgreich installiert und erreichbar
* Datenbankverbindung aktiv
* Admin-Login funktioniert ✅
