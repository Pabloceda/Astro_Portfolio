---
title: Configuración de pfSense Firewall
description: Guía de configuración de pfSense con múltiples interfaces, VLANs, NAT y reglas de firewall.
---

## Introducción

Documentación de la configuración de **pfSense** como firewall perimetral en un entorno con tres interfaces de red: WAN, LAN y DMZ. Incluye reglas NAT, políticas de firewall y configuración de VLANs.

## Topología de Red

| Interfaz | Red | Propósito |
|----------|-----|-----------|
| **WAN** | DHCP / IP Pública | Acceso a Internet |
| **LAN** | 192.168.1.0/24 | Red interna de gestión |
| **DMZ** | 172.16.0.0/24 | Servicios expuestos (WordPress) |

## Configuración de Interfaces

### WAN
- Tipo: DHCP (entorno virtualizado)
- Bloqueo de redes RFC1918 habilitado

### LAN  
- IP: 192.168.1.1/24
- DHCP Server: 192.168.1.100 - 192.168.1.200

### DMZ
- IP: 172.16.0.1/24
- Sin DHCP (IPs estáticas para servidores)

## Reglas de Firewall

### Orden jerárquico de reglas

:::caution
El orden de las reglas en pfSense es **fundamental**. Las reglas se evalúan de arriba a abajo y la primera coincidencia se aplica.
:::

1. **Reglas de bloqueo** (deny explícitos)
2. **Reglas de acceso a servicios** específicos  
3. **Regla por defecto** (deny all)

## Port Forwarding (NAT)

Para exponer WordPress desde la DMZ:

```
WAN:80  → 172.16.0.10:8080 (WordPress)
WAN:443 → 172.16.0.10:8443 (WordPress SSL)
```

---

*Documentación completa en la sección [TFG — Arquitectura de Red](/docs/tfg/arquitectura-red/).*
