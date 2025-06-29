# Laboratorio: Instalación y uso básico de Zabbix

## ¿Qué es Zabbix?

Zabbix es una herramienta de monitoreo de red y servidores de código abierto. Permite recolectar información sobre CPU, memoria, disco, red, procesos, etc., generar alertas, y visualizar todo en una interfaz web.

Es ideal para aprender cómo se realiza el monitoreo profesional de infraestructura IT.

---

## Requisitos para este laboratorio

- VirtualBox instalado.
- Máquina virtual con Ubuntu Server 22.04 (2 GB RAM, 10 GB disco, conexión a red).
- Acceso a internet desde la VM.
- Usuario con privilegios sudo.

---

## Paso 1: Preparar la VM

Si no tenés Ubuntu Server instalado, podés descargar una imagen OVA ya lista desde:

🔗 https://www.osboxes.org/ubuntu/

Importala en VirtualBox desde:  
**Archivo > Importar servicio virtualizado**.

---

## Paso 2: Instalar Zabbix

```bash
# 1. Agregar el repositorio de Zabbix
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-1+ubuntu22.04_all.deb
sudo dpkg -i zabbix-release_7.0-1+ubuntu22.04_all.deb
sudo apt update

# 2. Instalar Zabbix + Apache + MariaDB
sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent mariadb-server

# 3. Configurar la base de datos
sudo mysql -u root
```

En MySQL ejecutá lo siguiente:

```sql
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'zabbix123';
GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

Luego:

```bash
# 4. Importar datos iniciales
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -pzabbix123 zabbix

# 5. Editar configuración de Zabbix
sudo nano /etc/zabbix/zabbix_server.conf
# Buscar la línea: DBPassword=
# Y escribir: DBPassword=zabbix123

# 6. Iniciar los servicios
sudo systemctl restart zabbix-server zabbix-agent apache2 mariadb
sudo systemctl enable zabbix-server zabbix-agent apache2 mariadb
```

---

## Paso 3: Acceder al panel web

Desde tu navegador, accedé a:  
**http://[IP_DE_TU_VM]/zabbix**

**Usuario:** `Admin`  
**Contraseña:** `zabbix`

---

## Paso 4: Verificar que funcione

- Ingresá y comprobá que el "host local" (la propia VM) aparece como monitoreado.
- Observá los gráficos de uso de CPU, RAM y disco.
- Probá crear un **trigger** que alerte si la CPU supera el 80%.

---

## ¿Qué aprendés con este laboratorio?

- Cómo instalar y configurar una herramienta real de monitoreo.
- Cómo se conecta una base de datos, un agente, un servidor web y la lógica de Zabbix.
- Cómo ver gráficas en tiempo real y recibir alertas por estado del sistema.

---

## Más información

- Documentación: https://www.zabbix.com/documentation/current/manual
