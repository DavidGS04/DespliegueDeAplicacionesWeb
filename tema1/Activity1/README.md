# 🧩 Activity #1 – Instalación de Apache, MySQL y PHP

En esta actividad aprenderás a instalar y comprobar el funcionamiento de un servidor web en Ubuntu.
✔ Apache  
✔ MySQL  
✔ PHP  

El objetivo es dejar el entorno funcionando y demostrarlo con capturas de evidencia.

---

## 📌 Objetivos de la práctica
- Instalar **Apache2**  
- Verificar que el servidor web está activo  
- Instalar el servidor de bases de datos **MySQL**  
- Instalar **PHP** y módulos necesarios  
- Probar PHP mediante un archivo `phpinfo()`  
- Añadir todas las evidencias solicitadas mediante capturas  

---

# 🛠️ Paso 1: Actualizar los repositorios del sistema

Antes de instalar cualquier software, actualiza el sistema:

```bash
sudo apt update
sudo apt upgrade
```

### 📸 *Captura 1: Evidencia del comando `apt update` y `apt upgrade` ejecutándose*
![Captura1](/recursos/Activity1/apache1.png)

---

# 🛠️ Paso 2: Instalar Apache

```bash
sudo apt install apache2
```

### 📸 *Captura 2: Instalación de Apache desde la terminal*
![Captura2](/recursos/Activity1/apache2.png)

---

# 🧪 Paso 3: Verificar el estado del servicio Apache

```bash
sudo systemctl status apache2
```

Debe aparecer:

```
active (running)
```

### 📸 *Captura 3: Evidencia del estado “active (running)”*
![Captura3](/recursos/Activity1/apache3.png)

---

# 🌐 Paso 4: Acceder a la página web por defecto de Apache

En el navegador escribe:

```
http://localhost
```

### 📸 *Captura 4: Página de bienvenida de Apache en el navegador*
![Captura4](/recursos/Activity1/apache4.png)

---

# 🔥 Paso 5: Permitir tráfico HTTP (solo si usas UFW)

```bash
sudo ufw allow 'Apache'
sudo ufw status
```

### 📸 *Captura 5: Evidencia de regla activada en UFW*
![Captura5](/recursos/Activity1/apache5.png)

---

# 🛠️ Paso 6: Instalar MySQL Server

```bash
sudo apt install mysql-server
```

Comprobar estado:

```bash
sudo systemctl status mysql
```

### 📸 *Captura 6: MySQL en ejecución*
![Captura6](/recursos/Activity1/apache6.png)

Ejecutar script de seguridad:

```bash
sudo mysql_secure_installation
```

### 📸 *Captura 7: Proceso mysql_secure_installation*
![Captura7](/recursos/Activity1/apache7.png)

---

# 🛠️ Paso 7: Instalar PHP y módulos necesarios

```bash
sudo apt install php libapache2-mod-php php-mysql
```

Ver versión:

```bash
php -v
```

### 📸 *Captura 8: PHP instalado correctamente*
![Captura8](/recursos/Activity1/apache8.png)

Reiniciar Apache:

```bash
sudo systemctl restart apache2
```

---

# 🧪 Paso 8: Probar PHP en Apache

Crear archivo de prueba:

```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

Abrir en navegador:

```
http://localhost/info.php
```

### 📸 *Captura 9: Página PHP Info funcionando*
![Captura9](/recursos/Activity1/apache9.png)

(Después puedes eliminarlo por seguridad)

```bash
sudo rm /var/www/html/info.php
```

---
