---
title: Instalación de Wazuh SIEM
description: Guía de instalación y configuración de Wazuh como sistema SIEM para monitorización de seguridad.
---

## Introducción

**Wazuh** es una plataforma de seguridad open-source que proporciona detección de amenazas, monitorización de integridad, respuesta a incidentes y cumplimiento normativo. Esta guía cubre la instalación en Ubuntu Server como parte de una infraestructura de seguridad corporativa.

## Componentes de Wazuh

| Componente | Función |
|------------|---------|
| **Wazuh Manager** | Motor de análisis y correlación de eventos |
| **Wazuh Indexer** | Almacenamiento y búsqueda de alertas (basado en OpenSearch) |
| **Wazuh Dashboard** | Interfaz web de visualización y gestión |
| **Wazuh Agent** | Recolector de datos instalado en cada endpoint |

## Requisitos del Sistema

- **SO**: Ubuntu Server 22.04 LTS
- **RAM**: Mínimo 4 GB (recomendado 8 GB)
- **Disco**: Mínimo 50 GB
- **Red**: Acceso a la LAN para recibir logs de los agentes

## Instalación

:::note
Wazuh recomienda su instalador asistido para entornos de producción. La instalación manual permite mayor control pero requiere más configuración.
:::

```bash
# Descargar el instalador asistido de Wazuh
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh

# Ejecutar la instalación todo-en-uno
sudo bash wazuh-install.sh -a
```

El instalador desplegará los tres componentes en una sola máquina.

## Post-Instalación

1. Acceder al Dashboard: `https://<IP_SERVIDOR>:443`
2. Credenciales por defecto proporcionadas por el instalador
3. **Cambiar la contraseña** del usuario admin inmediatamente
4. Configurar los agentes en los endpoints a monitorizar

## Registro de Agentes

```bash
# En el endpoint a monitorizar
curl -so wazuh-agent.deb https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.0-1_amd64.deb

sudo dpkg -i wazuh-agent.deb
sudo /var/ossec/bin/agent-auth -m <IP_MANAGER>
sudo systemctl start wazuh-agent
```

---

*Esta guía forma parte de la infraestructura de seguridad documentada en el [TFG](/docs/tfg/arquitectura-red/).*
