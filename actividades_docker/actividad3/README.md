# 🐳 Actividad Docker #3 - Gestión de imágenes y contenedores Docker

## 👨‍💻 Autor

**David Garrido Suárez**

---

# 📌 Objetivo de la actividad

Aprender a descargar imágenes, crear contenedores con nombres personalizados y administrar su ciclo de vida.

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

## 🎯 Conceptos clave

### Imagen Docker
Una imagen es una plantilla de solo lectura que contiene todo lo necesario para ejecutar una aplicación.

### Contenedor Docker
Es una instancia en ejecución creada a partir de una imagen Docker.

### Docker Hub
Repositorio online desde el que se descargan imágenes oficiales y personalizadas.

### Gestión de contenedores
Docker permite crear, detener, iniciar y eliminar contenedores fácilmente.

---

## 🛠️ Pasos prácticos

### 1️⃣ Descargar la imagen Ubuntu

```bash
docker pull ubuntu
```

**Resultado esperado:**

```text
Status: Downloaded newer image for ubuntu:latest
```

---

### 2️⃣ Descargar la imagen hello-world

```bash
docker pull hello-world
```

**Resultado esperado:**  
La imagen se descarga correctamente.

---

### 3️⃣ Descargar la imagen Nginx

```bash
docker pull nginx
```

**Resultado esperado:**  
La imagen nginx queda disponible localmente.

---

### 4️⃣ Mostrar todas las imágenes descargadas

```bash
docker images
```

**Resultado esperado:**

```text
REPOSITORY    TAG       IMAGE ID       SIZE
ubuntu        latest    xxxxxxxx       77MB
hello-world   latest    xxxxxxxx       13kB
nginx         latest    xxxxxxxx       180MB
```

---

### 5️⃣ Crear un contenedor hello-world llamado prueba1

```bash
docker run --name prueba1 hello-world
```

**Resultado esperado:**  
Mensaje de bienvenida de Docker.

---

### 6️⃣ Crear un segundo contenedor llamado prueba2

```bash
docker run --name prueba2 hello-world
```

**Resultado esperado:**  
El contenedor se ejecuta correctamente.

---

### 7️⃣ Crear un tercer contenedor llamado prueba3

```bash
docker run --name prueba3 hello-world
```

**Resultado esperado:**  
Docker ejecuta correctamente el contenedor.

---

### 8️⃣ Mostrar todos los contenedores

```bash
docker ps -a
```

**Resultado esperado:**

```text
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     NAMES
abc123...      hello-world   "/hello"   1 minute ago    Exited (0) 1 minute ago   prueba1
def456...      hello-world   "/hello"   1 minute ago    Exited (0) 1 minute ago   prueba2
ghi789...      hello-world   "/hello"   1 minute ago    Exited (0) 1 minute ago   prueba3
```

---

### 9️⃣ Detener el contenedor prueba1

```bash
docker stop prueba1
```

**Resultado esperado:**

```text
prueba1
```

---

### 🔟 Eliminar el contenedor prueba1

```bash
docker rm prueba1
```

**Resultado esperado:**

```text
prueba1
```

---

### 1️⃣1️⃣ Mostrar contenedores después de eliminar uno

```bash
docker ps -a
```

**Resultado esperado:**  
Solo aparecen prueba2 y prueba3.

---

### 1️⃣2️⃣ Eliminar los contenedores restantes

```bash
docker rm prueba2 prueba3
```

**Resultado esperado:**

```text
prueba2
prueba3
```

---

### 1️⃣3️⃣ Verificar que no quedan contenedores

```bash
docker ps -a
```

**Resultado esperado:**

```text
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS   NAMES
```

(Tabla vacía)

---

## 📊 Comandos utilizados

| Comando | Descripción | Ejemplo |
|---------|-------------|---------|
| `docker pull` | Descargar imágenes | `docker pull ubuntu` |
| `docker images` | Mostrar imágenes locales | `docker images` |
| `docker run` | Crear y ejecutar contenedores | `docker run hello-world` |
| `docker run --name` | Asignar nombre | `docker run --name web nginx` |
| `docker ps` | Mostrar contenedores activos | `docker ps` |
| `docker ps -a` | Mostrar todos los contenedores | `docker ps -a` |
| `docker stop` | Detener contenedor | `docker stop prueba1` |
| `docker rm` | Eliminar contenedor | `docker rm prueba1` |

---

## 💡 Buenas prácticas

### 1. Usar nombres descriptivos

```bash
# ❌ Malo
docker run nginx

# ✅ Bien
docker run --name servidor-web nginx
```

### 2. Limpiar contenedores no utilizados

```bash
docker container prune -f
```

### 3. Verificar imágenes instaladas

```bash
docker images
```

### 4. Ver todos los contenedores

```bash
docker ps -a
```

---

## 🔍 Troubleshooting

### Error: "Conflict. The container name is already in use"

```bash
docker rm nombre_contenedor
```

---

### Error: "No such image"

```bash
docker pull ubuntu
```

---

### Error: "Cannot remove a running container"

```bash
docker stop nombre
docker rm nombre
```

---

## 🎯 Tareas completadas

- ✅ Descarga de imágenes Docker
- ✅ Gestión de imágenes locales
- ✅ Creación de contenedores con nombres
- ✅ Uso de `docker ps` y `docker ps -a`
- ✅ Eliminación de contenedores
- ✅ Verificación del estado final

---
