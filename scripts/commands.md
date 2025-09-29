# Comandos útiles

## Docker Run (rápido)
```
docker volume create n8n_data

docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -e GENERIC_TIMEZONE="America/Bogota" \
  -e TZ="America/Bogota" \
  -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true \
  -e N8N_RUNNERS_ENABLED=true \
  -v n8n_data:/home/node/.n8n \
  docker.n8n.io/n8nio/n8n
```

## Docker Compose (SQLite)
```
docker compose -f docker/docker-compose.sqlite.yml --env-file docker/.env up -d
```

## Docker Compose (Postgres)
```
docker compose -f docker/docker-compose.postgres.yml --env-file docker/.env up -d
```

## Actualización
```
docker pull docker.n8n.io/n8nio/n8n
# luego reinicia el contenedor con tus parámetros
```
