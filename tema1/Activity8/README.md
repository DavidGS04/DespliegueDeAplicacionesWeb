# 🧩 Activity #8 – VirtualHost

En esta actividad aprenderás a configurar **Virtual Hosts** en Apache para alojar múltiples sitios web en un mismo servidor.  
Basado en la documentación oficial y recursos proporcionados por el profesor.

---

## 📚 Documentación recomendada
- https://httpd.apache.org/docs/2.4/es/vhosts/
- https://www.digitalocean.com/community/tutorials/como-configurar-virtual-host-de-apache-en-ubuntu-14-04-lts-es
- https://httpd.apache.org/docs/2.0/es/vhosts/
- http://geekland.eu/integrar-maquina-virtual-en-una-red-local/
- http://linuxconfig.org/configuring-virtual-network-interfaces-in-linux
- https://www.oclc.org/support/services/ezproxy/documentation/technote/2w.en.html

---

# 🛠️ Paso 1: Crear la estructura de los sitios web

Ejemplo con dos sitios:

```bash
sudo mkdir -p /var/www/site1.com/public_html
sudo mkdir -p /var/www/site2.com/public_html
```

Asignar permisos:

```bash
sudo chown -R $USER:$USER /var/www/site1.com/public_html
sudo chown -R $USER:$USER /var/www/site2.com/public_html
```

Crear páginas de prueba:

```bash
echo "<h1>Bienvenido a Site1</h1>" > /var/www/site1.com/public_html/index.html
echo "<h1>Bienvenido a Site2</h1>" > /var/www/site2.com/public_html/index.html
```

---

# 🛠️ Paso 2: Crear archivos de VirtualHost

Ejemplo para **site1.com**:

```
sudo nano /etc/apache2/sites-available/site1.com.conf
```

Contenido:

```
<VirtualHost *:80>
    ServerName site1.com
    ServerAlias www.site1.com
    DocumentRoot /var/www/site1.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/site1_error.log
    CustomLog ${APACHE_LOG_DIR}/site1_access.log combined
</VirtualHost>
```

Repetir para **site2.com**:

```
sudo nano /etc/apache2/sites-available/site2.com.conf
```

---

# 🛠️ Paso 3: Activar los VirtualHost

```bash
sudo a2ensite site1.com.conf
sudo a2ensite site2.com.conf
sudo systemctl reload apache2
```

Desactivar el sitio por defecto (opcional):

```bash
sudo a2dissite 000-default.conf
sudo systemctl reload apache2
```

---

# 🛠️ Paso 4: Editar el archivo hosts para resolver dominios localmente

En Linux:

```bash
sudo nano /etc/hosts
```

Añadir:

```
127.0.0.1   site1.com
127.0.0.1   site2.com
```

---

# 🧪 Paso 5: Probar los sitios en el navegador

Visitar:

```
http://site1.com
http://site2.com
```

Cada uno debe mostrar su página correspondiente.

---

# 🛠️ Paso 6: Opcional – Múltiples IPs o interfaces virtuales

Crear interfaz virtual:

```bash
sudo ip addr add 192.168.1.50/24 dev eth0
```

Asignar ese IP a un VirtualHost:

```
<VirtualHost 192.168.1.50:80>
```

---
