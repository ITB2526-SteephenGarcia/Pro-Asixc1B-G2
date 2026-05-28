# Parte de fonaments del maquinari
## Proposta de CPD 


## 1. Ubicacion física


### 1.1 Situacion de la sala a l'edificio

InnovateTech tiene su sede en un edificio de oficinas urbano en Barcelona, de 4 plantas. El CPD se sitúa en la planta sótano (-1).
Justificación de la elección:
* El sótano es la planta con menor riesgo de identificación exterior: no hay ventanas, no es accesible directamente desde la calle y no hay tránsito de personal no autorizado.
* Estar bajo el nivel de la calle proporciona aislamiento térmico natural, reduciendo la carga de climatización.
* La estructura de hormigón de las paredes del sótano ofrece protección física adicional contra intrusiones, vibraciones y ruido exterior.
* Acceso exclusivo por una única puerta desde la escalera de emergencia, sin paso por el vestíbulo principal ni zonas de trabajo.

### Dimensiones y distribución de la sala

La sala del CPD tiene unas dimensiones de 8 m × 6 m = 48 m², con una altura libre de 3 m (descontando suelo y techo técnico, la altura útil es de unos 2,5 m).
La sala se divide en dos zonas diferenciadas:


| Zona | Dimensiones | Funciones |
| --- | --- | --- |
| Zona de racks (sala fría/caliente) | 6 m × 5 m | Aloja los 2 racks y los equipos IT |
| Zona de trabajo técnico | 6 m × 1 m | Mesa de trabajo para tareas de mantenimiento, acceso a los paneles eléctricos |



### 


### 1.2 Sistemas de climatización

Distribución de pasillos (hot aisle / cold aisle):
Se aplica el sistema de pasillo frío / pasillo caliente para optimizar la refrigeración:
* La parte frontal de los racks (donde se aspira el aire frío) se orienta hacia el pasillo frío, donde el climatizador impulsa aire frío.
* La parte posterior de los racks (donde se expulsa el aire caliente) se orienta hacia el pasillo caliente, desde donde el climatizador extrae y enfría el aire.
Equipo elegido: 2 × unidades de precisión STULZ CyberAir 3PRO
Se instalan 2 unidades de climatización de precisión (una activa + una en stand-by por redundancia N+1), situadas en la pared norte de la sala, retornando el aire caliente por el techo técnico.
Parámetros de funcionamiento:


| Parámetro | Rango recomendado | Valor objetivo |
| --- | --- | --- |
| Temperatura | 18°C – 27°C | 21°C |
| Filtrado del aire | Classe ISO 8 mínimo | Filtres HEPA H13 |


Justificación de los parámetros:
* 21°C es el punto óptimo para los servidores: suficientemente frío para evitar sobrecalentamiento pero no tan bajo que aumente innecesariamente el consumo energético.
* El 50% de humedad evita tanto la electricidad estática (humedad baja) como la condensación en los componentes (humedad alta).
* Los filtros HEPA H13 retienen el 99,95% de partículas ≥ 0,3 μm, protegiendo los componentes electrónicos del polvo.

Monitorización continua: sensores de temperatura y humedad en la entrada y salida de cada rack, conectados al sistema de monitorización central (Zabbix)
### 1.3 Medidas para dificultar la identificación de la sala:
* La puerta de acceso al CPD no tiene rótulo ni indicación visible. Exteriormente se identifica como "Sala Técnica ST-01" sin más detalles.
* No hay ventanas ni aperturas hacia el exterior.
* El ruido de los equipos de climatización queda aislado acústicamente mediante placas de material absorbente en las paredes, evitando que se perciba desde el pasillo.
* El sistema de control de acceso no lleva indicadores luminosos visibles desde fuera (no hay LED verde/rojo en el exterior).
* Los planos del edificio accesibles al público y al personal general no indican la ubicación exacta del CPD.

### 1.4 Distribución y gestión del cableado

Principios de gestión del cableado:

* Todo el cableado va etiquetado en ambos extremos con identificadores únicos (ej: `SW1-P01-SRV1`).
* Se utilizan colores estandarizados por tipo de cable:
  * **Azul:** red de datos (LAN)
  * **Amarillo:** conexiones de gestión (IPMI/iDRAC)
  * **Rojo:** alimentación eléctrica de los racks (PDU)
  * **Negro:** alimentación de los SAIs
  * **Verde:** fibra óptica (uplinks entre switches)
* El cableado de datos y el de alimentación eléctrica nunca se cruzan ni van juntos en el mismo canal, para evitar interferencias electromagnéticas.
* Dentro de los racks se utilizan organizadores de cable horizontales (1U entre cada patch panel) y pasacables verticales en los laterales del rack.
* Todo el cableado horizontal (entre racks y equipos) va por debajo del suelo técnico en canales de PVC cerrados.

### 


### 1.5 Suelo técnico y techo técnico

Suelo técnico:
* Se instala un suelo técnico elevado de 30 cm sobre el suelo estructural del edificio.
* Las placas son de 60 × 60 cm, de acero galvanizado con acabado antideslizante, con una capacidad de carga de 1.200 kg/m².
* Por debajo del suelo técnico circulan:
  * Cableado de red (cat.6A y fibra óptica)
  * Cableado de alimentación eléctrica
  * Conductos de impulsión de aire frío de los equipos de climatización (salidas de rejilla frente a cada rack)
Techo técnico:
* Se instala un techo técnico a 2,7 m de altura (dejando 30 cm libres hasta el techo estructural de 3 m).
* Por encima del techo técnico circulan:
  * Conductos de retorno de aire caliente hacia los equipos de climatización
  * Cableado de seguridad (cámaras, sensores de incendio)
  * Iluminación LED de la sala (6 paneles de 60W cada uno)

### 1.6 Estructuras y planos
Boceto del sotano:

![Imagen 1](images/image_1.png)

Imagen Generada de como lo queremos:

![Imagen 2](images/image_2.png)

# 2. Infraestructura IT

### 2.1 Servidores

Para una empresa de 10-50 trabajadores con servicios de streaming, base de datos y servicios web, se proponen 4 servidores físicos en formato rack (1U/2U):


|  | Modelo | Formato<br><br> | CPU<br><br> | RAM | Almacenamiento | Función |
| --- | --- | --- | --- | --- | --- | --- |
| SRV-01 | Dell PowerEdge R350 | 1U | Intel Xeon E-2336 (6C/12T) | 32 GB ECC | 2× SSD 480 GB (RAID 1) | Servidor web + SFTP |
| SRV-02 | Dell PowerEdge R350 | 1U | Intel Xeon E-2336 (6C/12T) | 32 GB ECC | 2× SSD 480 GB (RAID 1) | LDAP |
| SRV-03 | Dell PowerEdge R350 | 1U | Intel Xeon E-2336 (6C/12T) | 32 GB ECC | 2× SSD 480 GB (RAID 1) | Logs centralitzats |
| SRV-04 | Dell PowerEdge R350 | 1U | Intel Xeon E-2336 (6C/12T) | 32 GB ECC | 2× SSD 480 GB (RAID 1) | Base de dades |
| SRV-05 | Dell PowerEdge R350 | 1U | Intel Xeon E-2336 (6C/12T) | 32 GB ECC | 2× SSD 480 GB (RAID 1) | Streaming audio/video (icecast) |
| SRV-06<br><br> | Dell PowerEdge R350 | 1U | Intel Xeon E-2336 (6C/12T) | 16 GB ECC | 2× SSD 480 GB (RAID 1) | Backup i monitorització |


