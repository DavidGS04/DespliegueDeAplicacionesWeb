# 🐳 Actividad Docker #6 - Construcción de Imágenes Docker Personalizadas

## 👨‍💻 Autor

**David Garrido Suárez**

---

# 📌 Objetivo de la actividad

Aprender a generar imágenes personalizadas, optimizar tamaños y aplicar configuraciones seguras en Docker.

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

### Dockerfile
Archivo donde se definen las instrucciones necesarias para construir una imagen Docker.

### Imagen Docker
Plantilla inmutable usada para crear contenedores.

### Build Context
Directorio que Docker utiliza durante la construcción.

### Multi-stage Build
Método para crear imágenes más ligeras utilizando varias fases.

---

# 📦 Instrucciones fundamentales de Dockerfile

| Instrucción | Función |
|---|---|
| `FROM` | Define imagen base |
| `WORKDIR` | Directorio de trabajo |
| `COPY` | Copia archivos |
| `RUN` | Ejecuta comandos |
| `EXPOSE` | Expone puertos |
| `CMD` | Comando principal |
| `LABEL` | Añade metadatos |
| `USER` | Usuario del contenedor |

---

# 🛠️ PRÁCTICA 1 - Imagen Python personalizada

## PASO 1.1: Crear directorio de trabajo

```bash
mkdir -p ~/actividad-python-docker
cd ~/actividad-python-docker
```

---

## PASO 1.2: Crear Dockerfile

```bash
cat > Dockerfile << 'EOF'
FROM python:3.10-slim

WORKDIR /srv/app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY servidor.py .

EXPOSE 5050

CMD ["python", "servidor.py"]
EOF
```

---

## PASO 1.3: Crear dependencias

```bash
cat > requirements.txt << 'EOF'
flask==2.2.5
EOF
```

---

## PASO 1.4: Crear aplicación Python

```bash
cat > servidor.py << 'EOF'
from http.server import HTTPServer, BaseHTTPRequestHandler

class Web(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header("Content-type","text/html")
        self.end_headers()
        self.wfile.write(b"""
        <html>
        <body>
            <h1>Servidor Python Docker</h1>
            <p>Actividad Docker completada correctamente</p>
        </body>
        </html>
        """)

server = HTTPServer(("0.0.0.0", 5050), Web)
print("Servidor iniciado en puerto 5050")
server.serve_forever()
EOF
```

---

## PASO 1.5: Construir imagen

```bash
docker build -t python-custom:v1 .
```

---

## PASO 1.6: Verificar imagen creada

```bash
docker images
```

---

## PASO 1.7: Ejecutar contenedor

```bash
docker run -d --name servidor-python -p 5050:5050 python-custom:v1
```

---

## PASO 1.8: Comprobar funcionamiento

```bash
curl http://localhost:5050
```

---

## PASO 1.9: Consultar logs

```bash
docker logs servidor-python
```

---

## PASO 1.10: Eliminar contenedor

```bash
docker rm -f servidor-python
```

---

# 🛠️ PRÁCTICA 2 - Imagen Node.js

## PASO 2.1: Crear carpeta

```bash
mkdir -p ~/actividad-node
cd ~/actividad-node
```

---

## PASO 2.2: Crear Dockerfile

```bash
cat > Dockerfile << 'EOF'
FROM node:18-alpine

WORKDIR /usr/src/app

COPY app.js .

EXPOSE 4000

CMD ["node", "app.js"]
EOF
```

---

## PASO 2.3: Crear aplicación Node.js

```bash
cat > app.js << 'EOF'
const http = require('http');

http.createServer((req,res)=>{
    res.writeHead(200,{'Content-Type':'text/html'});
    res.end('<h1>Servidor Node.js con Docker</h1>');
}).listen(4000);

console.log("Servidor Node.js ejecutándose");
EOF
```

---

## PASO 2.4: Construir imagen

```bash
docker build -t node-custom:v1 .
```

---

## PASO 2.5: Ejecutar contenedor

```bash
docker run -d --name node-server -p 4000:4000 node-custom:v1
```

---

## PASO 2.6: Verificar acceso

```bash
curl http://localhost:4000
```

---

## PASO 2.7: Revisar contenedor activo

```bash
docker ps
```

---

## PASO 2.8: Eliminar contenedor

```bash
docker rm -f node-server
```

---

# 🛠️ PRÁCTICA 3 - Imagen optimizada Multi-stage

## PASO 3.1: Crear estructura

```bash
mkdir -p ~/docker-multistage
cd ~/docker-multistage
```

---

