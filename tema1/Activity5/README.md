# 🧩 Activity #5 – Directiva **Directory** en Apache  
*(Basado en el enunciado proporcionado)*

En esta actividad trabajaremos con control de acceso en Apache mediante las directivas **Directory**, **Require**, dominios, IPs y máscaras de red.  

---

## 📌 Objetivos de la práctica
- Crear directorios protegidos en Apache  
- Aplicar reglas de acceso mediante directiva **Require**  
- Permitir o denegar accesos por IP, dominio o subdominio  
- Comprobar el funcionamiento mediante evidencias  

---

# 🛠️ Paso 1: Crear directorios dir1 y dir2

```bash
sudo mkdir /var/www/html/dir1
sudo mkdir /var/www/html/dir2
```

Opcionalmente crea un index para cada uno:

```bash
echo "<h1>DIR 1</h1>" | sudo tee /var/www/html/dir1/index.html
echo "<h1>DIR 2</h1>" | sudo tee /var/www/html/dir2/index.html
```

### 📸 *Captura 1: directorios creados*
![captura1](ruta.png)

---

# 🧠 Paso 2: Diferencia entre configuraciones antiguas y nuevas (Require)

Ejemplo antiguo:

```
Order Deny,Allow
Deny from All
Allow from 192.168.1.100
```

Ejemplo moderno equivalente:

```
Require ip 192.168.1.100
```

| Sistema | Forma antigua | Forma moderna |
|--------|----------------|----------------|
| Denegar a todos | `Deny from all` | `Require all denied` |
| Permitir IP | `Allow from 192.168.1.100` | `Require ip 192.168.1.100` |
| Orden | `Order Allow,Deny` | *No necesario en Apache 2.4* |

### 📸 *Captura 2: documento de ejemplo o configuración comparada*
![captura2](ruta.png)

---

# 🛠️ Paso 3: Reglas para **dir1**

Editamos el VirtualHost o añadimos archivo en:  
`/etc/apache2/sites-available/000-default.conf`  
o  
`/etc/apache2/conf-available/`

### a) Permitir acceso desde **10.3.0.100**

```
<Directory /var/www/html/dir1>
    Require ip 10.3.0.100
</Directory>
```

### b) Permitir acceso desde **marisma.intranet**

```
Require host marisma.intranet
```

### c) Permitir acceso desde cualquier subdominio de marisma.intranet

```
Require host .marisma.intranet
```

### d) Permitir acceso desde **10.3.0.100/ máscara 255.255.0.0**

Máscara → rango equivalente: `10.3.0.0/16`

```
Require ip 10.3.0.0/16
```

### 📸 *Captura 3: configuración aplicada en Apache*
![captura3](ruta.png)

---

# 🛠️ Paso 4: Modificar acceso a dir1

### ✔ Permitir acceso solo a **marisma.intranet**  
### ✖ Denegar acceso a **10.3.0.101**

```
<Directory /var/www/html/dir1>
    Require host marisma.intranet
    Require not ip 10.3.0.101
</Directory>
```

### 📸 *Captura 4*
![captura4](ruta.png)

---

# 🛠️ Paso 5: Modificar acceso a dir2

### ✔ Permitir acceso a `10.3.0.100/8`  
→ Esto incluye *toda la red 10.0.0.0 – 10.255.255.255*

### ✖ Denegar acceso a marisma.intranet

```
<Directory /var/www/html/dir2>
    Require ip 10.0.0.0/8
    Require not host marisma.intranet
</Directory>
```

### 📸 *Captura 5*
![captura5](ruta.png)

---
