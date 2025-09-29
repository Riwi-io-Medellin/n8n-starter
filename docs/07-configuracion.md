# Configuración de n8n

## Métodos de configuración
- Variables de entorno (preferido en contenedores)
- Archivo de configuración
- Parámetros de línea de comandos

## Variables de entorno comunes

```
# Zona horaria
TZ="America/Bogota"
GENERIC_TIMEZONE="America/Bogota"

# Seguridad / cifrado
N8N_ENCRYPTION_KEY="<cadena-segura>"
N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true

# Runners / ejecución
N8N_RUNNERS_ENABLED=true

# Base de datos (ver capítulo Postgres)
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=postgres
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=n8n
DB_POSTGRESDB_USER=n8n
DB_POSTGRESDB_PASSWORD=strongpassword
DB_POSTGRESDB_SCHEMA=public

# Exposición/host
N8N_PORT=5678
N8N_HOST=0.0.0.0
WEBHOOK_URL=https://tu-dominio

# Logs/monitoring
N8N_LOG_LEVEL=info
```

Consulta la documentación oficial para el listado completo de variables y ejemplos avanzados.
