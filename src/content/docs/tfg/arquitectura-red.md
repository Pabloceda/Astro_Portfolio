---
title: Arquitectura de Red
description: Diseño y arquitectura de la infraestructura de red segura del TFG de Pablo Calderón.
---

## Descripción del Proyecto

El Trabajo de Fin de Grado (TFG) consiste en el diseño, implementación y documentación de una **infraestructura de red empresarial segura** en un entorno virtualizado. El proyecto abarca desde la segmentación de red con VLANs hasta el despliegue de servicios en contenedores Docker, pasando por la monitorización con un SIEM.

## Objetivos

1. Diseñar una topología de red segura con segmentación por zonas
2. Implementar un firewall perimetral con pfSense
3. Desplegar servicios web en una DMZ con Docker
4. Monitorizar la infraestructura con Wazuh SIEM
5. Documentar todo el proceso de forma profesional

## Arquitectura General

La infraestructura se compone de tres zonas de seguridad:

| Zona | Función | Segmento de Red |
|------|---------|-----------------|
| **LAN** | Red interna, gestión y administración | 192.168.1.0/24 |
| **DMZ** | Servicios expuestos a Internet | 172.16.0.0/24 |
| **WAN** | Conexión a Internet | DHCP |

## Componentes Principales

### pfSense (Firewall/Router)
- 3 interfaces: WAN, LAN, DMZ
- NAT/Port Forwarding
- Reglas de firewall por zona
- VLANs para segmentación interna

### Ubuntu Server (DMZ)
- Docker Engine + Docker Compose
- WordPress + MySQL (producción hardened)
- Healthchecks y Docker Secrets

### Wazuh SIEM (LAN)
- Wazuh Manager + Indexer + Dashboard
- Agentes en todos los endpoints
- Detección de amenazas en tiempo real

## Tecnologías Utilizadas

- **Virtualización**: VirtualBox / VMware
- **Firewall**: pfSense CE
- **Contenedores**: Docker + Docker Compose
- **SIEM**: Wazuh 4.x
- **CMS**: WordPress
- **Base de datos**: MySQL 8.0
- **Documentación**: LaTeX

## Documentación Relacionada

- [Configuración de pfSense](/docs/guias/pfsense-firewall/)
- [Docker Compose para Producción](/docs/guias/docker-compose-produccion/)
- [Instalación de Wazuh SIEM](/docs/guias/wazuh-siem/)
