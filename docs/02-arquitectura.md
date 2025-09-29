# Arquitectura de n8n

n8n se compone de varios elementos:

- Aplicación n8n: servidor principal con UI y API.
- Base de datos: por defecto SQLite; en producción se recomienda PostgreSQL.
- Runners/Queue: mecanismos para ejecutar tareas de forma escalable.
- Almacenamiento binario: disco local por defecto; opciones externas (S3, etc.).
- Webhooks: exponen endpoints para disparar workflows desde servicios externos.

Modos de ejecución:
- Modo estándar: todo en una sola instancia, útil para desarrollos y cargas bajas.
- Queue mode: separa la orquestación de la ejecución usando una cola (por ejemplo, Redis) y runners, recomendado para escalado.

Datos persistentes importantes:
- Directorio `.n8n`: claves de cifrado, logs, assets de control de versiones de workflows.
- Base de datos: credenciales, ejecuciones, workflows y configuraciones.

Diagrama (conceptual):

Cliente/Servicios → Webhooks/API → n8n (Orquestador/UI) → Runners → Integraciones/APIs/DBs