Justificación de la elección Dell PowerEdge:
* Garantía 3 años ProSupport con sustitución al día siguiente.
* Gestión remota vía iDRAC (permite reiniciar, acceder a BIOS y monitorizar sin acceso físico).
* Ampliamente documentados y compatibles con Linux.

### 


### 2.2 Patch Panels

Se instalan 2 patch panels de 24 puertos Cat.6A (uno por rack), situados en la parte superior de cada rack:


| Elemento | Posición en el rack | Descripción |
| --- | --- | --- |
| PP-R1 | Rack 1, posición 1U | Patch Panel 24p Cat.6A — conexiones servidores SRV-01, SRV-02 |
| PP-R2 | Rack 2, posición 1U | Patch Panel 24p Cat.6A — conexiones servidores SRV-03, SRV-04 i SAIs |



### 2.3 Switches



| Elemento | Modelo | Puertos | Posición | Función |
| --- | --- | --- | --- | --- |
| SW-CORE | Cisco Catalyst 2960-X 24TS | 24× GbE + 2× SFP | Rack 1, 2U | Switch principal de la LAN interna |
| SW-MGT | TP-Link TL-SG108E | 8× GbE | Rack 2, 1U | Switch de gestión (red de management iDRAC) |


* El SW-CORE interconecta todos los servidores y proporciona el uplink hacia el router/firewall de la empresa.
* El SW-MGT es una red de gestión aislada (out-of-band management), permitiendo acceder a los iDRAC de los servidores incluso si la red de producción cae.

### 2.4 Distribución de los racks

Se instalan 2 racks de 24U (APC NetShelter SX 24U), situados en paralelo siguiendo el sistema pasillo frío/caliente.

![Imagen 3](images/image_3.png)


## 


## 3. Infraestructura eléctrica


### 3.1 Alimentación redundante

El CPD dispone de dos circuitos eléctricos independientes procedentes de dos cuadros eléctricos diferenciados del edificio:
* Circuito A: alimenta PDU-A de cada rack (fuente de alimentación 1 de cada servidor).
* Circuito B: alimenta PDU-B de cada rack (fuente de alimentación 2 de cada servidor).

Todos los servidores elegidos disponen de fuentes de alimentación redundantes (dual PSU), de modo que si un circuito cae, el servidor continúa funcionando por el circuito restante sin interrupción.

![Imagen 4](images/image_4.png)


### 3.2 SAIs — Cálculo de autonomía

Consumo estimado de los equipos:


| Equipo | Consumo máximo |
| --- | --- |
| SRV-01 (Dell R350) | 350 W |
| SRV-02 (Dell R350) | 350 W |
| SRV-03 (Dell R450) | 550 W |
| SRV-04 (Dell R350) | 350 W |
| SRV-05 (Dell R350) | 350 W |
| SRV-06 (Dell R350) | 350 W |
| SW-CORE (Cisco 2960-X) | 65 W |
| SW-MGT (TP-Link) | 10 W |
| 2× Climatizadores (consum parcial) | 400 W |
| TOTAL | 2.575 W |


Aplicant un factor de seguretat del 20%: 2.575 × 1,20 = 3.090 W ≈ 3,1 kW
SAI escollit: 2× APC Smart-UPS SRT 3000VA / 2700W
* Capacidad por SAI: 2.700 W
* Cada SAI se encarga de un circuito (A o B).
* Cada servidor tiene una PSU conectada a cada SAI, de modo que ambos SAIs soportan la mitad de la carga total.0
  
#### Cálculo de autonomía:

Con el 48% de la carga por SAI (≈ 1.288 W) y las baterías del APC SRT3000:

* Autonomía estimada: 20-24 minutos con carga al 48%.
  
Este tiempo es suficiente para:
1. Que el generador del edificio (si existe) arranque.
1. Que el personal técnico inicie un apagado ordenado de los servidores vía scripts automatizados (el SAI notifica por SNMP).
1. Evitar pérdida de datos por corte repentino.

## 4. Seguridad física y lógica


### 4.1 Seguridad física

#### control de acceso

El CPD implementa un sistema de tres factores de autenticación por capas:


| Capa | Elemento | Descripción |
| --- | --- | --- |
| 1a | Tarjeta RFID | Todas las personas autorizadas disponen de una tarjeta de proximidad personal e intransferible |
| 2a | Código PIN | Además de la tarjeta, hay que introducir un PIN de 6 dígitos en un teclado numérico |
| 3a | Mantrap / esclusa | Puerta doble: la primera debe cerrarse antes de que se abra la segunda. Impide el acceso de personas no autorizadas aprovechando el acceso de una autorizada (tailgating) |


* El sistema registra fecha, hora e identidad de cada acceso e intento de acceso fallido.
* Fuera del horario laboral (20h – 8h), se activa un nivel de alerta adicional: cualquier acceso genera una notificación inmediata al responsable de seguridad.
* La lista de personas autorizadas se revisa trimestralmente.

#### Videovigilancia

Se instalan 4 cámaras IP de seguridad (Axis P3245-V):

| Cámara | Posición | Cobertura |
| --- | --- | --- |
| CAM-01 | Techo, frente a la puerta de acceso | Entrada principal, zona del mantrap |
| CAM-02 | Techo, esquina NE | Pasillo frío, frontal Rack 1 y Rack 2 |
| CAM-03 | Techo, esquina SO | Pasillo caliente, parte posterior de los racks |
| CAM-04 | Techo, zona de trabajo | Mesa de trabajo y cuadro eléctrico |


* Grabación continua 24/7 en resolución 1080p.
* Retención de 30 días en un NAS situado en el Rack 2 (SRV-06).
* Las cámaras funcionan en una red aislada (VLAN de seguridad), separada de la red de datos.
#### Sistemas de prevención, detección y extinción de incendios 
#### Detección:
* Detectores de humo ópticos (Siemens FDO221) en cada rincón de la sala y dentro de cada rack: total 6 detectores.
* Detectores de calor (activación por temperatura > 57°C) en el techo técnico: 2 detectores.
* Sistema conectado a la central de alarmas del edificio con aviso automático a los bomberos.
  
#### Extinción:
* Sistema de extinción por gas Novec 1230 (FK-5-1-12): no conductor, no corrosivo, no daña los equipos electrónicos y es seguro para las personas.
* No se utilizan sistemas de extinción por agua (sprinklers) para evitar daños irreversibles en los equipos.
* El gas se almacena en 2 cilindros situados en el exterior de la sala (pasillo), con tuberías hasta los difusores interiores.
* Antes de la activación del gas: señal acústica de 30 segundos para permitir la evacuación del personal.

#### Vías de evacuación:
* La sala dispone de una única puerta de acceso, que es a la vez la vía de evacuación (se abre siempre hacia fuera en caso de emergencia, sin necesidad de código).
* Señalización luminosa de emergencia autónoma (batería propia) sobre la puerta.
* Plano de evacuación plastificado pegado en la pared interior, visible desde cualquier punto de la sala.

#### Planells:
Elementos de seguridad CPD                                            Planol evacuacion

![Imagen 5](images/image_5.png)

### 4.2 Seguridad lógica
#### Restricción de acceso por autorización
* Todos los servidores se acceden exclusivamente por SSH con clave pública/privada (sin contraseña).
* Se crea un usuario de administración específico (admin_innovate) en cada servidor. El usuario root tiene el acceso SSH deshabilitado.
* El fichero /etc/ssh/sshd_config configura: PermitRootLogin no, PasswordAuthentication no, MaxAuthTries 3.
* Acceso a las aplicaciones web y bases de datos gestionado por roles diferenciados (admin, ventas, administración, trabajador).
  
