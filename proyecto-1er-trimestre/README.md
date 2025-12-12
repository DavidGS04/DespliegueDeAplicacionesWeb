# 🖧 🖥 Servidores Web: Proyecto 1º Trimestre

Guía **paso a paso en Ubuntu** para desplegar un servidor web interno de un instituto utilizando **Apache**, **WordPress**, una **aplicación Python con mod_wsgi**, **AWStats** para estadísticas y un **segundo servidor Nginx** en el puerto **8080** con **PHP y phpMyAdmin**.

El repositorio documenta **todos los comandos, configuraciones y comprobaciones**, incluyendo capturas de pantalla realizadas durante el proceso en la máquina virtual.

---

## 🧭 Entorno y herramientas a utilizar

- Sistema operativo: **Ubuntu Desktop 22.04**
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

![Captura](/recursos/1.png)
![Captura](/recursos/2.png)

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

![Captura](/recursos/3.png)

Guardar con `CTRL + O` y salir con `CTRL + X`.

### Comprobación

```bash
ping -c 1 centro.intranet
ping -c 1 departamentos.centro.intranet
ping -c 1 servidor2.centro.intranet
```

![Captura](/recursos/4.png)

---

## 2️⃣ Instalación de Apache y estructura de sitios

### Instalación de Apache

```bash
sudo apt -y install apache2 apache2-utils
```

![Captura](/recursos/5.png)

### Crear carpetas para los sitios

```bash
sudo mkdir -p /var/www/centro.intranet
sudo mkdir -p /var/www/departamentos.centro.intranet
```

![Captura](/recursos/6.png)

### Permisos correctos

```bash
sudo chown -R www-data:www-data /var/www/centro.intranet /var/www/departamentos.centro.intranet
sudo chmod -R 755 /var/www
```

![Captura](/recursos/7.png)

### Verificación del servicio

```bash
sudo systemctl status apache2 --no-pager
```

![Captura](/recursos/8.png)

---

## 3️⃣ Instalación de PHP y MySQL

### Instalar PHP y módulos necesarios

```bash
sudo apt -y install libapache2-mod-php php php-mysql php-cli php-curl php-xml php-gd
```

![Captura](/recursos/9.png)

### Instalar MySQL

```bash
sudo apt -y install mysql-server
```

![Captura](/recursos/10.png)

### Configuración básica de seguridad

```bash
sudo mysql_secure_installation
```

![Captura](/recursos/11.png)

Reiniciar Apache:

```bash
sudo systemctl restart apache2
```

![Captura](/recursos/12.png)

---

## 4️⃣ Configuración de VirtualHosts en Apache

### 4.1 VirtualHost para `centro.intranet` (WordPress)

```bash
sudo nano /etc/apache2/sites-available/centro.intranet.conf
```

![Captura](/recursos/13.png)

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

![Captura](/recursos/14.png)

---

### 4.2 Activar mod_wsgi

⚠️ **Este paso debe realizarse ANTES de crear el VirtualHost de Python**,  
ya que Apache necesita cargar el módulo para reconocer las directivas WSGI.

```bash
sudo apt -y install libapache2-mod-wsgi-py3 python3-venv
sudo a2enmod wsgi
sudo systemctl restart apache2
```

Comprobación opcional:

```bash
apache2ctl -M | grep wsgi
```

![Captura](/recursos/15.png)
![Captura](/recursos/16.png)

---

### 4.3 VirtualHost para `departamentos.centro.intranet` (Python + WSGI)

```bash
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
```

![Captura](/recursos/17.png)

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

![Captura](/recursos/18.png)

### Habilitar sitios y módulos

```bash
sudo a2ensite centro.intranet.conf departamentos.centro.intranet.conf
sudo a2enmod rewrite
sudo apache2ctl configtest
sudo systemctl reload apache2
```

---

## 5️⃣ Instalación y configuración de WordPress

### 5.1 Crear base de datos y usuario

```bash
sudo mysql
```

```sql
CREATE DATABASE wordpress CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'wp_password_seguro';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

![Captura](/recursos/19.png)

---

### 5.2 Descargar y copiar WordPress

```bash
cd /tmp
wget https://wordpress.org/latest.zip
unzip latest.zip
sudo rsync -avP wordpress/ /var/www/centro.intranet/
```

![Captura](/recursos/20.png)
![Captura](/recursos/21.png)

---

### 5.3 Configurar `wp-config.php`

```bash
cd /var/www/centro.intranet
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```

![Captura](/recursos/22.png)

Modificar:

```php
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wpuser' );
define( 'DB_PASSWORD', 'wp_password_seguro' );
define( 'DB_HOST', 'localhost' );
```

![Captura](/recursos/23.png)

Permisos:

```bash
sudo chown -R www-data:www-data /var/www/centro.intranet
sudo find /var/www/centro.intranet -type d -exec chmod 755 {} \;
sudo find /var/www/centro.intranet -type f -exec chmod 644 {} \;
```

![Captura](/recursos/24.png)

Acceso:

```
http://centro.intranet
```

![Captura](/recursos/25.png)
![Captura](/recursos/26.png)
![Captura](/recursos/27.png)

---

## 6️⃣ Aplicación Python con mod_wsgi

### 6.1 Aplicación Python mínima

```bash
sudo nano /var/www/departamentos.centro.intranet/app.py
```

![Captura](/recursos/28.png)

```python
def application(environ, start_response):
    status = '200 OK'
    headers = [('Content-Type', 'text/html; charset=utf-8')]
    start_response(status, headers)
    return [b"<h1>Aplicación Python OK</h1>"]
