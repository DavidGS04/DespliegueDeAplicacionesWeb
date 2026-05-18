# 🐳 Actividad Docker #5 - Orquestación de Servicios con Docker Compose

## 👨‍💻 Autor

**David Garrido Suárez**

---

# 📌 Objetivo de la actividad

Crear y administrar aplicaciones multi-servicio con Docker Compose usando redes, volúmenes y variables de entorno.

---

# 🖥️ Entorno utilizado

| Elemento              | Configuración    |
| --------------------- | ---------------- |
| Sistema operativo     | Ubuntu 24.04 LTS |
| Arquitectura          | x86_64           |
| Virtualización        | VirtualBox       |
| Motor de contenedores | Docker CE        |
| Usuario               | david            |

---

## 🎯 Conceptos importantes

### Docker Compose
Herramienta que permite levantar múltiples contenedores mediante un archivo `docker-compose.yml`.

### Servicio
Cada contenedor definido dentro del archivo Compose.

### Volumen persistente
Permite guardar datos incluso cuando el contenedor se elimina.

### Variables de entorno
Sirven para personalizar configuraciones sin modificar directamente el código.

---

# 🛠️ PRÁCTICA 1 - Servidor web con Nginx

## PASO 1: Crear directorio del proyecto

```bash
mkdir -p ~/compose-web
cd ~/compose-web
```

---

## PASO 2: Crear archivo docker-compose.yml

```bash
nano docker-compose.yml
```

Contenido:

```yaml
services:
  servidor_web:
    image: nginx:stable
    container_name: nginx_compose
    ports:
      - "8090:80"
    volumes:
      - ./sitio:/usr/share/nginx/html:ro
    restart: unless-stopped
```

---

## PASO 3: Crear contenido web

```bash
mkdir sitio
```

```bash
echo "<h1>Servidor Nginx usando Docker Compose</h1>" > sitio/index.html
```

---

## PASO 4: Iniciar aplicación

```bash
docker compose up -d
```

---

## PASO 5: Verificar contenedores

```bash
docker compose ps
```

---

## PASO 6: Probar desde navegador o terminal

```bash
curl http://localhost:8090
```

Resultado esperado:

```html
<h1>Servidor Nginx usando Docker Compose</h1>
```

---

## PASO 7: Consultar logs

```bash
docker compose logs
```

---

## PASO 8: Detener servicios

```bash
docker compose down
```

---

# 🛠️ PRÁCTICA 2 - Aplicación PHP + MariaDB

## PASO 1: Crear estructura del proyecto

```bash
mkdir -p ~/compose-lamp
cd ~/compose-lamp
```

---

## PASO 2: Crear docker-compose.yml

```bash
nano docker-compose.yml
```

Contenido:

```yaml
services:
  web:
    image: php:8.2-apache
    container_name: php_web
    ports:
      - "8085:80"
    volumes:
      - ./app:/var/www/html
    depends_on:
      - database

  database:
    image: mariadb:11
    container_name: mariadb_server
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: ejemplo_db
    volumes:
      - datos_mysql:/var/lib/mysql

volumes:
  datos_mysql:
```

---

## PASO 3: Crear aplicación PHP

```bash
mkdir app
```

```bash
nano app/index.php
```

Contenido:

```php
<?php
echo "<h1>Aplicación PHP funcionando correctamente</h1>";
echo "<p>Servidor ejecutándose con Docker Compose</p>";
echo "<p>Versión PHP: " . phpversion() . "</p>";
?>
```

---

## PASO 4: Iniciar servicios

```bash
docker compose up -d
```

---

## PASO 5: Comprobar estado

```bash
docker compose ps
```

---

## PASO 6: Acceder a la aplicación

```bash
curl http://localhost:8085
```

---

## PASO 7: Ver logs del sistema

```bash
docker compose logs --tail=30
```

---

## PASO 8: Verificar volumen creado

```bash
docker volume ls
```

---

## PASO 9: Parar servicios

```bash
docker compose down
```

---

# 🛠️ PRÁCTICA 3 - Variables de entorno

## PASO 1: Crear carpeta

```bash
mkdir -p ~/compose-env
cd ~/compose-env
```

---

## PASO 2: Crear archivo .env

```bash
nano .env
```

Contenido:

```env
APP_PORT=3001
APP_NAME=MiAplicacion
ENTORNO=desarrollo
```

---

## PASO 3: Crear docker-compose.yml

```bash
nano docker-compose.yml
```

Contenido:

```yaml
services:
  app:
    image: nginx:latest
    container_name: nginx_env
    ports:
      - "${APP_PORT}:80"
    environment:
      - APP_NAME=${APP_NAME}
      - ENTORNO=${ENTORNO}
```

---

## PASO 4: Ver configuración generada

```bash
docker compose config
```

---

## PASO 5: Levantar servicio

```bash
docker compose up -d
```

---

## PASO 6: Verificar ejecución

```bash
docker compose ps
```

---

## PASO 7: Detener aplicación

```bash
docker compose down
```

---

# 📊 Comandos principales de Docker Compose

| Comando | Función |
|----------|----------|
| `docker compose up -d` | Iniciar servicios |
| `docker compose down` | Detener y eliminar servicios |
| `docker compose ps` | Ver contenedores |
| `docker compose logs` | Mostrar logs |
| `docker compose restart` | Reiniciar servicios |
| `docker compose pull` | Descargar imágenes |
| `docker compose build` | Construir imágenes |
| `docker compose exec` | Ejecutar comandos dentro del contenedor |

---

# 📋 Estructura básica de Compose

```yaml
services:
  web:
    image: nginx
    ports:
      - "80:80"

  database:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: ejemplo
```

---

# 💡 Buenas prácticas

## 1. Usar nombres descriptivos

```yaml
container_name: servidor_web
```

---

## 2. Evitar usar latest en producción

```yaml
image: nginx:1.27
```

---

## 3. Utilizar volúmenes para persistencia

```yaml
volumes:
  - datos_mysql:/var/lib/mysql
```

---

## 4. Mantener servicios separados

```yaml
services:
  frontend:
  backend:
  database:
```

---

## 5. Utilizar archivos .env

```yaml
ports:
  - "${APP_PORT}:80"
```

---

# 🔍 Problemas comunes

## Puerto ocupado

```bash
docker ps
```

Cambiar el puerto en `docker-compose.yml`.

---

## Servicio no inicia

```bash
docker compose logs
```

---

## Eliminar todo completamente

```bash
docker compose down -v
```

---

# 🎯 Objetivos alcanzados

- ✅ Crear aplicaciones multi-contenedor
- ✅ Usar Docker Compose correctamente
- ✅ Configurar redes automáticas
- ✅ Utilizar volúmenes persistentes
- ✅ Implementar variables de entorno
- ✅ Gestionar logs y servicios
- ✅ Ejecutar aplicaciones web completas

---
