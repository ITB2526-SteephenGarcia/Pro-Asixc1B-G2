# 🏢 InnovateTech - Proyecto de Despliegue de CPD & Servicios en Red

¡Bienvenido al repositorio central de **InnovateTech**! Este proyecto detalla el diseño técnico, la infraestructura física/lógica y la implementación práctica de un Centro de Procesamiento de Datos (CPD) híbrido (físico y replicado en la nube AWS) optimizado para una empresa de 10 a 50 trabajadores con servicios de alta disponibilidad, bases de datos y streaming multimedia.

---

## 📌 Índice
1. [Diseño Físico e Infraestructura del CPD](#1-diseño-físico-e-infraestructura-del-cpd)
2. [Arquitectura de Servidores e IT](#2-arquitectura-de-servidores-e-it)
3. [Infraestructura Eléctrica y Climatización](#3-infraestructura-eléctrica-y-climatización)
4. [Seguridad Física y Lógica](#4-seguridad-física-y-lógica)
5. [Servicios de Red Implementados](#5-servicios-de-red-implementados)
6. [Capa de Datos y Automatizaciones (MariaDB)](#6-capa-de-datos-y-automatizaciones-mariadb)
7. [Tecnologías Utilizadas](#7-tecnologías-utilizadas)

---

## 1. Diseño Físico e Infraestructura del CPD

El CPD físico está proyectado en la **planta sótano (-1)** de la sede central en Barcelona. Esta ubicación proporciona aislamiento térmico natural, protección estructural de hormigón y nula exposición o identificación exterior.

* **Dimensiones:** Superficie total de 48 m² ($8\text{ m} \times 6\text{ m}$) con una altura útil de 2.5 m.
* **Distribución:** Dividido eficientemente en una **Zona de Racks** ($6\text{ m} \times 5\text{ m}$) con arquitectura de pasillo frío/caliente y una **Zona de Trabajo Técnico** ($6\text{ m} \times 1\text{ m}$).
* **Suelo y Techo Técnico:** Suelo elevado de 30 cm con placas de acero galvanizado (capacidad de carga de 1.200 kg/m²) para cableado y conductos de climatización. Techo técnico a 2.7 m para retorno de aire caliente e iluminación LED.

---

## 2. Arquitectura de Servidores e IT

La infraestructura cuenta con **6 servidores físicos Dell PowerEdge R350 (1U)** con procesadores Intel Xeon E-2336, configurados con **RAID 1 (Espejo)** para tolerancia a fallos de almacenamiento y gestión remota Out-of-Band mediante **iDRAC**.

### Distribución de Nodos
| Identificador | Modelo | RAM | Función Principal |
| :--- | :--- | :--- | :--- |
| **SRV-01** | Dell PowerEdge R350 | 32 GB ECC | Servidor Web (Nginx) + SFTP |
| **SRV-02** | Dell PowerEdge R350 | 32 GB ECC | Central de Usuarios LDAP |
| **SRV-03** | Dell PowerEdge R350 | 32 GB ECC | Servidor de Logs Centralizados (Rsyslog) |
| **SRV-04** | Dell PowerEdge R350 | 32 GB ECC | Motor de Base de Datos MariaDB |
| **SRV-05** | Dell PowerEdge R350 | 32 GB ECC | Streaming de Audio/Video (Icecast2 + Jellyfin) |
| **SRV-06** | Dell PowerEdge R350 | 16 GB ECC | Backups y Monitorización Centralizada (Zabbix) |

### Gestión de Conectividad (Cableado Estructurado)
El cableado sigue una estricta política de colores para facilitar el mantenimiento preventivo y evitar interferencias electromagnéticas (separación total de datos y alimentación):
* 🔵 **Azul:** Red de Datos Local (LAN / Cat.6A).
* 🟡 **Amarillo:** Conexiones de Gestión de Infraestructura (iDRAC / IPMI).
* 🔴 **Rojo:** Alimentación Eléctrica del Rack (PDU Circuito A) / Enlaces WAN.
* ⚫ **Negro:** Alimentación Eléctrica desde los SAIs (Circuito B).
* 🟢 **Verde:** Fibra Óptica para enlaces troncales (Uplinks).

---

## 3. Infraestructura Eléctrica y Climatización

### Climatización de Precisión (N+1)
Se utilizan **2 unidades STULZ CyberAir 3PRO** (1 activa + 1 en stand-by). 
* **Temperatura Objetivo:** 21°C (Rango óptimo de rendimiento energético y hardware).
* **Humedad:** 50% fija para mitigar tanto la electricidad estática como la condensación.
* **Filtrado:** Filtros de alta eficiencia **HEPA H13** (retención del 99,95% de partículas).

### Sistema Eléctrico Redundante y Autonomía
Cada servidor cuenta con **doble fuente de alimentación (Dual PSU)** mapeadas a dos circuitos eléctricos independientes (Circuito A y Circuito B).
* **SAIs Seleccionados:** 2× APC Smart-UPS SRT 3000VA / 2700W.
* **Autonomía calculada:** **20-24 minutos** operando al 48% de la carga máxima estimada ($2.575\text{ W} \times 1.2 = 3.090\text{ W}$), permitiendo un apagado ordenado automatizado vía comandos SNMP corporativos en caso de caída del suministro.

---

## 4. Seguridad Física y Lógica

### Seguridad Física (Defensa en Capas)
1.  **Control de Acceso de 3 Factores:** Tarjeta RFID individual $\rightarrow$ Código PIN de 6 dígitos $\rightarrow$ Exclusa física (Mantrap) anti-tailgating.
2.  **Videovigilancia:** 4 cámaras IP Axis P3245-V con grabación continua 24/7 en resolución 1080p, aisladas en una VLAN exclusiva de seguridad.
3.  **Protección contra Incendios:** Detección óptica de humo Siemens y extinción automática mediante **Gas Novec 1230** (Inocuo para componentes electrónicos y operarios).

### Seguridad Lógica (Bastionado/Hardening)
* **SSH Hardening:** Acceso root deshabilitado (`PermitRootLogin no`), autenticación obligatoria por llave pública/privada sin contraseñas (`PasswordAuthentication no`) y límite estricto de 3 intentos de conexión (`MaxAuthTries 3`).
* **Firewalling:** Implementación perimetral con **pfSense** aplicando políticas implícitas *Whitelist* (Deny All por defecto) combinado con cortafuegos locales (`ufw`/`iptables`) por host.
* **Políticas de Respaldo 3-2-1:** Gestión automatizada de copias incrementales diarias y completas semanales replicadas localmente en NAS y de forma externa en la nube (**AWS S3**).

---

## 5. Servicios de Red Implementados

### 🌐 Hub de Gestión Web + Autenticación Centralizada
* Despliegue automatizado de un portal web corporativo dinámico en PHP y Nginx mediante **Ansible Playbooks**.
* **Integración LDAP:** Autenticación unificada de usuarios técnicos a través del servicio local `slapd`. Sincronización del almacenamiento masivo **SFTP Seguro (Puerto 2222)** validado contra los objetos del directorio LDAP corporativo (`nsswitch.conf`).

### 📺 Ecosistema Multimedia (Streaming de Alta Eficiencia)
* **Radio Corporativa (Audio):** Servidor de streaming de audio en vivo implementado con **Icecast2** y alimentado mediante **FFmpeg** automatizado para la difusión en bucle de listas de reproducción.
* **Plataforma de Video (VoD):** Implementación de **Jellyfin** utilizando segmentación de vídeo **HLS (HTTP Live Streaming)** en fragmentos `.ts` e índices `.m3u8` para garantizar una reproducción fluida e instantánea sin latencia.
* **Videoconferencia P2P:** Despliegue de un servidor **Jitsi Meet** nativo apoyado en el protocolo **WebRTC** con cifrado de extremo a extremo (DTLS y SRTP) utilizando códecs de última generación (Opus para audio, VP8/H.264 para vídeo).

> 📊 **Métricas de rendimiento en AWS (Stress Test):** Con todos los servicios multimedia multimedia corriendo en simultáneo, las pruebas de ancho de banda arrojaron una velocidad media de descarga de **1859 Mbps** y subida de **1662 Mbps** con una latencia de apenas **2.4 ms**, operando holgadamente a un **11%** de la capacidad total del enlace.

---

## 6. Capa de Datos y Automatizaciones (MariaDB)

La persistencia de la infraestructura utiliza un motor relacional MariaDB securizado, accesible de forma remota únicamente desde el segmento de la red LAN autorizada (puerto 3306 abierto con `bind-address = 0.0.0.0`).

### Mecanismos de Control e Integridad (Triggers & Events)
El sistema controla la lógica de negocio y audita la seguridad mediante gatillos en la base de datos:
1.  **Trigger 1:** Bloqueo automático de usuarios restringidos para realizar llamadas.
2.  **Trigger 2:** Control estricto y cuota de minutos mensuales consumidos.
3.  **Trigger 3:** Control y límite de llamadas diarias máximas por cuenta.
4.  **Trigger 4:** Auditoría de accesos e intentos no autorizados con volcado a tablas de log.
5.  **Trigger 5:** Restricción al rol de Administración para que no altere datos de videollamadas, desviando el aviso a una tabla de alertas.
6.  **Event Scheduler:** Automatización de backups periódicos internos ejecutados de forma programada dentro del motor de base de datos.

### 🤖 Bot de Alertas en Tiempo Real (Telegram Integración)
Se ha diseñado un script en Bash programado en el **Cron** del sistema (ejecución cada 5 minutos) que monitoriza la tabla de incidencias de la base de datos. Utiliza la herramienta `curl` y la API de **Telegram (BotFather)** para enviar notificaciones push instantáneas directamente al teléfono del administrador del CPD cuando ocurre una anomalía crítica en la infraestructura.

---

## 7. Tecnologías Utilizadas

* **Sistemas Operativos & Nube:** Linux (Ubuntu Server), AWS VPC (EC2, S3, Security Groups).
* **Automatización & Gestión:** Ansible (Playbooks), Crontab, Bash Scripting.
* **Servicios Web & Directorio:** Nginx, PHP, OpenLDAP (`slapd`), SFTP.
* **Multimedia & Streaming:** Icecast2, FFmpeg, Jellyfin (HLS), Jitsi Meet (WebRTC, DTLS, SRTP).
* **Bases de Datos:** MariaDB (SQL, Triggers, Events, Roles/Permisos).
* **Monitorización:** Zabbix (SNMP, Agents, NUT para SAIs).

---
_Proyecto desarrollado como propuesta integral de Fundamentos de Hardware y Servicios en Red - 2026._

[Ir al proyecto COMPLETO](pro-asixc1b-g2.md)

