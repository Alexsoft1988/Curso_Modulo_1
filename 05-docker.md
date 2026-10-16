# 🐳 Docker y Docker Compose en Amazon Linux 2023

Instalaremos **Docker, Docker Compose y Buildx** en la instancia EC2 creada para el laboratorio.

## 1. Conectarse a la EC2

```bash
ssh -i "mi-clave.pem" ec2-user@IP_PUBLICA
```

## 2. Actualizar el sistema

```bash
sudo dnf update -y
```

## 3. Instalar Docker

```bash
sudo dnf install -y docker
sudo systemctl enable --now docker
```

Agregar el usuario al grupo Docker:

```bash
sudo usermod -aG docker ec2-user
```

Cerrar la sesión y volver a conectarse:

```bash
exit
```

## 4. Probar Docker

```bash
docker --version
docker run --rm hello-world
```

## 5. Instalar Docker Compose y Buildx

Instalar `curl`:

```bash
sudo dnf install -y curl
sudo mkdir -p /usr/local/lib/docker/cli-plugins
```

### Docker Compose

```bash
ARCH=$(uname -m)

if [ "$ARCH" = "x86_64" ]; then
  COMPOSE_ARCH="x86_64"
elif [ "$ARCH" = "aarch64" ]; then
  COMPOSE_ARCH="aarch64"
fi

sudo curl -SL   "https://github.com/docker/compose/releases/latest/download/docker-compose-linux-${COMPOSE_ARCH}"   -o /usr/local/lib/docker/cli-plugins/docker-compose

sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```

### Docker Buildx

```bash
if [ "$ARCH" = "x86_64" ]; then
  BUILDX_ARCH="amd64"
elif [ "$ARCH" = "aarch64" ]; then
  BUILDX_ARCH="arm64"
fi

BUILDX_VERSION=$(curl -s https://api.github.com/repos/docker/buildx/releases/latest |   grep '"tag_name":' | cut -d'"' -f4)

sudo curl -SL   "https://github.com/docker/buildx/releases/download/${BUILDX_VERSION}/buildx-${BUILDX_VERSION}.linux-${BUILDX_ARCH}"   -o /usr/local/lib/docker/cli-plugins/docker-buildx

sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-buildx
```

Probar:

```bash
docker compose version
docker buildx version
```

> Usaremos **Docker Compose V2**, por eso el comando es `docker compose`.
> **Buildx** permite construir imágenes y es utilizado por Docker Compose cuando el proyecto contiene un `Dockerfile`.

## 6. Probar Docker Compose

Crear un directorio:

```bash
mkdir ~/docker-compose-lab
cd ~/docker-compose-lab
```

Crear `compose.yaml`:

```yaml
services:
  nginx:
    image: nginx:alpine
    ports:
      - "8080:80"
```

Levantar el contenedor:

```bash
docker compose up -d
```

Ver el servicio:

```bash
docker compose ps
```

Detener y eliminar el contenedor:

```bash
docker compose down
```

## 7. Listo para Jenkins

La instancia queda preparada con:

- Docker
- Docker Compose V2
- Docker Buildx
- `ec2-user` configurado para utilizar Docker

En el siguiente laboratorio instalaremos **Jenkins**.

