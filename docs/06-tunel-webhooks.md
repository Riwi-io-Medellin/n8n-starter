# n8n con túnel (webhooks en desarrollo)

Uso recomendado solo para desarrollo y pruebas, NO para producción.

Permite recibir webhooks desde servicios externos en tu n8n local a través de un túnel gestionado por n8n.

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
  docker.n8n.io/n8nio/n8n \
  start --tunnel
```

Sigue las instrucciones en la UI para obtener la URL pública temporal. Recuerda deshabilitar el túnel en producción y usa alternativas seguras (reverse proxy + SSL).