#### Firewalls
* Firewall perimetral: pfSense instalado en una máquina dedicada (o como VM en el SRV-06), protegiendo el CPD del acceso no autorizado desde la red de la empresa e Internet.
* Reglas básicas: whitelist por defecto (todo bloqueado excepto lo que se abre explícitamente).
* Firewall de host (ufw / iptables) activado en cada servidor, permitiendo únicamente los puertos necesarios para cada servicio.
* Puertos abiertos por defecto en cada servidor: únicamente SSH (22)
  
#### Monitorización

Zabbix instalado en el SRV-06 para monitorizar:

* Uso de CPU, RAM y disco de todos los servidores.
* Temperatura de los servidores (vía SNMP/iDRAC).
* Estado de los servicios (web, LDAP, logs, BD,streaming).
* Nivel de carga de los SAIs (vía NUT - Network UPS Tools).
* Alertas por correo electrónico automáticas si algún servicio cae o si la temperatura supera los 25°C.
  
#### Copias de seguridad / Backups
### Política de backups 3-2-1:

* 3 copias de los datos.
* En 2 soportes diferentes (disco local + NAS).
* 1 copia fuera del CPD (copia en la nube AWS S3).


| Tipus | Freqüència | Horari | Retenció |
| --- | --- | --- | --- |
| Backup incremental | Diario | 02:00h | 7 días |
| Backup completo | Semanal | Viernes 03:00h | 4 semanas |
| Backup a la nube (S3) | Semanal | Viernes 04:00h | 3 mesos |




| Servidor | Configuración RAID | Justificación |
| --- | --- | --- |
| SRV-01 | RAID 1 (espejo) | Redundancia simple para servicios web y SFTP |
| SRV-02 | RAID 1 (espejo) | Redundancia para LDAP |
| SRV-03 | RAID 1 (espejo) | Redundancia para logs centralizados |
| SRV-04 | RAID 1 (espejo) | Redundancia para base de datos |
| SRV-05 | RAID 1 (espejo) | Redundancia para streaming Icecast |
| SRV-06 | RAID 1 (espejo) | Redundancia para backup y monitorización |



### 


### 


### 


### 4.3 Prevención de riesgos laborales (PRL)



| Riesgo | Medida aplicada |
| --- | --- |
| Riesgo eléctrico | Todos los cables y conexiones cumplen la normativa IEC 60364. Cuadro eléctrico con disyuntores y diferencial. Prohibido trabajar en tensión sin EPI adecuado (guantes aislantes clase 0). |
| Ruido | Los ventiladores de los servidores generan 65-75 dB. El personal que trabaje en la sala más de 30 minutos debe usar protección auditiva (orejeras SNR 25 dB como mínimo). |
| Cop/atrapamiento | Las puertas de los racks deben cerrarse siempre que no se esté trabajando activamente. Las PDUs y cables en el suelo están protegidos bajo el suelo técnico. |
| Iluminación | Mínima de 500 lux en la zona de trabajo (normativa UNE-EN 12464-1). Iluminación de emergencia autónoma de 100 lux activada automáticamente en caso de corte eléctrico. |
| Calidad del aire | El sistema de climatización filtra y renueva el aire continuamente. Prohibido comer, beber o fumar dentro del CPD (riesgo de contaminación de los equipos). |
| Ergonomía | La mesa de trabajo tiene altura regulable. Las tareas de rack (instalar/retirar equipos pesados) requieren dos operarios y el uso de soporte de rack extraíble para equipos de más de 15 kg. |
| Señalética | Señales de: riesgo eléctrico, riesgo de fuego (extinción por gas), EPI obligatorio, salida de emergencia, prohibido comer. Todas en castellano y pictograma ISO 7010. |


Implementació del CPD al núvol AWS

![Imagen 6](images/image_6.png)


![Imagen 7](images/image_7.png)


![Imagen 8](images/image_8.png)


![Imagen 9](images/image_9.png)


![Imagen 10](images/image_10.png)

# Server LOGS
Pasos previs:

![Imagen 11](images/image_11.png)

Creació d’usuaris

![Imagen 12](images/image_12.png)

Copia de la clau al nou usuari

![Imagen 13](images/image_13.png)


![Imagen 14](images/image_14.png)

Instalació y configuració amb Ansible de la gestió de Logs (rsyslog).

![Imagen 15](images/image_15.png)


![Imagen 16](images/image_16.png)


![Imagen 17](images/image_17.png)

Carpeta de Logs de tots els servidores:

![Imagen 18](images/image_18.png)

Logs en tiempo real de el servidor 172.31.47.154

![Imagen 19](images/image_19.png)

Instalació client LDAP

![Imagen 20](images/image_20.png)


![Imagen 21](images/image_21.png)


![Imagen 22](images/image_22.png)


![Imagen 23](images/image_23.png)

# SERVER WEB + SFTP
Crear el usuario admin_innovate para no usar el default (root)

![Imagen 24](images/image_24.png)

Darle permisos de administrador sin contraseña

![Imagen 25](images/image_25.png)

Crear la carpeta de llaves

![Imagen 26](images/image_26.png)

Meter las 4 llaves de cada server que se consiguio mediante cat

![Imagen 27](images/image_27.png)

Permisos para que solo el dueño “admin_innovate” pueda leer estas llaves

![Imagen 28](images/image_28.png)

ssh al user admin_innovate para no usar el root

![Imagen 29](images/image_29.png)

Cat para demostar las llaves en este nuevo user

![Imagen 30](images/image_30.png)

Conectar con los logs de mi compañero: sudo nano /etc/rsyslog.d/

![Imagen 31](images/image_31.png)


![Imagen 32](images/image_32.png)

Restart para que le aparezca los logs y status para comprovar el estado

![Imagen 33](images/image_33.png)


![Imagen 34](images/image_34.png)

Montar server web con nginx meidante ansible: sudo apt install -y ansible

![Imagen 35](images/image_35.png)

Crear el archivo con el código (Playbook)

![Imagen 36](images/image_36.png)


![Imagen 37](images/image_37.png)

Activamos ansible mediante : sudo ansible-playbook -c local -i "localhost," configurar_web_sftp.yml

![Imagen 38](images/image_38.png)

Permitir puerto del sftp: sudo ufw allow 2222/tcp

![Imagen 39](images/image_39.png)

Comprovacion del funcionamiento de nginx

![Imagen 40](images/image_40.png)

Comprovacion del funcionamiento de sftp : sftp -P 2222 clienteweb@54.235.232.168

![Imagen 41](images/image_41.png)

Diseño de la web

![Imagen 42](images/image_42.png)


![Imagen 43](images/image_43.png)