## PASO 3.2: Crear Dockerfile

```bash
cat > Dockerfile << 'EOF'
FROM node:18 AS builder

WORKDIR /build

COPY package.json .

RUN npm install

COPY . .

FROM node:18-alpine

WORKDIR /app

COPY --from=builder /build .

EXPOSE 3000

CMD ["npm","start"]
EOF
```

---

## PASO 3.3: Crear package.json

```bash
cat > package.json << 'EOF'
{
  "name":"multistage-app",
  "version":"1.0.0",
  "main":"index.js",
  "scripts":{
    "start":"node index.js"
  }
}
EOF
```

---

## PASO 3.4: Crear index.js

```bash
cat > index.js << 'EOF'
console.log("Aplicación multi-stage funcionando");
setInterval(()=>{},1000);
EOF
```

---

## PASO 3.5: Construir imagen

```bash
docker build -t multistage-app:v1 .
```

---

## PASO 3.6: Comparar tamaños

```bash
docker images
```

---

# 🛠️ PRÁCTICA 4 - Seguridad y mejores prácticas

## PASO 4.1: Crear directorio

```bash
mkdir -p ~/docker-secure
cd ~/docker-secure
```

---

## PASO 4.2: Crear .dockerignore

```bash
cat > .dockerignore << 'EOF'
.git
node_modules
README.md
.env
EOF
```

---

## PASO 4.3: Crear Dockerfile seguro

```bash
cat > Dockerfile << 'EOF'
FROM node:18-alpine

LABEL autor="David Garrido Suárez"
LABEL version="1.0"

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /home/app

COPY --chown=appuser:appgroup index.js .

USER appuser

EXPOSE 3000

CMD ["node","index.js"]
EOF
```

---

## PASO 4.4: Crear archivo index.js

```bash
cat > index.js << 'EOF'
console.log("Contenedor ejecutándose con usuario no-root");
setInterval(()=>{},1000);
EOF
```

---

## PASO 4.5: Construir imagen

```bash
docker build -t secure-node:v1 .
```

---

## PASO 4.6: Verificar usuario

```bash
docker run --rm secure-node:v1 whoami
```

---

# 🛠️ PRÁCTICA 5 - Imagen Alpine mínima

## PASO 5.1: Crear directorio

```bash
mkdir -p ~/docker-alpine
cd ~/docker-alpine
```

---

## PASO 5.2: Crear Dockerfile

```bash
cat > Dockerfile << 'EOF'
FROM alpine:3.18

RUN apk add --no-cache curl bash nano

CMD ["sh"]
EOF
```

---

## PASO 5.3: Construir imagen

```bash
docker build -t alpine-tools:v1 .
```

---

## PASO 5.4: Ver tamaño de imagen

```bash
docker images | grep alpine-tools
```

---

## PASO 5.5: Ejecutar contenedor

```bash
docker run --rm alpine-tools:v1 curl --version
```

---

# 📊 Comparativa de tamaños

| Imagen | Tamaño aproximado |
|---|---|
| python:3.10-slim | 140MB |
| node:18 | 900MB |
| node:18-alpine | 120MB |
| alpine:3.18 | 5MB |

---

# 📋 Tabla de comandos usados

| Comando | Descripción |
|---|---|
| `docker build` | Construir imagen |
| `docker images` | Listar imágenes |
| `docker run` | Ejecutar contenedor |
| `docker logs` | Ver logs |
| `docker history` | Ver capas |
| `docker inspect` | Inspeccionar imagen |
| `docker rm -f` | Eliminar contenedor |
| `docker rmi` | Eliminar imagen |

---

# 💡 Buenas prácticas aplicadas

## Utilizar imágenes ligeras

```dockerfile
FROM alpine:3.18
```

---

## Usar usuario no-root

```dockerfile
USER appuser
```

---

## Minimizar capas

```dockerfile
RUN apk add --no-cache curl bash
```

---

## Añadir .dockerignore

```bash
node_modules
.git
.env
```

---

# 🧹 Limpieza final

```bash
docker rm -f servidor-python node-server 2>/dev/null

docker rmi python-custom:v1 \
node-custom:v1 \
multistage-app:v1 \
secure-node:v1 \
alpine-tools:v1
```

---

# 🎯 Objetivos completados

- ✅ Creación de imágenes Docker
- ✅ Uso de Dockerfile
- ✅ Ejecución de contenedores personalizados
- ✅ Multi-stage build
- ✅ Optimización de tamaño
- ✅ Uso de usuario no-root
- ✅ Imágenes Alpine
- ✅ Gestión de capas

---
