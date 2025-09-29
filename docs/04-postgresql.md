# n8n con PostgreSQL

Aunque n8n funciona con SQLite por defecto, en producción se recomienda PostgreSQL para mayor robustez y concurrencia.

## Variables de entorno clave

```
DB_TYPE=postgresdb
DB_POSTGRESDB_DATABASE=<DB_NAME>
DB_POSTGRESDB_HOST=<DB_HOST>
DB_POSTGRESDB_PORT=<DB_PORT>
DB_POSTGRESDB_USER=<DB_USER>
DB_POSTGRESDB_SCHEMA=<DB_SCHEMA>
DB_POSTGRESDB_PASSWORD=<DB_PASSWORD>
```

Sigue persistiendo el directorio `.n8n` para claves de cifrado, logs y assets de control de versiones.

## Ejemplo con Docker Run

```
docker volume create n8n_data

docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -e GENERIC_TIMEZONE="<TU_ZONA_HORARIA>" \
  -e TZ="<TU_ZONA_HORARIA>" \
  -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true \
  -e N8N_RUNNERS_ENABLED=true \
  -e DB_TYPE=postgresdb \
  -e DB_POSTGRESDB_DATABASE=<DB_NAME> \
  -e DB_POSTGRESDB_HOST=<DB_HOST> \
  -e DB_POSTGRESDB_PORT=<DB_PORT> \
  -e DB_POSTGRESDB_USER=<DB_USER> \
  -e DB_POSTGRESDB_SCHEMA=<DB_SCHEMA> \
  -e DB_POSTGRESDB_PASSWORD=<DB_PASSWORD> \
  -v n8n_data:/home/node/.n8n \
  docker.n8n.io/n8nio/n8n
```

## Docker Compose (Postgres)

Consulta `docker/docker-compose.postgres.yml`. Incluye servicios de `postgres` y `n8n` y utiliza variables de `.env`.

```
docker compose -f docker/docker-compose.postgres.yml --env-file docker/.env up -d
```

## Buenas prácticas
- Usa contraseñas fuertes y restringe el acceso de red a Postgres
- Realiza backups (base de datos + `.n8n`)
- Define `N8N_ENCRYPTION_KEY` para portabilidad de claves