```php
session_start();
// --- CONFIGURACIÓN DE INFRAESTRUCTURA REAL (INNOVATE TECH) ---
define('LDAP_SERVER', '172.31.16.181');
define('LDAP_BASE_DN', 'dc=innovatetech,dc=local');
define('LDAP_OU', 'ou=users');
define('DB_HOST', '172.31.16.37');
define('DB_USER', 'web_user');          // Asegúrate de crear este usuario en MariaDB
define('DB_PASS', 'pirineus'); // Contraseña asignada en la BBDD
define('DB_NAME', 'innovatetech');
$login_error = "";
$current_tab = isset($_GET['tab']) ? $_GET['tab'] : 'dashboard';
// 1. CONTROL DE AUTENTICACIÓN LDAP
if ($_SERVER['REQUEST_METHOD'] == 'POST' && isset($_POST['action']) && $_POST['action'] == 'login') {
$username = filter_input(INPUT_POST, 'username', FILTER_SANITIZE_STRING);
$password = $_POST['password'];
$ldap_conn = ldap_connect(LDAP_SERVER);
if ($ldap_conn) {
ldap_set_option($ldap_conn, LDAP_OPT_PROTOCOL_VERSION, 3);
// Construcción del DN usando UID según vuestro ldapsearch
$user_dn = "uid=" . $username . "," . LDAP_OU . "," . LDAP_BASE_DN;
// Intento de Bind (Autenticación)
$ldap_bind = @ldap_bind($ldap_conn, $user_dn, $password);
if ($ldap_bind) {
$_SESSION['authenticated'] = true;
$_SESSION['username'] = $username;
header("Location: index.php?tab=dashboard");
exit;
} else {
$login_error = "Credenciales LDAP incorrectas o usuario inexistente.";
}
ldap_close($ldap_conn);
} else {
$login_error = "Error de red: Imposible conectar con Servidor LDAP (172.31.16.181).";
}
}
// 2. LOGOUT
if (isset($_GET['action']) && $_GET['action'] == 'logout') {
session_destroy();
header("Location: index.php");
exit;
}
// Bloqueo global de navegación si no hay sesión activa
if (!isset($_SESSION['authenticated']) && $current_tab !== 'login') {
$current_tab = 'login';
}
// 3. EXTRACCIÓN DE DATOS DE BBDD (Pestaña Auditoría/Backups)
$backup_logs = [];
$db_connected = false;
if (isset($_SESSION['authenticated']) && $current_tab == 'auditoria') {
$conn = @new mysqli(DB_HOST, DB_USER, DB_PASS, DB_NAME);
if (!$conn->connect_error) {
$db_connected = true;
$result = $conn->query("SELECT id_backup, data_hora, taules_incloses, ruta_fitxer, resultat, notes FROM backup_log ORDER BY data_hora DESC LIMIT 8");
if ($result) {
while ($row = $result->fetch_assoc()) {
$backup_logs[] = $row;
}
}
$conn->close();
} else {
$db_error_msg = "Error de conexión MariaDB (172.31.16.37): " . $conn->connect_error;
}
}
?>
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Innovate Tech - Portal de Gestión CPD</title>
<style>
:root {
--bg-color: #0d1117;
--card-bg: #161b22;
--text-main: #c9d1d9;
--lan-blue: #58a6ff;     /* Código azul: Datos */
--mgt-yellow: #f2cc60;   /* Código amarillo: Gestión */
--alert-red: #f85149;    /* Código rojo: Alertas */
--border-color: #30363d;
}
body {
margin: 0;
font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
background-color: var(--bg-color);
color: var(--text-main);
}
header {
background-color: var(--card-bg);
border-bottom: 1px solid var(--border-color);
padding: 15px 30px;
display: flex;
justify-content: space-between;
align-items: center;
}
header h1 { margin: 0; font-size: 1.5rem; color: #fff; }
header p { margin: 0; font-size: 0.9rem; color: var(--lan-blue); }
.nav-tabs {
display: flex;
background-color: #21262d;
border-bottom: 1px solid var(--border-color);
padding: 0 20px;
}
.nav-tabs a {
padding: 15px 20px;
color: var(--text-main);
text-decoration: none;
font-weight: 500;
border-bottom: 2px solid transparent;
transition: all 0.2s;
}
.nav-tabs a:hover { color: #fff; }
.nav-tabs a.active {
color: var(--lan-blue);
border-bottom: 2px solid var(--lan-blue);
background-color: var(--card-bg);
}
.container { max-width: 1200px; margin: 30px auto; padding: 0 20px; }
.card {
background-color: var(--card-bg);
border: 1px solid var(--border-color);
border-radius: 6px;
padding: 25px;
margin-bottom: 25px;
}
.grid-dashboard {
display: grid;
grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
gap: 20px;
}
.status-badge {
display: inline-block;
padding: 4px 8px;
border-radius: 4px;
font-size: 0.8rem;
font-weight: bold;
}
.status-online { background: rgba(56, 139, 253, 0.15); color: var(--lan-blue); }
.status-offline { background: rgba(248, 81, 73, 0.15); color: var(--alert-red); }
/* Estilos de tablas */
table { width: 100%; border-collapse: collapse; margin-top: 15px; }
th, td { padding: 12px; border-bottom: 1px solid var(--border-color); text-align: left; }
th { background-color: #1f242c; color: var(--lan-blue); }
/* Formularios y Botones */
.form-group { margin-bottom: 15px; }
label { display: block; margin-bottom: 5px; font-weight: bold; }
input[type="text"], input[type="password"] {
width: 100%; padding: 10px; background: #0d1117; border: 1px solid var(--border-color);
color: #fff; border-radius: 4px; box-sizing: border-box;
}
.btn {
display: inline-block; padding: 10px 25px; background-color: var(--lan-blue);
color: #fff; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; text-decoration: none;
}
.btn:hover { background-color: #388bfd; }
.btn-secondary { background: transparent; border: 1px solid var(--border-color); color: var(--text-main); }
.btn-secondary:hover { background: var(--border-color); color: #fff; }
</style>
</head>
<body>
<header>
<div>
<h1>INNOVATE TECH | CPD Hub</h1>
<p>Infraestructura Interna Sótano (-1)</p>
</div>
<?php if (isset($_SESSION['authenticated'])): ?>
<div style="font-size: 0.9rem;">
<span>SysOp: <strong style="color: var(--mgt-yellow);"><?php echo htmlspecialchars($_SESSION['username']); ?></strong></span> |
<a href="?action=logout" style="color: var(--alert-red); text-decoration: none;">Cerrar Sesión</a>
</div>
<?php endif; ?>
</header>
<?php if (isset($_SESSION['authenticated'])): ?>
<div class="nav-tabs">
<a href="?tab=dashboard" class="<?php echo $current_tab == 'dashboard' ? 'active' : ''; ?>">🏠 Inicio</a>
<a href="?tab=multimedia" class="<?php echo $current_tab == 'multimedia' ? 'active' : ''; ?>">📺 Multimedia</a>
<a href="?tab=auditoria" class="<?php echo $current_tab == 'auditoria' ? 'active' : ''; ?>">🗄️ BBDD & Almacén</a>
<a href="?tab=zabbix" class="<?php echo $current_tab == 'zabbix' ? 'active' : ''; ?>">📊 Monitorización</a>
<a href="?tab=infraestructura" class="<?php echo $current_tab == 'infraestructura' ? 'active' : ''; ?>">⚙️ Infraestructura</a>
</div>
<?php endif; ?>
<div class="container">
<?php if ($current_tab == 'login'): ?>
<div class="card" style="max-width: 420px; margin: 60px auto;">
<h2 style="margin-top:0; text-align: center;">🔐 Autenticación LDAP</h2>
<p style="text-align: center; color: #8b949e; font-size: 0.9rem;">Acceso exclusivo para personal técnico de Innovate Tech.</p>
<?php if (!empty($login_error)): ?>
<div style="background: rgba(248,81,73,0.1); color: var(--alert-red); padding: 10px; border-radius: 4px; margin-bottom: 15px; border: 1px solid rgba(248,81,73,0.2);">
<?php echo $login_error; ?>
</div>
<?php endif; ?>
<form method="POST" action="">
<input type="hidden" name="action" value="login">
<div class="form-group">
<label>UID de Usuario:</label>
<input type="text" name="username" required placeholder="ej. innovatetechadmin">
</div>
<div class="form-group">
<label>Contraseña Corporativa:</label>
<input type="password" name="password" required placeholder="••••••••">
</div>
<button type="submit" class="btn" style="width: 100%; margin-top: 10px;">Validar Identidad</button>
</form>
</div>
<?php endif; ?>
<?php if ($current_tab == 'dashboard'): ?>
<div class="card">
<h2>Bienvenido al Hub Central del CPD</h2>
<p>Desde este portal unificado gestionas los servidores replicados en la infraestructura en la nube de la compañía.</p>
</div>
<h3 style="color: var(--lan-blue);">Ecosistema de Nodos y Servicios</h3>
<div class="grid-dashboard">
<div class="card">
<h4>SERVER WEB (SRV-01)</h4>
<p style="font-size: 0.85rem; color: #8b949e;">IP Privada: 172.31.47.154<br>IP Elástica: 100.55.210.86</p>
<span class="status-badge status-online">Online (Host Activo)</span>
</div>
<div class="card">
<h4>SERVER LDAP (SRV-02)</h4>
<p style="font-size: 0.85rem; color: #8b949e;">IP Privada: 172.31.16.181<br>Servicio: Central de Usuarios</p>
<span class="status-badge status-online">Online (VPC Link)</span>
</div>
<div class="card">
<h4>SERVER BBDD (SRV-03)</h4>
<p style="font-size: 0.85rem; color: #8b949e;">IP Privada: 172.31.16.37<br>Motor: MariaDB Clúster</p>
<span class="status-badge status-online">Online (Replicado)</span>
</div>
<div class="card">
<h4>SERVER STREAMING (SRV-05)</h4>
<p style="font-size: 0.85rem; color: #8b949e;">IP Privada: Interna VPC<br>Pública: 34.205.94.36</p>
<span class="status-badge status-online">Online (Multimedia)</span>
</div>
</div>
<?php endif; ?>
<?php if ($current_tab == 'multimedia'): ?>
<div class="grid-dashboard">
<div class="card">
<h3>📻 Radio Corporativa en Directo</h3>
<p style="color: #8b949e;">Streaming de audio en directo y listado de reproducción actual directo desde el servidor multimedia.</p>
<div style="border: 1px solid var(--border-color); border-radius: 6px; overflow: hidden; background: #21262d; height: 350px; margin-top: 15px;">
<iframe src="http:
</div>
</div>
<div class="card" style="display:flex; flex-direction:column; justify-content: space-between;">
<div>
<h3>🎬 Plataforma de Vídeo (VoD)</h3>
<p style="color: #8b949e;">Servidor bajo demanda Jellyfin. Transmisión optimizada en alta definición mediante segmentación HLS para evitar la latencia de red.</p>
</div>
<a href="/jellyfin/" target="_blank" class="btn" style="text-align:center;">Abrir Jellyfin Portal</a>
</div>
</div>
<div class="card" style="margin-top:25px;">
<h3>🤝 Sala de Videoconferencia P2P</h3>
<p style="color: #8b949e;">Comunicaciones seguras inter-departamentales cifradas de extremo a extremo mediante protocolos DTLS y SRTP.</p>
<div style="display:flex; gap: 10px; margin-top:15px;">
<input type="text" id="roomName" placeholder="Nombre de la sala (ej: reunion-it)" style="max-width:300px;">
<button onclick="launchJitsi()" class="btn">Crear / Unirse a Sala</button>
</div>
<script>
function launchJitsi() {
var room = document.getElementById('roomName').value;
if(room) {
window.open('http://34.205.94.36/index.html', '_blank');
} else { alert('Por favor, introduce un nombre para la sala.'); }
}
</script>
</div>
<?php endif; ?>
<?php if ($current_tab == 'auditoria'): ?>
<div class="card">
<h3>🗄️ Historial de Backups Corporativos (MariaDB)</h3>
<p style="color: #8b949e;">Lectura de auditoría automatizada en tiempo real desde el nodo 172.31.16.37 base de datos: <code>innovatetech</code>.</p>
<?php if (isset($db_error_msg)): ?>
<div style="color: var(--alert-red); background: rgba(248,81,73,0.1); padding: 12px; border-radius: 4px;"><?php echo $db_error_msg; ?></div>
<?php else: ?>
<table>
<thead>
<tr>
<th>ID Backup</th>
<th>Fecha y Hora</th>
<th>Tablas Afectadas</th>
<th>Ruta del Backup (.sql)</th>
<th>Estado</th>
</tr>
</thead>
<tbody>
<?php if (empty($backup_logs)): ?>
<tr><td colspan="5" style="text-align:center; color: var(--mgt-yellow);">No hay registros en la tabla 'backup_log'.</td></tr>
<?php else: ?>
<?php foreach ($backup_logs as $log): ?>
<tr>
<td><code>#<?php echo $log['id_backup']; ?></code></td>
<td><?php echo $log['data_hora']; ?></td>
<td><span style="color:#8b949e; font-size:0.9rem;"><?php echo htmlspecialchars($log['taules_incloses']); ?></span></td>
<td><code><?php echo htmlspecialchars($log['ruta_fitxer']); ?></code></td>
<td>
<span class="status-badge <?php echo $log['resultat'] == 'ok' ? 'status-online' : 'status-offline'; ?>">
<?php echo strtoupper($log['resultat']); ?>
</span>
</td>
</tr>
<?php endforeach; ?>
<?php endif; ?>
</tbody>
</table>
<?php endif; ?>
</div>
<div class="card">
<h3>📁 Conexión al Servidor de Archivos (SFTP Seguro)</h3>
<p>El almacenamiento masivo está integrado de manera lógica con vuestras identidades LDAP en el servidor principal.</p>
<div style="background: #0d1117; padding: 15px; border-radius: 6px; border:1px solid var(--border-color); font-family: monospace;">
<strong>Protocolo / Servidor:</strong> sftp:
<strong>Puerto de Enlace:</strong> 22 <br>
<strong>Autenticación:</strong> Credenciales LDAP Corporativas (MaxAuthTries limitado por seguridad)
</div>
</div>
<?php endif; ?>
<?php if ($current_tab == 'zabbix'): ?>
<div class="card">
<h3>📊 Monitorización de Infraestructura Zabbix (172.31.15.83)</h3>
<p style="color: #8b949e; margin-bottom: 20px;">Vigilancia continua de consumo energético de SAIs mediante NUT, carga de CPUs de la VPC y monitorización de ancho de banda.</p>
<a href="http://54.161.84.211:8080/" target="_blank" class="btn" style="margin-bottom: 15px;">Abrir Consola Completa de Zabbix</a>
<div style="border: 1px solid var(--border-color); border-radius: 6px; overflow: hidden; background: #fff; height: 600px;">
<iframe src="http://100.55.210.86:8085/" style="width: 100%; height: 100%; border: none;"></iframe>
</div>
</div>
<?php endif; ?>
<?php if ($current_tab == 'infraestructura'): ?>
<div class="card">
<h3>⚙️ Especificaciones de Diseño del CPD (Planta -1)</h3>
<p>Guía de referencia rápida para operaciones físicas e intervenciones de mantenimiento en el rack de servidores.</p>
<h4 style="color: var(--mgt-yellow); margin-top:20px;">Código de Colores de Cableado Estructurado</h4>
<table>
<thead>
<tr><th>Tipo de Red / Servicio</th><th>Color del Latiguillo (UTP Cat6)</th><th>Ubicación Física</th></tr>
</thead>
<tbody>
<tr><td>Red de Datos Local (LAN)</td><td style="color: #58a6ff; font-weight:bold;">Azul</td><td>Pasillo Frío / Switches Core</td></tr>
<tr><td>Líneas de Gestión (iDRAC / IPMI)</td><td style="color: #f2cc60; font-weight:bold;">Amarillo</td><td>Switch de Gestión Superior</td></tr>
<tr><td>Enlaces WAN / Proveedor de Fibra</td><td style="color: #ff7b72; font-weight:bold;">Rojo</td><td>Entrada de Conexión del pfSense</td></tr>
</tbody>
</table>
<h4 style="color: var(--lan-blue); margin-top:30px;">Pruebas de Rendimiento de Red (AWS Speedtest)</h4>
<div style="background: #0d1117; padding: 15px; border-radius: 6px; border:1px solid var(--border-color); font-family: monospace;">
<strong>Velocidad de Bajada (Download):</strong> 1859.86 Mbit/s <br>
<strong>Velocidad de Subida (Upload):</strong> 1662.20 Mbit/s <br>
<strong>Latencia Media (Ping):</strong> 1.601 ms
</div>
</div>
<?php endif; ?>
</div>
<footer style="text-align: center; margin: 60px 0 20px 0; color: #8b949e; font-size: 0.85rem; border-top: 1px solid var(--border-color); padding-top: 15px;">
<p>© 2026 Innovate Tech S.A. | Todos los servicios están enrutados internamente mediante la VPC de AWS.</p>
</footer>
</body>
</html>

```
#### Diseño Final Web

