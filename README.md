# n8n Starter – Tutorial Teórico (ES)

Este repositorio está diseñado como un tutorial teórico para entender n8n: qué es, cómo funciona, cómo instalarlo/operarlo con Docker y buenas prácticas para auto-hospedarlo con seguridad.

Si buscas una guía práctica paso a paso con comandos probados, también encontrarás ejemplos de Docker Compose y ejercicios teóricos para afianzar conceptos.

## Contenido

- Conceptos básicos de n8n: ¿qué es y para qué sirve?
- Arquitectura y componentes clave
- Instalación con Docker (SQLite por defecto y PostgreSQL)
- Actualización de n8n (Docker y Docker Compose)
- Uso de túnel para webhooks en desarrollo
- Configuración (variables de entorno y métodos)
- Seguridad y endurecimiento (SSO, SSL, bloquear nodos, etc.)
- Escalado, colas (Queue Mode) y Runners
- Logs, monitoreo y resolución de errores comunes
- Ejercicios teóricos para reforzar el aprendizaje
- Recursos oficiales y comunidad

## Estructura del repo

```
.
├─ README.md
├─ docs/
│  ├─ 01-que-es-n8n.md
│  ├─ 02-arquitectura.md
│  ├─ 03-instalacion-docker.md
│  ├─ 04-postgresql.md
│  ├─ 05-actualizacion.md
│  ├─ 06-tunel-webhooks.md
│  ├─ 07-configuracion.md
│  ├─ 08-seguridad.md
│  ├─ 09-escalado-rendimiento.md
│  ├─ 10-logs-monitoreo.md
│  ├─ 15-ejercicios-teoricos.md
│  └─ 99-recursos.md
├─ docker/
│  ├─ docker-compose.sqlite.yml
│  ├─ docker-compose.postgres.yml
│  └─ .env.example
└─ scripts/
   └─ commands.md
```

## Requisitos previos (auto-hosting)

- Conocimientos de contenedores (Docker/Compose) y redes
- Gestión de recursos (CPU/Mem/Disco) y escalado básico
- Buenas prácticas de seguridad (SSL/Firewall/Usuario y permisos)

Si no tienes experiencia administrando servidores, considera usar n8n Cloud.

## Empezar rápido (modo lectura recomendada)

- Lee `docs/01-que-es-n8n.md` y `docs/02-arquitectura.md` para el contexto.
- Revisa `docs/03-instalacion-docker.md` para levantar n8n en Docker con SQLite.
- Si necesitas base de datos externa, mira `docs/04-postgresql.md` y `docker/docker-compose.postgres.yml`.
- Para actualizar, consulta `docs/05-actualizacion.md`.
- Para pruebas de webhooks desde servicios externos, usa `docs/06-tunel-webhooks.md`.

## Cómo ejecutar este proyecto (Docker Compose)

Sigue estos pasos tal como están para que funcione al clonar este repositorio.

1) Clonar y entrar al proyecto

```
git clone <URL_DE_TU_REPO> n8n-starter
cd n8n-starter
```

2) Crear el archivo de variables de entorno

```
cp docker/.env.example docker/.env
```

Edita `docker/.env` y ajusta al menos:
- `TZ` y `GENERIC_TIMEZONE` (por ejemplo `America/Bogota`)
- Opcional: `N8N_ENCRYPTION_KEY` (recomendado en entornos persistentes)
- Si usarás PostgreSQL: credenciales fuertes en `DB_POSTGRESDB_*`

3) Elegir opción de despliegue

- Opción A – SQLite (más simple, desarrollo):

```
docker compose -f docker/docker-compose.sqlite.yml --env-file docker/.env up -d
```

- Opción B – PostgreSQL (recomendada para producción):

```
docker compose -f docker/docker-compose.postgres.yml --env-file docker/.env up -d
```

4) Verificar estado y logs

```
# Estado de los servicios
docker compose -f docker/docker-compose.postgres.yml --env-file docker/.env ps

# Logs de n8n (cambiar el archivo -f si usaste sqlite)
docker compose -f docker/docker-compose.postgres.yml --env-file docker/.env logs -f n8n
```

5) Acceder a la interfaz

Abre en tu navegador:

```
http://localhost:5678
```

6) Detener y limpiar (cuando termines)

```
docker compose -f docker/docker-compose.postgres.yml --env-file docker/.env down
```

Notas importantes
- El campo `version` en los archivos `docker-compose.*.yml` es obsoleto y Docker Compose lo ignora; puedes dejarlo o quitarlo sin afectar la ejecución.
- Si necesitas leer variables de entorno desde Code Node/expresiones, añade en `docker/.env`:
  - `N8N_BLOCK_ENV_ACCESS_IN_NODE=false`
- Por seguridad del Git Node, si no usas repos bare, añade en `docker/.env`:
  - `N8N_GIT_NODE_DISABLE_BARE_REPOS=true`
- En producción define `N8N_ENCRYPTION_KEY` para portabilidad de claves.
- Si expones n8n en internet, usa reverse proxy + SSL y ajusta `WEBHOOK_URL` a tu dominio HTTPS.

## Licencia

Este contenido se ofrece con fines educativos. Verifica licencias y requisitos de n8n para entornos productivos.
