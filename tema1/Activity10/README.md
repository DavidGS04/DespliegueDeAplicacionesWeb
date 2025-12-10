# 🧩 Activity #10 – SSL, Certificados y HTTPS en Apache

En esta actividad aprenderás a instalar y configurar certificados **SSL**, tanto **autofirmados** como **Let's Encrypt**, en un servidor Apache alojado en una instancia **AWS EC2 con IP pública**.

Esta práctica combina:
- 🔐 Cifrado asimétrico  
- 🔒 Certificados SSL autofirmados  
- 🌍 HTTPS real mediante Let's Encrypt + Certbot  
- 🖥️ Configuración de Virtual Hosts HTTPS  
- 📝 Documentación y evidencias

---

# 📚 Documentación recomendada

### 🔐 Cifrado asimétrico
https://www.criptored.upm.es/intypedia/video.php?id=criptografia-asimetrica&lang=es  

### 🔒 Certificado autofirmado (mod-ssl)
https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-20-04-es  

### 📄 Instalación de certificado autofirmado
https://josejuansanchez.org/iaw/practica-01-04/index.html  

### 🌍 HTTPS con Let’s Encrypt y Certbot
https://josejuansanchez.org/iaw/practica-https/index.html  

---

# ☁️ Requisitos previos (AWS EC2)

- Instancia EC2 ejecutando **Ubuntu 20.04/22.04**  
- Security Group con puertos **80** y **443** abiertos  
- IP pública funcionando  
- Apache instalado  

Comprobación:

```bash
sudo systemctl status apache2
```

---

# 🛠️ PARTE 1 – Certificado SSL autofirmado (OpenSSL)

Según la práctica:  
👉 https://josejuansanchez.org/iaw/practica-01-04/index.html

---

## 1️⃣ Activar el módulo SSL en Apache

```bash
sudo a2enmod ssl
sudo systemctl restart apache2
```

### 📸 *Captura 1 – Activación del módulo SSL*
`![cap1](ruta.png)`

---

## 2️⃣ Crear un certificado autofirmado con OpenSSL

```bash
sudo mkdir /etc/apache2/ssl
cd /etc/apache2/ssl

sudo openssl req -x509 -nodes -days 365   -newkey rsa:2048   -keyout apache.key   -out apache.crt
```

Rellena los datos solicitados (CN = dominio o IP pública).

### 📸 *Captura 2 – Generación del certificado*
`![cap2](ruta.png)`

---

## 3️⃣ Configurar Virtual Host SSL

Archivo:

```bash
sudo nano /etc/apache2/sites-available/default-ssl.conf
```

Debes verificar o añadir:

```
<IfModule mod_ssl.c>
<VirtualHost _default_:443>
    ServerAdmin ubuntu@localhost
    ServerName TU_DOMINIO_O_IP

    DocumentRoot /var/www/html

    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/apache.crt
    SSLCertificateKeyFile /etc/apache2/ssl/apache.key
</VirtualHost>
</IfModule>
```

Habilitar el sitio:

```bash
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2
```

### 📸 *Captura 3 – Navegador mostrando HTTPS con advertencia (autofirmado)*
`![cap3](ruta.png)`

---

# 🛠️ PARTE 2 – HTTPS real con Let’s Encrypt + Certbot

Según la práctica:  
👉 https://josejuansanchez.org/iaw/practica-https/index.html

---

## 4️⃣ Crear un dominio dinámico en NO-IP

1. Registrarse en https://www.noip.com  
2. Crear un **hostname** del tipo:
   ```
   tunombre.ddns.net
   ```
3. Apuntar el dominio a la **IP pública** de tu EC2

### 📸 *Captura 4 – Host NO-IP configurado*
`![cap4](ruta.png)`

---

## 5️⃣ Instalar Certbot

```bash
sudo apt update
sudo apt install certbot python3-certbot-apache -y
```

---

## 6️⃣ Generar el certificado HTTPS válido

```bash
sudo certbot --apache
```

Selecciona tu dominio **NO-IP**, responde *Y* cuando pregunte sobre redirección automática HTTP → HTTPS.

Certbot instalará automáticamente:

- Certificado válido  
- Actualización automática  
- Redirección HTTPS  
- Nuevos VirtualHost seguros  

### 📸 *Captura 5 – Certbot completando instalación*
`![cap5](ruta.png)`

---

# 🧪 Comprobaciones finales

## ✔ Probar HTTPS válido:

```
https://tunombre.ddns.net
```

Debe mostrar el **candado verde** 🔒 en el navegador.

### 📸 *Captura 6 – Navegador mostrando HTTPS válido*
`![cap6](ruta.png)`

---

## ✔ Verificar renovación automática

```bash
sudo certbot renew --dry-run
```

---

# 📝 Zona de evidencias

Aquí debes añadir todas las capturas solicitadas:

```
![evidencia1](ruta.png)
![evidencia2](ruta.png)
![evidencia3](ruta.png)
```

Incluye evidencias de:

- Creación del certificado autofirmado  
- Configuración SSL en Apache  
- NO-IP configurado  
- Certbot funcionando  
- Navegador mostrando HTTPS  
- Renovación automática  

---

# ✔ Actividad completada

