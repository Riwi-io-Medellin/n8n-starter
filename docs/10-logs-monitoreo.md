# Logs y monitoreo

## Logs
- Configura `N8N_LOG_LEVEL` (debug, info, warn, error)
- Centraliza logs (ELK, Loki, etc.) para análisis

## Monitoreo
- Supervisa salud de contenedor, CPU, RAM, disco
- Exporta métricas (si aplica) a Prometheus/Grafana

## Errores comunes
- Falta de permisos en `.n8n`
- Variables de entorno incompletas
- Problemas de red con Postgres
- Puertos ocupados

Incluye alertas tempranas para evitar caídas en producción.
