# 🐳 Actividad Docker #1 — Instalación y configuración inicial de Docker en Ubuntu

## 👨‍💻 Autor

**David Garrido Suárez**

---

# 📌 Objetivo de la actividad

Implementar Docker Engine en Ubuntu 24.04 LTS, configurar el entorno correctamente y validar el funcionamiento del sistema de contenedores mediante pruebas básicas.

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

# ⚙️ 1. Actualización inicial del sistema

Antes de instalar Docker se actualizan los paquetes del sistema y repositorios.

## 💻 Comandos ejecutados

```bash id="3c4z5v"
sudo apt update
sudo apt upgrade -y
```

## ✅ Resultado

* Sistema actualizado correctamente.
* Repositorios sincronizados.
* Dependencias listas para instalación.

---

# 📦 2. Instalación de paquetes requeridos

Docker necesita herramientas adicionales para trabajar con repositorios HTTPS y claves GPG.

## 💻 Instalación

```bash id="j4c1m2"
sudo apt install -y \
apt-transport-https \
ca-certificates \
curl \
gnupg \
lsb-release
```

## ✅ Resultado

Todos los paquetes se instalaron correctamente sin conflictos.

---

# 🔑 3. Configuración de la clave GPG de Docker

Se añade la clave oficial del repositorio Docker para garantizar autenticidad y seguridad.

## 💻 Comando utilizado

```bash id="5t7a0u"
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/docker.gpg
```

## ✅ Resultado

La clave quedó almacenada correctamente en el sistema.

---

# 🌐 4. Agregar repositorio oficial Docker

Se registra el repositorio estable de Docker Community Edition.

## 💻 Configuración

```bash id="7l8e1k"
echo \
"deb [arch=amd64 signed-by=/usr/share/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## ✅ Resultado

Repositorio agregado correctamente al sistema.

---

# 🐳 5. Instalación de Docker Engine

Instalación del motor Docker y componentes principales.

## 💻 Instalación

```bash id="f2u8q3"
sudo apt update

sudo apt install -y \
docker-ce \
docker-ce-cli \
containerd.io \
docker-compose-plugin
```

## ✅ Resultado

Docker CE instalado correctamente junto al plugin Compose.

---

# 🔎 6. Verificación de instalación

Se comprueba la versión instalada de Docker.

## 💻 Verificación

```bash id="1r6x9n"
docker --version
```

## ✅ Resultado esperado

```bash id="d7m5v4"
Docker version 29.x.x
```

---

# 🔐 7. Configuración de permisos sin sudo

Para evitar ejecutar Docker como superusuario se añade el usuario actual al grupo `docker`.

## 💻 Configuración

```bash id="4g9n0x"
sudo groupadd docker

sudo usermod -aG docker $USER

newgrp docker
```

## ✅ Resultado

Usuario agregado correctamente al grupo Docker.

---

# 🚀 8. Primer contenedor: hello-world

Se ejecuta el contenedor oficial de prueba para validar el funcionamiento del daemon Docker.

## 💻 Ejecución

```bash id="2s5k8z"
docker run hello-world
```

## ✅ Resultado

Docker descarga automáticamente la imagen y ejecuta el contenedor correctamente.

---

# 📊 9. Comprobaciones del sistema Docker

## Ver estado del servicio

```bash id="0k3x7p"
sudo systemctl status docker
```

## Mostrar información detallada

```bash id="8u2m4y"
docker info
```

## Listar contenedores

```bash id="5j9q1n"
docker ps -a
```

## Listar imágenes descargadas

```bash id="3e1v6t"
docker images
```

## ✅ Resultado

* Docker activo y funcionando.
* Daemon operativo.
* Imagen `hello-world` descargada correctamente.

---

# ⚡ 10. Habilitar Docker al iniciar Ubuntu

## 💻 Comando

```bash id="9b4c7w"
sudo systemctl enable docker
```

## ✅ Resultado

Docker queda configurado para arrancar automáticamente con el sistema.

---

# 🧪 Validaciones realizadas

| Verificación              | Resultado |
| ------------------------- | --------- |
| Docker instalado          | ✅         |
| Daemon activo             | ✅         |
| Contenedor ejecutado      | ✅         |
| Usuario sin sudo          | ✅         |
| Docker Compose disponible | ✅         |
| Servicio persistente      | ✅         |

---

# 📚 Comandos principales utilizados

```bash id="8q5y2w"
docker --version
docker info
docker ps -a
docker images
docker run hello-world
sudo systemctl status docker
sudo systemctl enable docker
```

---

# 🔒 Seguridad aplicada

* Uso de repositorio oficial Docker
* Verificación mediante clave GPG
* Configuración de permisos mediante grupo `docker`
* Servicios administrados mediante systemd

---

# 📈 Resultado final

La instalación y configuración inicial de Docker se completó correctamente sobre Ubuntu 24.04, permitiendo:

* Ejecutar contenedores
* Gestionar imágenes Docker
* Administrar el daemon
* Trabajar sin privilegios root
* Preparar el entorno para futuras actividades

---
