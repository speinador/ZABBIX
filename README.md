# 🚀 Instalación y uso básico de **Zabbix**

## 🧠 ¿Qué es Zabbix?

🖥️ Zabbix es una herramienta de monitoreo de red y servidores de código abierto. Permite recolectar información sobre:

- 📊 CPU
- 💾 Memoria
- 📁 Disco
- 🌐 Red
- 🔍 Procesos

También genera alertas automáticas y gráficos detallados accesibles desde una interfaz web.

🎯 Ideal para aprender cómo se realiza el monitoreo profesional de infraestructura IT.

---

## 🧰 Requisitos para este laboratorio

✅ VirtualBox instalado  
✅ Máquina virtual con Ubuntu Server 22.04  
✅ 2 GB RAM y 10 GB de disco mínimo  
✅ Acceso a internet desde la VM  
✅ Usuario con privilegios `sudo`

---

## 🧱 Paso 1: Preparar la VM

📥 Si no tenés Ubuntu Server instalado, podés descargar una imagen OVA desde:

🔗 [https://www.osboxes.org/ubuntu/](https://www.osboxes.org/ubuntu/)

Importala en VirtualBox desde:  
📁 **Archivo > Importar servicio virtualizado**

---

## ⚙️ Paso 2: Instalar Zabbix

```bash
# Agregar repositorio Zabbix
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-1+ubuntu22.04_all.deb
sudo dpkg -i zabbix-release_7.0-1+ubuntu22.04_all.deb
sudo apt update

# Instalar Zabbix + Apache + MariaDB
sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent mariadb-server
```

### 🗄️ Configurar la base de datos

```bash
sudo mysql -u root
```

```sql
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'zabbix123';
GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

```bash
# Importar datos
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -pzabbix123 zabbix

# Configurar Zabbix
sudo nano /etc/zabbix/zabbix_server.conf
# DBPassword=zabbix123

# Iniciar servicios
sudo systemctl restart zabbix-server zabbix-agent apache2 mariadb
sudo systemctl enable zabbix-server zabbix-agent apache2 mariadb
```

---

## 🌐 Paso 3: Acceder al panel web

Abrí tu navegador y accedé a:  
🔗 `http://[IP_DE_TU_VM]/zabbix`

🔑 Usuario: `Admin`  
🔐 Contraseña: `zabbix`

---

## ✅ Paso 4: Verificación

- ✅ Confirmá que el host local aparece como monitoreado
- 📈 Observá gráficos de CPU, RAM y disco
- ⚠️ Crea un trigger que alerte si la CPU supera el 80%

---

## 🎓 ¿Qué vas a aprender?

- Cómo instalar y configurar una herramienta real de monitoreo
- Cómo conectar base de datos, agente y servidor web
- Cómo visualizar gráficas en tiempo real
- Cómo definir alertas automatizadas

---

## 📚 Más información

📖 Documentación oficial: https://www.zabbix.com/documentation/current/manual
