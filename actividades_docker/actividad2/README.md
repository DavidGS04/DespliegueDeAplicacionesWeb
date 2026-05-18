# 🐳 Actividad Docker #2 — Gestión y ciclo de vida de contenedores

## 👨‍💻 Autor

**David Garrido Suárez**

---

# 📌 Objetivo de la actividad

Aprender el funcionamiento básico de los contenedores Docker, comprendiendo cómo ejecutarlos, administrarlos y controlar su ciclo de vida mediante comandos CLI.

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

# 📚 Introducción

Docker permite ejecutar aplicaciones dentro de entornos aislados llamados **contenedores**.
Cada contenedor funciona como un proceso independiente que incluye todas las dependencias necesarias para una aplicación.

Durante esta actividad se trabajará con:

* Contenedores interactivos
* Ejecución en segundo plano
* Gestión de logs
* Mapeo de puertos
* Administración de estados
* Inspección avanzada
* Ejecución remota de comandos

---

# ⚡ 1. Lanzar un contenedor Ubuntu interactivo

## 💻 Comando ejecutado

```bash
docker run -it ubuntu bash
```

## 🔍 Verificar sistema interno

```bash
cat /etc/os-release
```

## ✅ Resultado

Se accede directamente al shell Bash del contenedor Ubuntu.

Para salir:

```bash
exit
```

---

# 🌐 2. Desplegar servidor Nginx en background

## 💻 Ejecución

```bash
docker run -d \
--name nginx-server \
-p 8080:80 \
nginx
```

## 📌 Explicación

| Parámetro | Función                       |
| --------- | ----------------------------- |
| `-d`      | Ejecuta en segundo plano      |
| `--name`  | Asigna nombre al contenedor   |
| `-p`      | Mapea puertos host:contenedor |

## ✅ Resultado

Nginx queda funcionando en el puerto 8080.

---

# 📄 3. Consultar logs del contenedor

## 💻 Mostrar logs

```bash
docker logs nginx-server
```

## 💻 Logs en tiempo real

```bash
docker logs -f nginx-server
```

## ✅ Resultado

Visualización de registros de arranque y actividad del servidor web.

---

# 🌍 4. Acceso desde navegador o terminal

## 💻 Acceso mediante curl

```bash
curl http://localhost:8080
```

## 🌐 Acceso web

```text
http://localhost:8080
```

## ✅ Resultado

Se muestra la página de bienvenida de Nginx.

---

# 🧪 5. Crear múltiples contenedores de prueba

## 💻 Ejecución

```bash
docker run --name test1 hello-world
docker run --name test2 hello-world
docker run --name test3 hello-world
```

## ✅ Resultado

Docker descarga y ejecuta cada contenedor correctamente.

---

# 📋 6. Mostrar todos los contenedores

## 💻 Comando

```bash
docker ps -a
```

## ✅ Resultado

Listado completo de:

* Contenedores activos
* Contenedores detenidos
* Estado actual
* Imágenes utilizadas

---

# ⛔ 7. Detener contenedor activo

## 💻 Detención

```bash
docker stop nginx-server
```

## 🔍 Verificación

```bash
docker ps
```

## ✅ Resultado

El contenedor deja de aparecer como activo.

---

# 🔄 8. Reiniciar contenedor detenido

## 💻 Inicio nuevamente

```bash
docker start nginx-server
```

## 🔍 Verificar ejecución

```bash
docker ps
```

## ✅ Resultado

Nginx vuelve a ejecutarse correctamente.

---

# 🔎 9. Inspección avanzada del contenedor

## 💻 Inspeccionar detalles internos

```bash
docker inspect nginx-server
```

## ✅ Información obtenida

* ID interno
* Configuración de red
* Variables de entorno
* Imagen utilizada
* Estado del contenedor
* Configuración de almacenamiento

---

# 🖥️ 10. Ejecutar comandos dentro del contenedor

## 💻 Ejecutar comando remoto

```bash
docker exec nginx-server ls -la /usr/share/nginx/html
```

## 💻 Acceso interactivo

```bash
docker exec -it nginx-server bash
```

Dentro del contenedor:

```bash
ls
pwd
cat /usr/share/nginx/html/index.html
```

Salir:

```bash
exit
```

## ✅ Resultado

Acceso completo al entorno interno del contenedor.

---

# 📊 11. Monitorización en tiempo real

## 💻 Estadísticas del contenedor

```bash
docker stats nginx-server
```

## ✅ Información monitorizada

* Uso CPU
* Uso memoria
* I/O de red
* Procesos activos

---

# 🔄 Ciclo de vida de un contenedor

```text
CREATED → RUNNING → STOPPED → REMOVED
```

### Estados principales

| Estado  | Descripción          |
| ------- | -------------------- |
| Created | Contenedor creado    |
| Running | Contenedor activo    |
| Stopped | Contenedor detenido  |
| Removed | Contenedor eliminado |

---

# 📚 Comandos principales utilizados

| Comando          | Función                     |
| ---------------- | --------------------------- |
| `docker run`     | Crear y ejecutar contenedor |
| `docker ps`      | Ver contenedores activos    |
| `docker ps -a`   | Ver todos los contenedores  |
| `docker logs`    | Mostrar logs                |
| `docker stop`    | Detener contenedor          |
| `docker start`   | Iniciar contenedor          |
| `docker inspect` | Ver configuración interna   |
| `docker exec`    | Ejecutar comandos           |
| `docker stats`   | Monitorización              |

---

# 🛡️ Buenas prácticas aplicadas

* Uso de nombres descriptivos
* Separación de servicios por contenedor
* Uso de puertos personalizados
* Monitorización mediante logs
* Gestión de estados correctamente

---

# 📈 Resultado final

La actividad permitió:

* Comprender el ciclo de vida de un contenedor
* Ejecutar servicios en segundo plano
* Administrar contenedores activos y detenidos
* Interactuar internamente con contenedores
* Monitorizar recursos en tiempo real

---
