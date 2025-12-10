# 🧩 Activity #1 – Instalación de Apache, MySQL y PHP

En esta actividad instalarás un entorno **LAMP** completo en Ubuntu:  
✔ **L**inux  
✔ **A**pache  
✔ **M**ySQL  
✔ **P**HP  

Además, deberás aportar **capturas de pantalla** como evidencia para el profesor.

---

# 📌 Objetivos de la práctica
- Instalar y comprobar el funcionamiento de **Apache**
- Instalar el servidor de bases de datos **MySQL**
- Instalar **PHP** y conectarlo con Apache
- Verificar que el servidor interpreta archivos PHP
- Añadir capturas como evidencia

---

# 🛠️ Paso 1: Actualizar el sistema

```bash
sudo apt update
sudo apt upgrade -y
```

### 📸 *Captura 1 – Actualización del sistema*
![cap1](/recursos/Activity1/apache1.png)

---

# 🛠️ Paso 2: Instalar Apache

```bash
sudo apt install apache2 -y
```

Comprobar estado:

```bash
sudo systemctl status apache2
```

### 📸 *Captura 2 – Apache instalado y activo*
![cap2](/recursos/Activity1/apache2.png)

Probar en navegador:

```
http://localhost
```

### 📸 *Captura 3 – Página de bienvenida de Apache*
![cap3](/recursos/Activity1/apache3.png)

---

# 🛠️ Paso 3: Instalar MySQL

```bash
sudo apt install mysql-server -y
```

Comprobar:

```bash
sudo systemctl status mysql
```

### 📸 *Captura 4 – MySQL funcionando*
![cap4](/recursos/Activity1/apache4.png)

Ejecutar script de seguridad:

```bash
sudo mysql_secure_installation
```

### 📸 *Captura 5 – Configuración de seguridad de MySQL*
![cap5](/recursos/Activity1/apache5.png)

---

# 🛠️ Paso 4: Instalar PHP y módulos necesarios

```bash
sudo apt install php libapache2-mod-php php-mysql -y
```

Ver versión:

```bash
php -v
```

### 📸 *Captura 6 – PHP instalado*
![cap6](/recursos/Activity1/apache6.png)

Reiniciar apache:

```bash
sudo systemctl restart apache2
```

---

# 🧪 Paso 5: Probar PHP en Apache

Crear archivo de prueba:

```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

Abrir en navegador:

```
http://localhost/info.php
```

### 📸 *Captura 7 – Página PHP Info funcionando*
![cap7](/recursos/Activity1/apache7.png)

---

# 🧹 Paso 6: Eliminar PHP Info (opcional pero recomendado)

```bash
sudo rm /var/www/html/info.php
```

### 📸 *Captura 8 – Eliminar archivo*
![cap7](/recursos/Activity1/apache8.png)

---

# 📁 Directorios importantes

| Servicio | Ruta |
|---------|------|
| DocumentRoot Apache | `/var/www/html/` |
| Configuración Apache | `/etc/apache2/` |
| Configuración MySQL | `/etc/mysql/` |
| Configuración PHP | `/etc/php/` |

---