Se autetifica mediante ldap

![Imagen 44](images/image_44.png)

Resumen de los servers alojados en la web

![Imagen 45](images/image_45.png)

Server multimedia (audio/video/streaming)

![Imagen 46](images/image_46.png)

Server de BBDD con MariaDB

![Imagen 47](images/image_47.png)

Server de monitorizacion con Zabbix

![Imagen 48](images/image_48.png)

Resumen de nuestros Racks

![Imagen 49](images/image_49.png)

Conexión con filezilla para administrar SFTP

![Imagen 50](images/image_50.png)


![Imagen 51](images/image_51.png)


![Imagen 52](images/image_52.png)

Demostración de clonozilla del sftp

![Imagen 53](images/image_53.png)

Hacer que el sftp se verifique mediante Ldap

![Imagen 54](images/image_54.png)

Se pone la ip privada del server ldap: ldap://172.31.16.181

![Imagen 55](images/image_55.png)

Se pone el dominio del ldap de mi compañero: dc=innovatetech,dc=local

![Imagen 56](images/image_56.png)

Versión a la que se configura: 3

![Imagen 57](images/image_57.png)


![Imagen 58](images/image_58.png)


![Imagen 59](images/image_59.png)

cuenta de ldap con root: cn=admin,dc=innovatetech,dc=local

