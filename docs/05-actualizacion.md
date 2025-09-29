# Actualización de n8n

## Obtener nuevas imágenes

```
# Última versión estable
docker pull docker.n8n.io/n8nio/n8n

# Versión específica
docker pull docker.n8n.io/n8nio/n8n:1.81.0

# Canal next (inestable)
docker pull docker.n8n.io/n8nio/n8n:next
```

## Reinicio del contenedor

```
docker ps -a

docker stop <container_id>
docker rm <container_id>

# volver a iniciar con tus opciones
docker run --name=<container_name> [opciones] -d docker.n8n.io/n8nio/n8n
```

## Docker Compose

```
# Dentro del directorio del compose
# docker compose pull
# docker compose down
# docker compose up -d
```

Asegúrate de conservar los volúmenes de datos (base de datos y `.n8n`). Revisa el changelog de n8n antes de actualizar en producción.
