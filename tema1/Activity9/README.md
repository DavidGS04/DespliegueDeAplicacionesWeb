# 🧩 Activity #9 – Authentication

En esta actividad aprenderás a configurar **autenticación básica**, creación de usuarios, grupos y el uso de las directivas `Require`, `AuthType`, `AuthUserFile`, `AuthGroupFile` y `Satisfy`.

---

## 📚 Documentación recomendada
- https://httpd.apache.org/docs/2.4/es/howto/auth.html  
- https://httpd.apache.org/docs/2.0/es/howto/auth.html  

---

# 🛠️ Paso 1: Crear usuarios

Se deben crear los usuarios:

**usuario1, usuario2, usuario3, usuario4, usuario5**

Ejecuta:

```bash
sudo htpasswd -c /etc/apache2/.htpasswd usuario1
sudo htpasswd /etc/apache2/.htpasswd usuario2
sudo htpasswd /etc/apache2/.htpasswd usuario3
sudo htpasswd /etc/apache2/.htpasswd usuario4
sudo htpasswd /etc/apache2/.htpasswd usuario5
```

### 📸 *Captura 1 – Creación de usuarios*
`![cap1](ruta.png)`

---

# 🛠️ Paso 2: Crear grupos de usuarios

Grupo **grupo1** → usuario1, usuario2  
Grupo **grupo2** → usuario3, usuario4, usuario5

Crear archivo de grupos:

```bash
sudo nano /etc/apache2/.htgroups
```

Contenido:

```
grupo1: usuario1 usuario2
grupo2: usuario3 usuario4 usuario5
```

### 📸 *Captura 2 – Archivo de grupos*
`![cap2](ruta.png)`

---

# 🛠️ Paso 3: Crear directorio privado1 (acceso para todos)

```bash
sudo mkdir /var/www/html/privado1
echo "<h1>Privado 1</h1>" | sudo tee /var/www/html/privado1/index.html
```

Configurar autenticación en Apache:

```
<Directory /var/www/html/privado1>
    AuthType Basic
    AuthName "Acceso restringido"
    AuthUserFile /etc/apache2/.htpasswd
    Require valid-user
</Directory>
```

Esto permite el acceso a **todos los usuarios válidos**.

### 📸 *Captura 3 – Configuración privado1*
`![cap3](ruta.png)`

---

# 🛠️ Paso 4: Crear directorio privado2 (acceso solo grupo1)

```bash
sudo mkdir /var/www/html/privado2
echo "<h1>Privado 2</h1>" | sudo tee /var/www/html/privado2/index.html
```

Configurar autenticación:

```
<Directory /var/www/html/privado2>
    AuthType Basic
    AuthName "Zona Grupo1"
    AuthUserFile /etc/apache2/.htpasswd
    AuthGroupFile /etc/apache2/.htgroups
    Require group grupo1
</Directory>
```

Solo **usuario1 y usuario2** podrán acceder.

### 📸 *Captura 4 – Configuración privado2*
`![cap4](ruta.png)`

---

# 🛠️ Paso 5: Usar la directiva `Satisfy` (restricción combinada)

La directiva **Satisfy** combina:

- Autorización por IP (`Require ip`)
- Autorización por usuario (`Require user`, `Require valid-user`, `Require group`)

## ✔ Configurar privado2 para que solo sea accesible desde localhost

```
<Directory /var/www/html/privado2>
    AuthType Basic
    AuthName "Zona Grupo1"
    AuthUserFile /etc/apache2/.htpasswd
    AuthGroupFile /etc/apache2/.htgroups
    Require group grupo1
    Require ip 127.0.0.1
    Satisfy all
</Directory>
```

### 🔎 Comportamiento:

### 🔸 `Satisfy all`
→ Debe cumplirse **tanto** la IP correcta **como** la autenticación.  
✔ Solo usuarios del grupo1 desde localhost accederán.  
✖ Desde cualquier otra IP: acceso denegado aunque pongan usuario/contraseña correctos.

### 🔸 `Satisfy any`
→ Basta cumplir **una** condición: IP correcta **o** autenticación.  
✔ Accede cualquiera desde localhost sin pedir contraseña  
✔ Acceden usuario1/usuario2 desde cualquier IP  
✖ Usuarios del grupo2 son rechazados siempre

### 📸 *Captura 5 – Probando diferencias entre any / all*
`![cap5](ruta.png)`

---