![Imagen 60](images/image_60.png)


![Imagen 61](images/image_61.png)


![Imagen 62](images/image_62.png)

El archivo /etc/nsswitch.conf establece el orden de prioridad que sigue el sistema operativo para buscar y autenticar usuarios en Linux.  Asi que ponemos ldap para que valide mediante ldap

![Imagen 63](images/image_63.png)

Carpeta utilizada y restart del server

![Imagen 64](images/image_64.png)

Comprobación de la conexión con ldap

nc -zv 172.31.16.181 389: Comprueba la conectividad por red con el servidor LDAP de mi compañero. El mensaje succeeded! confirma que el puerto 389 (LDAP) está abierto y la máquina se comunica con la suya sin bloqueos.

![Imagen 65](images/image_65.png)

Verifica que la integración con LDAP funciona. Busca al usuario clienteweb tanto en los archivos locales como en el servidor remoto de LDAP y, al devolver su línea de información (UID, carpeta home, shell /bin/false), demuestra que el servidor ya lee correctamente los usuarios creados por mi compañero.

![Imagen 66](images/image_66.png)

ldapserach hacia la ip privada de mi compañero y el usuario clienteweb que creo para mi y verificar el sftp.

![Imagen 67](images/image_67.png)


![Imagen 68](images/image_68.png)

#### Conexion Server web + Sftp con BBDD

La persona que administra aws debe de activar el puerto de la base de datos 3306 TCP que pertenece a mariadb. Luego el encargado de BBDD cambia la carpeta sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf y poner:

bind-address = 0.0.0.0

![Imagen 69](images/image_69.png)

Con esto el server Web + Sftp tiene conexión con los demás servers. Resumen:

![Imagen 70](images/image_70.png)

#### Hacer segura el Server web + Sftp

Prohíbe por completo que el usuario root pueda iniciar sesión directamente desde fuera por SSH.

![Imagen 71](images/image_71.png)

Desactiva por completo el inicio de sesión mediante contraseñas escritas. A partir de ese momento, la única forma de entrar al servidor por SSH es usando una llave

![Imagen 72](images/image_72.png)

Limpia el número de intentos permitidos para iniciar sesión en una misma conexión. Si alguien se equivoca 3 veces seguidas al intentar autenticarse, el servidor le corta la conexión en la cara.

![Imagen 73](images/image_73.png)

Conexion Server web con zabbix

![Imagen 74](images/image_74.png)

Carpeta: sudo nano /etc/zabbix/zabbix_agentd.conf
El archivo configura el Zabbix Agent para recopilar métricas de rendimiento como el uso de CPU y RAM. Al definir Server y ServerActive con la IP 172.31.15.83, autorizo y vinculo el agente para que envíe los datos capturados al servidor central de Zabbix. Finalmente, el parámetro Hostname=SRV-WEB etiqueta el origen de la información para que aparezca identificado correctamente en el panel de monitorización.

![Imagen 75](images/image_75.png)


![Imagen 76](images/image_76.png)


![Imagen 77](images/image_77.png)

# SERVER LDAP
configuracion slapd

![Imagen 116](images/image_116.png)

comprovacio ldap

![Imagen 117](images/image_117.png)

estructura ldif, per a usuari server sftp i web

![Imagen 118](images/image_118.png)


![Imagen 119](images/image_119.png)

Conf estructura LDIF final amb usuari admin i usuari sftp i web

![Imagen 120](images/image_120.png)

Comprovacio i implamantacio en la base de dades de LDAP

![Imagen 121](images/image_121.png)

# SERVER BACKUP i Monitoratge
Instalació zabbix:

![Imagen 122](images/image_122.png)


![Imagen 123](images/image_123.png)


![Imagen 124](images/image_124.png)

CONTRASEÑA ZABBIX: InnovateZabbix2026

![Imagen 125](images/image_125.png)

Configuració de la web de zabbix

![Imagen 126](images/image_126.png)


![Imagen 127](images/image_127.png)


![Imagen 128](images/image_128.png)


![Imagen 129](images/image_129.png)

Agregar tots els servidors a zabbix per fer el monitoratge

![Imagen 130](images/image_130.png)

Creació de les carpetas per els diferents backups

![Imagen 131](images/image_131.png)

Pasar la clau privada a la maquina de backups per conectar-se a les altres maquines

![Imagen 132](images/image_132.png)


![Imagen 133](images/image_133.png)

Script per fer els backups

![Imagen 134](images/image_134.png)

Configuració dels horaris per fer el backup a crontab.

![Imagen 135](images/image_135.png)

Backups fets correctament

![Imagen 136](images/image_136.png)


![Imagen 137](images/image_137.png)


![Imagen 138](images/image_138.png)

sudo nano /etc/zabbix/zabbix_agentd.conf
Server=172.31.15.83 ServerActive=172.31.15.83 Hostname=SRV-LOGS

![Imagen 139](images/image_139.png)