```

![Captura](/recursos/29.png)

```bash
sudo nano /var/www/departamentos.centro.intranet/wsgi.py
```

![Captura](/recursos/30.png)

```python
import sys
sys.path.insert(0, '/var/www/departamentos.centro.intranet')
from app import application
```

![Captura](/recursos/31.png)

```bash
sudo chown -R www-data:www-data /var/www/departamentos.centro.intranet
sudo systemctl restart apache2
```

![Captura](/recursos/32.png)

Acceso:

```
http://departamentos.centro.intranet
```

![Captura](/recursos/33.png)

---

### 6.2 Protección con autenticación básica

```bash
sudo apt -y install apache2-utils
sudo htpasswd -c /etc/apache2/.htpasswd profesor
```

![Captura](/recursos/34.png)

Editar VirtualHost:

```
<Directory /var/www/departamentos.centro.intranet>
    AuthType Basic
    AuthName "Acceso restringido"
    AuthUserFile /etc/apache2/.htpasswd
    Require valid-user
</Directory>
```

![Captura](/recursos/35.png)

```bash
sudo systemctl reload apache2
```

![Captura](/recursos/36.png)

---

## 7️⃣ Instalación y configuración de AWStats

```bash
sudo apt -y install awstats
sudo a2enmod cgi
sudo systemctl restart apache2
sudo nano /etc/apache2/conf-available/awstats.conf
sudo a2enconf awstats
sudo systemctl reload apache2
```

![Captura](/recursos/37.png)
![Captura](/recursos/38.png)
![Captura](/recursos/39.png)
![Captura](/recursos/40.png)
Configuración:

```bash
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
sudo nano /etc/awstats/awstats.centro.intranet.conf
```

![Captura](/recursos/41.png)

Valores clave:

```
LogFile="/var/log/apache2/centro_access.log"
SiteDomain="centro.intranet"
HostAliases="localhost 127.0.0.1 www.centro.intranet"
```

![Captura](/recursos/42.png)
![Captura](/recursos/43.png)
![Captura](/recursos/44.png)

Actualizar estadísticas:

```bash
sudo /usr/lib/cgi-bin/awstats.pl -config=centro.intranet -update
```

![Captura](/recursos/45.png)

Acceso:

```
http://centro.intranet/awstats/awstats.pl?config=centro.intranet
```

![Captura](/recursos/46.png)

---

## 8️⃣ Segundo servidor: Nginx en puerto 8080

### Instalación

```bash
sudo apt -y install nginx php-fpm php-mysql
```

![Captura](/recursos/47.png)

### Crear DocumentRoot

```bash
sudo mkdir -p /var/www/servidor2.centro.intranet
echo "<?php phpinfo();" | sudo tee /var/www/servidor2.centro.intranet/info.php
sudo chown -R www-data:www-data /var/www/servidor2.centro.intranet
```

![Captura](/recursos/48.png)

### Configurar servidor Nginx

```bash
sudo nano /etc/nginx/sites-available/servidor2.centro.intranet
```

```
server {
    listen 8080;
    server_name servidor2.centro.intranet;
    root /var/www/servidor2.centro.intranet;
    index index.php index.html;

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;
    }
}
```

![Captura](/recursos/49.png)

Primero eliminamos el archivo por defecto para que no haya conflictos con el puerto 8080.

```bash
sudo rm /etc/nginx/sites-enabled/default
```

![Captura](/recursos/50.png)

```bash
sudo ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
sudo systemctl status nginx
```

![Captura](/recursos/51.png)

Acceso:

```
http://servidor2.centro.intranet:8080/info.php
```

![Captura](/recursos/52.png)

---

## 9️⃣ phpMyAdmin en Nginx

```bash
sudo apt -y install phpmyadmin
sudo ln -s /usr/share/phpmyadmin /var/www/servidor2.centro.intranet/phpmyadmin
sudo systemctl reload nginx
```

![Captura](/recursos/53.png)

Acceso:

```
http://servidor2.centro.intranet:8080/phpmyadmin
```

![Captura1](/recursos/54.png)

---

## 🔟 Comprobaciones finales

- WordPress: `http://centro.intranet`

  ![Captura1](/recursos/55.png)

- Python + autenticación: `http://departamentos.centro.intranet`

  ![Captura1](/recursos/36.png)

- AWStats: `http://centro.intranet/awstats/awstats.pl?config=centro.intranet`

  ![Captura1](/recursos/46.png)

- Nginx + PHP: `http://servidor2.centro.intranet:8080/info.php`

  ![Captura1](/recursos/52.png)

- phpMyAdmin en Nginx: `http://servidor2.centro.intranet:8080/phpmyadmin/`

  ![Captura1](/recursos/54.png)

---
