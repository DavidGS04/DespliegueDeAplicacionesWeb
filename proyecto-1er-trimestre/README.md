# 🖧 🖥 Servidores Web: Proyecto 1º Trimestre

Guía **paso a paso en Ubuntu** para desplegar un servidor web interno de un instituto utilizando **Apache**, **WordPress**, una **aplicación Python con mod_wsgi**, **AWStats** para estadísticas y un **segundo servidor Nginx** en el puerto **8080** con **PHP y phpMyAdmin**.

El repositorio documenta **todos los comandos, configuraciones y comprobaciones**, incluyendo capturas de pantalla realizadas durante el proceso en la máquina virtual.

---

## 🧭 Entorno y herramientas a utilizar

- Sistema operativo: Ubuntu Desktop 22.04
- Usuario con privilegios `sudo`
- Navegador web: Firefox
- Dominios internos configurados mediante `/etc/hosts`:
  - `centro.intranet`
  - `departamentos.centro.intranet`
  - `servidor2.centro.intranet`

---

## 0️⃣ Preparación inicial del sistema

Actualizar el sistema e instalar herramientas básicas:

```bash
sudo apt update && sudo apt -y upgrade
sudo apt -y install curl wget unzip git
```

📸 *Captura sugerida*: actualización del sistema finalizada correctamente.

---

## 1️⃣ Configuración de dominios locales (`/etc/hosts`)

Editar el archivo:

```bash
sudo nano /etc/hosts
```

Añadir al final:

```
127.0.0.1   centro.intranet
127.0.0.1   departamentos.centro.intranet
127.0.0.1   servidor2.centro.intranet
```

Guardar con `CTRL + O` y salir con `CTRL + X`.

### Comprobación

```bash
ping -c 1 centro.intranet
ping -c 1 departamentos.centro.intranet
ping -c 1 servidor2.centro.intranet
```

📸 *Captura sugerida*: pings respondiendo correctamente.

---

## 2️⃣ Instalación de Apache y estructura de sitios

### Instalación de Apache

```bash
sudo apt -y install apache2 apache2-utils
```

### Crear carpetas para los sitios

```bash
sudo mkdir -p /var/www/centro.intranet
sudo mkdir -p /var/www/departamentos.centro.intranet
```

### Permisos correctos

```bash
sudo chown -R www-data:www-data /var/www/centro.intranet /var/www/departamentos.centro.intranet
sudo chmod -R 755 /var/www
```

### Verificación del servicio

```bash
sudo systemctl status apache2 --no-pager
```

📸 *Captura sugerida*: Apache activo y en ejecución.

---

## 3️⃣ Instalación de PHP y MySQL

### Instalar PHP y módulos necesarios

```bash
sudo apt -y install libapache2-mod-php php php-mysql php-cli php-curl php-xml php-gd
```

### Instalar MySQL

```bash
sudo apt -y install mysql-server
```

### Configuración básica de seguridad

```bash
sudo mysql_secure_installation
```

Reiniciar Apache:

```bash
sudo systemctl restart apache2
```

📸 *Captura sugerida*: instalación correcta de PHP/MySQL.

---

## 4️⃣ Configuración de VirtualHosts en Apache

### 4.1 VirtualHost para `centro.intranet` (WordPress)

```bash
sudo nano /etc/apache2/sites-available/centro.intranet.conf
```

Contenido:

```
<VirtualHost *:80>
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet

    <Directory /var/www/centro.intranet>
        AllowOverride All
        Options Indexes FollowSymLinks
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/centro_error.log
    CustomLog ${APACHE_LOG_DIR}/centro_access.log combined
</VirtualHost>
```

---

## 4.2️⃣ Activar mod_wsgi (REQUERIDO para aplicaciones Python)

⚠️ **Este paso debe realizarse ANTES de crear el VirtualHost de Python**,  
ya que Apache necesita cargar el módulo WSGI para reconocer las directivas
`WSGIDaemonProcess`, `WSGIProcessGroup` y `WSGIScriptAlias`.

```bash
sudo apt -y install libapache2-mod-wsgi-py3 python3-venv
sudo a2enmod wsgi
sudo systemctl restart apache2
```

Comprobación opcional:

```bash
apache2ctl -M | grep wsgi
```

---

### 4.3 VirtualHost para `departamentos.centro.intranet` (Python + WSGI)

```bash
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
```

Contenido:

```
<VirtualHost *:80>
    ServerName departamentos.centro.intranet

    WSGIDaemonProcess departamentos user=www-data group=www-data threads=5
    WSGIProcessGroup departamentos
    WSGIScriptAlias / /var/www/departamentos.centro.intranet/wsgi.py

    <Directory /var/www/departamentos.centro.intranet>
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/departamentos_error.log
    CustomLog ${APACHE_LOG_DIR}/departamentos_access.log combined
</VirtualHost>
```

### Habilitar sitios y módulos

```bash
sudo a2ensite centro.intranet.conf departamentos.centro.intranet.conf
sudo a2enmod rewrite
sudo apache2ctl configtest
sudo systemctl reload apache2
```

📸 *Captura sugerida*: sitios habilitados correctamente.

---

## 5️⃣ Instalación y configuración de WordPress
*(sin cambios respecto a tu versión)*

---

## 6️⃣ Aplicación Python con mod_wsgi
*(sin cambios respecto a tu versión)*

---

## 7️⃣ Instalación y configuración de AWStats
*(sin cambios respecto a tu versión)*

---

## 8️⃣ Segundo servidor: Nginx en puerto 8080
*(sin cambios respecto a tu versión)*

---

## 9️⃣ phpMyAdmin en Nginx
*(sin cambios respecto a tu versión)*

---

## 🔟 Comprobaciones finales

- WordPress: `http://centro.intranet`
- Python + Auth: `http://departamentos.centro.intranet`
- AWStats: `http://centro.intranet/awstats/`
- Nginx + PHP: `http://servidor2.centro.intranet:8080`

Comandos útiles:

```bash
sudo systemctl status apache2 nginx mysql
sudo tail -f /var/log/apache2/*.log
sudo tail -f /var/log/nginx/*.log
```

---