# Parte De Serveis de Xarxa

## Server audio,vídeo y streaming
PubkeyAuthentication yes: Activé esta directiva para permitir que los usuarios puedan iniciar sesión de forma segura utilizando llaves públicas SSH.
PasswordAuthentication no: Desactivé el acceso por contraseña tradicional para blindar mi servidor contra ataques de fuerza bruta, obligando a usar únicamente llaves criptográficas.
PermitRootLogin no: Prohibí el inicio de sesión directo al usuario administrador (root) por motivos de seguridad, forzando a entrar primero con un usuario normal.

![Imagen 140](images/image_140.png)


![Imagen 141](images/image_141.png)


![Imagen 142](images/image_142.png)

## Instalación de Icecast2 (El Servidor de audio)
sudo apt install icecast2  ices2 -y

![Imagen 143](images/image_143.png)


![Imagen 144](images/image_144.png)


![Imagen 145](images/image_145.png)

Descarga de la herramienta FFmpeg que pondra la musica en bucle

![Imagen 146](images/image_146.png)


![Imagen 147](images/image_147.png)



### Configurar y Encender el Servicio

Se abre el archivo de ciclo de vida:
sudo nano /etc/default/icecast2
ENABLE=true

![Imagen 148](images/image_148.png)

STATUS:

![Imagen 149](images/image_149.png)

Creamos el canal y la playlist de música OGG

![Imagen 150](images/image_150.png)

Se descarga un archivo de audio OGG de prueba

![Imagen 151](images/image_151.png)

Se genera la lista de reproducción

![Imagen 152](images/image_152.png)

Configurar el archivo XML del emisor

![Imagen 153](images/image_153.png)


![Imagen 154](images/image_154.png)

Crear una carpeta y meter una canción de prueba

![Imagen 155](images/image_155.png)

Desde la web ahora sale el canal radio.mp3

![Imagen 194](images/image_194.png)

Entrando al canal sale el audio

![Imagen 156](images/image_156.png)

Para mejorar el server de audio

![Imagen 157](images/image_157.png)


![Imagen 158](images/image_158.png)


![Imagen 159](images/image_159.png)


![Imagen 160](images/image_160.png)


![Imagen 161](images/image_161.png)

Descargamos en mp3 los audios que queremos
Subimos los archivos al ssh de aws mediante scp desde el pc normal: scp -i key nombre.mp3 usuario@ip publica

![Imagen 162](images/image_162.png)

Los movemos a la carpeta de la Web de Nginx del server audio/video/streaming

![Imagen 163](images/image_163.png)

Copiamos los mismos a la playlist interna de la radio

![Imagen 164](images/image_164.png)

Permisos obligatorios para que no se queden en 0 segundos

![Imagen 165](images/image_165.png)

comando final para el arranque

![Imagen 166](images/image_166.png)


![Imagen 167](images/image_167.png)

http:34.205.94.36/radio.html


## Implantació del Servei de Vídeo (Jellyfin)
Descarga de crt:

![Imagen 168](images/image_168.png)

Crear la carpeta para las llaves de seguridad e importar la firma oficial de Jellyfin:

![Imagen 169](images/image_169.png)

Añadir el repositorio oficial

![Imagen 170](images/image_170.png)

instalar Jellyfin:
Jellyfin és una plataforma de servidor de mitjans de codi obert (open-source) dissenyada per a la gestió, organització i transmissió de continguts multimèdia (vídeo i àudio) a través de la xarxa. A diferència d'altres solucions propietàries com Plex o Emby, Jellyfin no requereix llicències ni pagaments premium, fet que permet un control absolut de les dades i de la privacitat dins de la infraestructura del CPD d'InnovateTech.

![Imagen 171](images/image_171.png)


![Imagen 172](images/image_172.png)


![Imagen 173](images/image_173.png)

Creacion de usuario:

![Imagen 174](images/image_174.png)

Añadir biblioteca de medios

![Imagen 175](images/image_175.png)


![Imagen 176](images/image_176.png)

Ruta donde se guardara todo

![Imagen 177](images/image_177.png)


![Imagen 178](images/image_178.png)


![Imagen 179](images/image_179.png)


![Imagen 180](images/image_180.png)


![Imagen 181](images/image_181.png)

Ahora iniciamos sesion

![Imagen 182](images/image_182.png)


![Imagen 183](images/image_183.png)

Segundo video con scp desde el pc real

![Imagen 184](images/image_184.png)

Desde el server audio/video/streaming
ls para saber si esta, pasar el video a mp4 y dar permisos en la carpeta

![Imagen 185](images/image_185.png)


![Imagen 186](images/image_186.png)

entrada en la web para el server de videos http://34.205.94.36:8096


## Funcion del server de video

El servei s'implementa sota el model de Vídeo sota Demanda (VoD - Video on Demand), on els usuaris poden reproduir els continguts allotjats al servidor en qualsevol moment. Per garantir una reproducció fluida sense necessitat de descarregar el fitxer complet prèviament, s'utilitza el protocol de streaming HLS (HTTP Live Streaming):

Trocejament del contingut: El servidor agafa el fitxer de vídeo original (en format MP4 codificat amb el còdec H.264) i el divideix en petits fragments de pocs segons (fitxers .ts).
Índex de reproducció: Es genera un fitxer d'índex o llista de reproducció (extensió .m3u8) que indica al client l'ordre d'aquests fragments.
Transmissió via HTTP: El client (navegador web Firefox) va sol·licitant i descarregant aquests fragments de forma seqüencial a través del port web estàndard (8096 en Jellyfin). Això evita bloquejos de tallafocs i permet la reproducció instantània. Per a la nostra corporació, aquest servei és una eina clau per a:
Formació corporativa: Allotjar cursos, seminaris tècnics i videotutorials per als nous empleats, centralitzant el coneixement de l'empresa.
Comunicació interna: Distribuir comunicats oficials de la direcció, presentacions de resultats o esdeveniments corporatius a tota la plantilla de forma unificada.
Estalvi de recursos: En reproduir per streaming, s'evita la saturació d'emmagatzematge als equips locals dels treballadors. 


## Video conferencia
Desplegar la Videoconferencia (Jitsi + Nginx)

![Imagen 188](images/image_188.png)

Crear la interfaz web de la videoconferencia

![Imagen 189](images/image_189.png)


![Imagen 190](images/image_190.png)


![Imagen 191](images/image_191.png)

Permitir que confien en mi ip: http://34.205.94.36

![Imagen 187](images/image_187.png)

![Imagen 192](images/image_192.png)

Iniciamos streaming

![Imagen 193](images/image_193.png)

entrada para jitsi meet http://34.205.94.36

## Protocolo WebRTC

Descripció de protocols de videoconferència (WebRTC) Tecnologia de codi obert que permet la comunicació d'àudio, vídeo i dades en temps real directament entre navegadors i mòbils.

Sense connectors (plugins): Funciona de manera nativa mitjançant les APIs dels navegadors moderns (Firefox, Chrome), sense necessitat d'instal·lar cap programari addicional.
Canals segurs P2P (Peer-to-Peer)
Connexió directa: El flux de dades viatja directament d'un usuari a un altre sense passar pel servidor central d'AWS, reduint dràsticament la càrrega de la infraestructura.
Seguretat obligatòria: Totes les transmissions estan xifrades de extrem a extrem mitjançant els protocols DTLS i SRTP, garantint la confidencialitat.
Còdecs d'alta eficiència
Optimitzen la qualitat i redueixen el consum d'amplada de banda amb baixa latència:

Àudio (Opus): Estàndard que s'adapta en temps real a la qualitat de la connexió a Internet.
Vídeo (VP8 / H.264): Garanteixen retards gairebé nuls (mil·lisegons) per a una conversa fluida.


## Apartado final Velocidad , los bits y la tasa de bits (bitrate)
En la Radio (Icecast + FFmpeg)

![Imagen 195](images/image_195.png)

Instalar y hacer las 2 Pruebas de Rendimiento
sudo apt install speedtest-cli -y
Primera prueba

![Imagen 196](images/image_196.png)

Velocitat de baixada (Download): 1859.86 Mbit/s
Velocitat de pujada (Upload): 1662.20 Mbit/s
Latència (Ping): 1.601 ms
Segunda prueba poniendo todo en marcha

![Imagen 197](images/image_197.png)

Velocidad de bajada (Download): 1493.09 Mbit/s
Velocitat de subida (Upload): 1834.20 Mbit/s
Latència (Ping): 2.425 ms
Anàlisis del Comportamiento i Consumo del servicio Multimèdia

### Análisis de consumo con múltiples servicios activos

He comparado la capacidad real de mi AWS con el consumo estimado de los servicios funcionando a la vez:
* Streaming de Audio: He configurado la emisión a 128 kbps (0.12 Mbps) por usuario. Si se conectaran 100 personas a la vez, el consumo de subida sería de solo 12.5 Mbps.
* Streaming de Vídeo (Jellyfin): La reproducción de contenido Full HD desde el servidor consume entre 4 Mbps y 8 Mbps por cliente.
* Videoconferencia: Un flujo en alta definición consume unos 3 Mbps simétricos (tanto de subida como de bajada).
Simulación de estrés en un escenario real: Si simulara un caso crítico con 50 usuarios en la radio, 20 mirando vídeos en Jellyfin y 5 en una videoconferencia, el consumo total de la red sería de unos 180 Mbps. Como en mis tests he medido una subida de más de 1600 Mbps, el servidor solo estaría utilizando un 11% de su capacidad. Por lo tanto, la velocidad se mantendría intacta y sin degradación.

### Clasificación del sistema

* Clasificación: ACEPTABLE
* Justificación: Considero que la infraestructura de AWS escogida es excelente. He comprobado que al activar todos los servicios el impacto en la red es inapreciable, manteniendo una latencia increíble de solo 2.4 ms. El ancho de banda del que dispongo evita cualquier tipo de corte, buffering o pérdida de paquetes.

### Propuestas de mejora que añado

Aunque el rendimiento actual roza la perfección, propongo estas tres mejoras para optimizar el sistema en producción:
1. Limitar el bitrate en Jellyfin: Configurar un tope de 10 Mbps por usuario para evitar que la transcodificación de usuarios con mala conexión fatigue la CPU de mi máquina.
1. Implementar QoS (Quality of Service): Priorizar el tráfico de la radio en directo y la videoconferencia por delante de las descargas web estándar de Nginx.
1. Monitorizar el tráfico saliente: Instalar herramientas como vnstat para controlar el volumen de datos consumidos y evitar sorpresas o sobrecostes en la facturación de AWS.

# Parte De Base de Datos

# Server BBDD

Creación de la base de datos con mysql (mariadb):
Instalaremos los paquetes necesarios para poder utilizar mariadb en el servidor:

![Imagen 78](images/image_78.png)


![Imagen 79](images/image_79.png)

Una vez creado, crearemos el script sql para generar las tablas:

![Imagen 80](images/image_80.png)


![Imagen 81](images/image_81.png)


![Imagen 82](images/image_82.png)

Ahora proseguimos con la creación de los 4 roles con sus permisos:

![Imagen 83](images/image_83.png)

Asignaremos permisos a cada rol:

![Imagen 84](images/image_84.png)

Verificamos que los roles existen:

![Imagen 85](images/image_85.png)

Ahora crearemos el script:

![Imagen 86](images/image_86.png)

Le asignamos permisos y ejecutamos el script:

![Imagen 87](images/image_87.png)

Iremos creando cada usuario paso a paso asignándole su rol correspondiente. Y quedaría tal que así:

![Imagen 88](images/image_88.png)

Continuamos con los triggers:
Trigger para bloqueo de usuarios para llamadas:

![Imagen 89](images/image_89.png)


![Imagen 90](images/image_90.png)

Seguimos con el segundo trigger, control de cuota de minutos mensuales:

![Imagen 91](images/image_91.png)

Ahora con el tercero Control de llamadas diarias máximas:

![Imagen 92](images/image_92.png)

Ahora el cuarto Auditoría de accesos no autorizados:

![Imagen 93](images/image_93.png)

Ahora el quinto:
El trigger 5 controla que el rol administración no pueda insertar llamadas (acceso al sistema de video trucadas de clientes), y si lo intenta lo registra en tabla avisos.

![Imagen 94](images/image_94.png)

Ejecutamos el comando SHOW TRIGGERSS\G y salen los 5 triggers que hemos creado.
Para hacer el event periodic de backup hay que activar el scheduler de event en MariaDB:

![Imagen 95](images/image_95.png)

Luego creamos el event:

![Imagen 96](images/image_96.png)


![Imagen 97](images/image_97.png)

Conexión a la base de de datos desde otra estancia:
Crearemos un usuario para ello:
Para ello nos dirigimos al servidor web e instalamos mysql cliente:

![Imagen 98](images/image_98.png)

Una vez instalado nos dirigimos al servidor de base de datos y crearemos un usuario para que pueda remotamente desde cualquier server.

![Imagen 99](images/image_99.png)


![Imagen 100](images/image_100.png)


![Imagen 101](images/image_101.png)

Ahora nos dirigimos al apartado del security group de mi instancia y abrimos el inbound con puerto 3306 en protocolo TCP.

![Imagen 102](images/image_102.png)

En nuestro servidor de base de datos hacemos la siguiente configuración para que escuche en todas las interfaces y no solo en localhost.
Cambiamos 127.0.0.1 a 0.0.0.0

![Imagen 103](images/image_103.png)


![Imagen 104](images/image_104.png)


![Imagen 105](images/image_105.png)


![Imagen 106](images/image_106.png)

Ahora procedemos a la creación del bot de telegram.
Buscamos BotFather en la aplicación de telegram e introducimos /start
vemos las opciones y ponemos el parámetro /newbot
introducimos el nombre del bot y luego el nombre de usuario
Como podemos observar, BotFather nos ha gnerado un TOKEN. Deberíamos buscar en telegram el nombre de nuestro bot e introducir el parámetro /start
BUscamos el enlace que nos proporcionó BotFather sustituyendo el TOKEN y veremos como se nos generó un JSON .

![Imagen 107](images/image_107.png)

Podemos observar como nos ha salido un ID {“id”:8259012916”}.
Nos dirigimos al servidor de base de datos y crearemos un script de alertas, que de eso se basa el bot de telegram:

![Imagen 108](images/image_108.png)


![Imagen 109](images/image_109.png)


![Imagen 110](images/image_110.png)

Instalamos curl:

![Imagen 111](images/image_111.png)

Ejecutamos el script:

![Imagen 112](images/image_112.png)

Configuramos el cron para que se ejecute cada 5 minutos:

![Imagen 113](images/image_113.png)

Pondremos a prueba y mandaremos un aviso en la base de datos de mariadb:

![Imagen 114](images/image_114.png)


![Imagen 115](images/image_115.png)

# Video final de demostracion

[3min.webm](https://github.com/user-attachments/assets/1d1a60a0-cbb0-402c-9de3-d3f6783dbe71)

