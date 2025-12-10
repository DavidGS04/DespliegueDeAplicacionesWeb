# 🧩 Activity #1 – Instalación de Apache

En esta actividad aprenderás a instalar y comprobar el funcionamiento del servidor web **Apache** en Ubuntu.

---

## 📌 Objetivos de la práctica
- Instalar el servidor web Apache2  
- Iniciar y comprobar el estado del servicio  
- Probar el acceso desde el navegador  
- Añadir evidencias mediante capturas de pantalla

---

# 🛠️ Paso 1: Actualizar los repositorios del sistema

Antes de instalar cualquier software, actualiza el sistema:

```bash
sudo apt update
sudo apt upgrade -y
```

### 📸 *Captura 1: Evidencia del comando `apt update` y `apt upgrade` ejecutándose*
![Captura1](recursos/Activity1/apache1.png)

---

# 🛠️ Paso 2: Instalar Apache

Ejecuta el siguiente comando para instalar Apache:

```bash
sudo apt install apache2 -y
```

### 📸 *Captura 2: Instalar Apache desde la terminal*
![Captura2](ruta-de-tu-imagen.png)

---

# 🧪 Paso 3: Verificar el estado del servicio Apache

Comprueba que Apache está funcionando:

```bash
sudo systemctl status apache2
```

El estado debe mostrar:

```
active (running)
```

### 📸 *Captura 3: Evidencia del estado “active (running)”*
![Captura3](ruta-de-tu-imagen.png)

---

# 🌐 Paso 4: Acceder a la página web por defecto de Apache

En el navegador escribe:

```
http://localhost
```

o la IP de tu máquina:

```
http://tu-ip
```

Debería aparecer la **página de bienvenida de Apache2**.

### 📸 *Captura 4: Página de bienvenida de Apache en el navegador*
![Captura4](ruta-de-tu-imagen.png)

---

# 🔥 Paso 5: Permitir tráfico HTTP en el firewall (solo si usas UFW)

```bash
sudo ufw allow 'Apache'
sudo ufw status
```

### 📸 *Captura 5: Evidencia de regla activada en UFW*
![Captura5](ruta-de-tu-imagen.png)

---

# 📁 Rutas importantes de Apache

| Ruta | Descripción |
|------|-------------|
| `/var/www/html/` | Carpeta principal del sitio web |
| `/etc/apache2/apache2.conf` | Configuración principal |
| `/etc/apache2/sites-available/` | Archivos de VirtualHost |
| `/var/log/apache2/` | Registros y logs de Apache |

---

# ✅ Resultado Final

Al finalizar correctamente esta actividad debes tener:

✔ Apache instalado  
✔ Servicio en ejecución  
✔ Página de bienvenida visible  
✔ Evidencias en forma de capturas añadidas  

---

# 📚 Referencia utilizada
🔗 Guía DigitalOcean:  
https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-20-04-es

---
