# Instalación con Docker (SQLite por defecto)

n8n recomienda Docker para la mayoría de escenarios de auto-hosting por su aislamiento y facilidad de despliegue.

## Prerrequisitos
- Docker Engine y Docker Compose instalados
- Puerto 5678 disponible en el host
- Conocer tu zona horaria (por ejemplo: "America/Bogota")

## Inicio rápido

Crea un volumen persistente y levanta el contenedor:

```
docker volume create n8n_data

docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -e GENERIC_TIMEZONE="<TU_ZONA_HORARIA>" \
  -e TZ="<TU_ZONA_HORARIA>" \
  -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true \
  -e N8N_RUNNERS_ENABLED=true \
  -v n8n_data:/home/node/.n8n \
  docker.n8n.io/n8nio/n8n
```

- Mapea 5678/tcp al host
- Configura la zona horaria para el sistema y los nodos de agenda
- Enforce de permisos seguros del archivo de configuración
- Habilita Task Runners (recomendado)
- Persiste el directorio `/home/node/.n8n` (claves, logs, assets, etc.)

Accede a n8n en: http://localhost:5678

## Docker Compose (SQLite)

Consulta `docker/docker-compose.sqlite.yml` y `.env.example` para variables.

```
# Ejemplo básico
docker compose -f docker/docker-compose.sqlite.yml --env-file docker/.env up -d
```

## Notas
- En entornos productivos, considera usar PostgreSQL (ver siguiente capítulo)
- Asegura el acceso (SSL/Reverse Proxy/Firewall)
- Realiza backups periódicos del volumen `n8n_data`
