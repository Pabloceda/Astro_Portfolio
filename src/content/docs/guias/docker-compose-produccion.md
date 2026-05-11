---
title: Docker Compose para Producción
description: Guía completa para desplegar servicios con Docker Compose en un entorno de producción seguro.
---

## Introducción

Esta guía documenta el proceso de despliegue de un stack **WordPress + MySQL** utilizando Docker Compose con configuraciones de producción, incluyendo hardening de seguridad, Docker Secrets y healthchecks.

## Requisitos Previos

- Ubuntu Server 22.04+ con acceso root
- Docker Engine instalado (ver [instalación oficial](https://docs.docker.com/engine/install/ubuntu/))
- Docker Compose v2+

## Estructura del Proyecto

```bash
wordpress-stack/
├── docker-compose.yml
├── .env
├── secrets/
│   ├── db_password.txt
│   └── db_root_password.txt
└── config/
    └── uploads.ini
```

## Docker Compose

```yaml title="docker-compose.yml"
services:
  db:
    image: mysql:8.0.36
    container_name: wordpress-db
    restart: unless-stopped
    environment:
      MYSQL_DATABASE: wordpress_db
      MYSQL_USER: wp_user
      MYSQL_PASSWORD_FILE: /run/secrets/db_password
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
    secrets:
      - db_password
      - db_root_password
    volumes:
      - db_data:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 30s
      timeout: 10s
      retries: 5

  wordpress:
    image: wordpress:6.4-php8.2-apache
    container_name: wordpress-app
    restart: unless-stopped
    depends_on:
      db:
        condition: service_healthy
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_NAME: wordpress_db
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD_FILE: /run/secrets/db_password
      WORDPRESS_TABLE_PREFIX: "wp_custom_"
    secrets:
      - db_password
    volumes:
      - wp_data:/var/www/html

secrets:
  db_password:
    file: ./secrets/db_password.txt
  db_root_password:
    file: ./secrets/db_root_password.txt

volumes:
  db_data:
  wp_data:
```

## Medidas de Seguridad Aplicadas

:::note
Estas medidas son esenciales para cualquier despliegue en producción.
:::

1. **Docker Secrets** en lugar de variables de entorno en texto plano
2. **Healthchecks** para garantizar la disponibilidad de los servicios
3. **Prefijo de tablas personalizado** para WordPress
4. **Imágenes con tags fijos** (no `latest`) para reproducibilidad
5. **Volúmenes nombrados** para persistencia de datos

## Despliegue

```bash
# Crear los ficheros de secrets
echo "contraseña_segura_aquí" > secrets/db_password.txt
echo "contraseña_root_segura" > secrets/db_root_password.txt

# Levantar los servicios
docker compose up -d

# Verificar el estado
docker compose ps
```

---

*Esta documentación forma parte del [TFG de Pablo Calderón](/docs/tfg/arquitectura-red/).*
