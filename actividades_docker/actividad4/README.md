# 🐳 Actividad Docker #4 - Persistencia y Redes en Docker

## 👨‍💻 Autor

**David Garrido Suárez**

---

# 📌 Objetivo de la actividad

Comprender el funcionamiento de volúmenes, bind mounts y redes Docker mediante ejemplos prácticos.

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

## 🎯 Conceptos principales

### Volumen Docker
Espacio de almacenamiento administrado por Docker que mantiene los datos aunque el contenedor sea eliminado.

### Bind Mount
Permite enlazar carpetas reales del sistema host con directorios internos del contenedor.

### Red personalizada
Red virtual creada por Docker para que varios contenedores puedan comunicarse entre sí utilizando nombres.

### Persistencia
Capacidad de conservar datos incluso después de detener o eliminar contenedores.

---

# 🛠️ Parte 1 - Uso de volúmenes Docker

## 1️⃣ Crear un volumen persistente

```bash
docker volume create volumen_practica
```

**Resultado esperado:** Docker crea el volumen correctamente.

---

## 2️⃣ Mostrar volúmenes disponibles

```bash
docker volume ls
```

**Resultado esperado:**

```bash
DRIVER    VOLUME NAME
local     volumen_practica
```

---

## 3️⃣ Ejecutar contenedor usando el volumen

```bash
docker run -d --name contenedor_datos -v volumen_practica:/datos ubuntu sleep 1000
```

**Explicación:**
- `-v volumen_practica:/datos` monta el volumen dentro del contenedor
- `sleep 1000` mantiene el contenedor funcionando

---

## 4️⃣ Crear un archivo dentro del volumen

```bash
docker exec contenedor_datos bash -c "echo 'Docker persistente' > /datos/info.txt"
```

---

## 5️⃣ Verificar contenido almacenado

```bash
docker exec contenedor_datos cat /datos/info.txt
```

**Resultado esperado:**

```bash
Docker persistente
```

---

## 6️⃣ Inspeccionar información del volumen

```bash
docker volume inspect volumen_practica
```

**Resultado esperado:** JSON con información del volumen y punto de montaje.

---

## 7️⃣ Eliminar el contenedor

```bash
docker rm -f contenedor_datos
```

---

## 8️⃣ Crear un nuevo contenedor reutilizando el mismo volumen

```bash
docker run -d --name contenedor_nuevo -v volumen_practica:/datos ubuntu sleep 1000
```

---

## 9️⃣ Verificar persistencia de datos

```bash
docker exec contenedor_nuevo cat /datos/info.txt
```

**Resultado esperado:**

```bash
Docker persistente
```

✅ El archivo sigue existiendo aunque el contenedor anterior fue eliminado.

---

# 🛠️ Parte 2 - Uso de Bind Mounts

## 🔟 Crear carpeta compartida en el host

```bash
mkdir -p ~/web_compartida
echo "<h1>Servidor Docker</h1>" > ~/web_compartida/index.html
```

---

## 1️⃣1️⃣ Ejecutar Nginx usando bind mount

```bash
docker run -d --name nginx_bind -p 8090:80 -v ~/web_compartida:/usr/share/nginx/html:ro nginx
```

**Explicación:**
- `:ro` indica modo solo lectura
- El contenido del host se refleja dentro del contenedor

---

## 1️⃣2️⃣ Acceder desde navegador o curl

```bash
curl http://localhost:8090
```

**Resultado esperado:**

```html
<h1>Servidor Docker</h1>
```

---

## 1️⃣3️⃣ Modificar contenido desde el host

```bash
echo "<h1>Contenido actualizado</h1>" > ~/web_compartida/index.html
```

---

## 1️⃣4️⃣ Verificar cambios automáticamente

```bash
curl http://localhost:8090
```

**Resultado esperado:**

```html
<h1>Contenido actualizado</h1>
```

✅ Los cambios se reflejan instantáneamente sin reiniciar el contenedor.

---

# 🌐 Parte 3 - Redes Docker

## 1️⃣5️⃣ Crear una red personalizada

```bash
docker network create red_practica
```

---

## 1️⃣6️⃣ Ver redes existentes

```bash
docker network ls
```

**Resultado esperado:**

```bash
NETWORK ID     NAME            DRIVER    SCOPE
xxxxxx         red_practica    bridge    local
```

---

## 1️⃣7️⃣ Crear contenedor web conectado a la red

```bash
docker run -d --name servidor_web --network red_practica nginx
```

---

## 1️⃣8️⃣ Crear segundo contenedor en la misma red

```bash
docker run -it --name cliente --network red_practica ubuntu bash
```

Dentro del contenedor ejecutar:

```bash
apt update
apt install -y curl
curl http://servidor_web
```

**Resultado esperado:** Página HTML de bienvenida de Nginx.

✅ Los contenedores se comunican utilizando el nombre del contenedor.

---

## 1️⃣9️⃣ Inspeccionar la red

```bash
docker network inspect red_practica
```

**Resultado esperado:** Información JSON mostrando:
- Contenedores conectados
- Direcciones IP asignadas
- Configuración bridge

---

# 🧹 Limpieza de recursos

## 2️⃣0️⃣ Eliminar contenedores, red y volumen

```bash
docker rm -f nginx_bind servidor_web cliente contenedor_nuevo
docker network rm red_practica
docker volume rm volumen_practica
```

---

## 📊 Tabla de comandos utilizados

| Comando | Función |
|----------|----------|
| `docker volume create` | Crear volumen |
| `docker volume ls` | Ver volúmenes |
| `docker volume inspect` | Inspeccionar volumen |
| `docker network create` | Crear red |
| `docker network ls` | Ver redes |
| `docker network inspect` | Inspeccionar red |
| `docker run -v` | Montar volumen o bind mount |
| `docker exec` | Ejecutar comandos dentro del contenedor |
| `docker rm -f` | Eliminar contenedores |

---

## 💡 Buenas prácticas

### Usar volúmenes para datos importantes

```bash
docker volume create datos_mysql
docker run -d -v datos_mysql:/var/lib/mysql mysql
```

---

### Utilizar bind mounts en desarrollo

```bash
docker run -v ~/proyecto:/app node
```

---

### Crear redes separadas para aplicaciones

```bash
docker network create backend_red
```

---

### Usar nombres descriptivos

```bash
docker run --name servidor_nginx nginx
```

---

## 🔍 Troubleshooting

### Error: Volume already exists

```bash
docker volume rm volumen_practica
docker volume create volumen_practica
```

---

### Error: Network already exists

```bash
docker network rm red_practica
docker network create red_practica
```

---

### Contenedores no se comunican

Verificar red:

```bash
docker network inspect red_practica
```

---

### Puerto ocupado

Cambiar puerto del host:

```bash
docker run -d -p 8091:80 nginx
```

---

## 🎯 Tareas completadas

- ✅ Crear y usar volúmenes Docker
- ✅ Mantener datos persistentes
- ✅ Usar bind mounts
- ✅ Compartir archivos con el host
- ✅ Crear redes Docker
- ✅ Comunicar contenedores por nombre
- ✅ Inspeccionar redes y volúmenes
- ✅ Eliminar recursos correctamente

---
