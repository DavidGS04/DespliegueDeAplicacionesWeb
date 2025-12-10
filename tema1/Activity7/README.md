# 🧩 Activity #7 – Reescritura (mod_rewrite) en Apache  

En esta actividad aprenderás a usar el módulo **mod_rewrite** de Apache, uno de los más potentes para manipular URLs, redireccionar, transformar parámetros y mapear rutas limpias hacia scripts internos.

---

# 📌 Objetivos de la actividad

- Activar y usar `mod_rewrite`
- Aplicar reglas de reescritura dentro de un `<Directory>`
- Entender cómo funcionan `RewriteRule` y expresiones regulares
- Realizar los ejercicios del enlace proporcionado
- Probar operaciones aritméticas mediante URL rewriting
- Añadir capturas de evidencia

---

# 🛠️ Paso 1: Activar el módulo de reescritura

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

### 📸 *Captura 1 – Activación de mod_rewrite*
`![cap1](ruta.png)`

---

# 🛠️ Paso 2: Habilitar el uso de Rewrite en Apache

Edita:

```bash
sudo nano /etc/apache2/apache2.conf
```

Busca tu carpeta:

```
<Directory /var/www/html>
    AllowOverride All
</Directory>
```

Esto permite usar `.htaccess` si lo deseas.  
Si NO usas `.htaccess`, deberás colocar las reglas dentro del `<Directory>` como indica el profesor.

---

# 🧪 Paso 3: Reescritura básica (ejemplo del enunciado)

Dentro de:

```
<Directory /var/www/html>
```

añade:

```
Options FollowSymLinks
RewriteEngine On
RewriteBase /
RewriteRule ^([a-z]+)/([0-9]+)/([0-9]+)$ operacion.php?op=$1&op1=$2&op2=$3
```

Esto permitirá URLs como:

```
http://localhost/suma/5/3
http://localhost/resta/10/2
http://localhost/multiplica/4/8
http://localhost/divide/20/4
```

### 📸 *Captura 2 – Configuración de la regla RewriteRule*
`![cap2](ruta.png)`

---

# 🧪 Paso 4: Crear el archivo operacion.php

Crea el archivo:

```bash
sudo nano /var/www/html/operacion.php
```

Contenido completo:

```php
<?php
if (isset($_GET['op']) && isset($_GET['op1']) && isset($_GET['op2'])) {
    $op = $_GET["op"];
    $op1 = $_GET["op1"];
    $op2 = $_GET["op2"];
    $r = 0;

    if ($op == "suma")
        $r = $op1 + $op2;
    else if ($op == "resta")
        $r = $op1 - $op2;
    else if ($op == "multiplica")
        $r = $op1 * $op2;
    else if ($op == "divide")
        $r = $op1 / $op2;

    echo "op = " . $op . "<br>";
    echo "op1 = " . $op1 . " ";
    echo "op2 = " . $op2 . " ";
    echo "Resultado = " . $r;
}
?>
```

### 📸 *Captura 3 – operacion.php funcionando*
`![cap3](ruta.png)`

---

# 🧩 Paso 5: Ejercicios del enlace del profesor

Debes realizar los ejercicios del siguiente enlace:

🔗 http://www.josedomingo.org/pledin/2011/10/ejemplos-del-modulo-rewrite-en-apache-2-2/

Ejemplos típicos:

### ✔ Redirigir URLs antiguas a nuevas

```
RewriteRule ^antigua$ nueva.html
```

### ✔ Crear URLs limpias para perfiles

```
RewriteRule ^usuario/([a-zA-Z0-9]+)$ perfil.php?user=$1
```

### ✔ Forzar HTTPS

```
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [L,R=301]
```

### ✔ Evitar acceso a archivos .txt

```
RewriteRule \.txt$ - [F]
```

### ✔ Redirigir según User-Agent

```
RewriteCond %{HTTP_USER_AGENT} Chrome
RewriteRule ^$ chrome.html
```

### 📸 *Captura 4 – Evidencia de ejercicios realizados*
`![cap4](ruta.png)`

---
