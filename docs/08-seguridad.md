# Seguridad y endurecimiento

Buenas prácticas clave:
- HTTPS obligatorio (SSL/TLS). Usa reverse proxy (Nginx/Caddy/Traefik)
- Autenticación segura (considera SSO en Enterprise)
- Bloquea nodos sensibles si no se usan
- Deshabilita API pública si no es necesaria
- Usa usuarios/roles de base de datos mínimos
- Actualiza con regularidad y monitorea vulnerabilidades
- Aísla redes de contenedores y restringe puertos
- Backups verificados y pruebas de restauración

Recursos oficiales:
- Hardening Task Runners
- Set up SSL
- Set up SSO
- Disable the API
