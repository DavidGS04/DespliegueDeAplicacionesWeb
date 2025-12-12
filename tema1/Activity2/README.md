# 🧩 Activity #2 – Configuración Básica de Apache

En esta actividad realizaremos varios cambios en la configuración del servidor **Apache2**, trabajando con puertos, redirecciones, directivas y creación de estructuras de prueba.  
También se incluye la parte **Activity #2.2**, dedicada a la creación de *scripts* en Bash.

---

## 📌 Objetivos de Activity #2.1

### 1️⃣ Añadir el puerto 81 en Apache además del 80  
Editamos el archivo:

```bash
sudo nano /etc/apache2/ports.conf
```

Añadimos:

```
Listen 81
```

---

### 2️⃣ Añadir dominio “marisma.intranet” al archivo *hosts*

```bash
sudo nano /etc/hosts
```

Añadir línea:

```
127.0.0.1   marisma.intranet
```

---

### 3️⃣ Cambiar directiva **ServerTokens** para mostrar solo el producto

En:  

```
sudo nano /etc/apache2/conf-available/security.conf
```

Modificar:

```
ServerTokens Prod
```

---

### 4️⃣ Cambiar **ServerSignature** y probar páginas de error

En mismo archivo:

```
ServerSignature Off
```

Prueba generando una página de error:

```
http://localhost/error404
```

---

### 5️⃣ Crear directorios *prueba* y *prueba2*

```bash
sudo mkdir /var/www/html/prueba
sudo mkdir /var/www/html/prueba2
```

Crear páginas:

```bash
echo "<h1>Carpeta prueba</h1>" | sudo tee /var/www/html/prueba/index.html
echo "<h1>Carpeta prueba2</h1>" | sudo tee /var/www/html/prueba2/index.html
```

---

### 6️⃣ Redireccionar la carpeta *prueba* hacia *prueba2*

En el VirtualHost:

```
Redirect /prueba /prueba2
```

---

### 7️⃣ Redireccionar solo una página  
Ejemplo:

```
Redirect /prueba/pagina.html /prueba2/pagina.html
```

---

### 8️⃣ Usar la directiva **UserDir**

Activar módulo:

```bash
sudo a2enmod userdir
sudo systemctl restart apache2
```

Esto habilita:

```
/home/usuario/public_html
```

---

### 9️⃣ Usar directiva **Alias**

Ejemplo:

```
Alias /docs /home/usuario/documentos
```

---

### 🔟 Directiva **Options**

Sirve para activar/desactivar características en directorios.  
Ejemplo de desactivar listado:

```
Options -Indexes
```

---

# 🧩 Activity #2.2 – Scripts de automatización en Bash

### ✨ Script 1 – Añadir puerto de escucha a Apache

Debe:
- recibir un parámetro  
- comprobar si existe ya  
- añadirlo si no aparece

```bash
#!/bin/bash
if [ "$#" -ne "1" ]; then
  echo "Error: parámetros insuficientes."
  echo "Uso: script1.sh puerto"
  exit 1
fi

if grep -q "Listen $1" /etc/apache2/ports.conf; then
  echo "El puerto ya existe."
else
  echo "Añadiendo puerto..."
  echo "Listen $1" | sudo tee -a /etc/apache2/ports.conf
  sudo systemctl restart apache2
fi
```

---

### ✨ Script 2 – Añadir dominio al *hosts*

```bash
#!/bin/bash
if [ "$#" -ne "2" ]; then
  echo "Uso: script2.sh dominio ip"
  exit 1
fi

if grep -q "$1" /etc/hosts; then
  echo "El dominio ya existe."
else
  echo "$2 $1" | sudo tee -a /etc/hosts
fi
```

---

### ✨ Script 3 – Crear página web simple

```bash
#!/bin/bash
if [ "$#" -ne "3" ]; then
  echo "Uso: script3.sh titulo cabecera mensaje"
  exit 1
fi

cat << EOF > index.html
<html>
<head><title>$1</title></head>
<body>
<h1>$2</h1>
<p>$3</p>
</body>
</html>
EOF
```

---
