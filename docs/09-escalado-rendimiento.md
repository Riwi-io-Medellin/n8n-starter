# Escalado, rendimiento y colas

## Modos de ejecución
- Estándar: rápido de iniciar, menos escalable
- Queue Mode: orquestador + runners, recomendado para cargas altas

## Runners
- Permiten ejecución distribuida y paralela
- Configura concurrencia y límites para evitar sobrecarga

## Control de concurrencia
- A nivel de instancia y de workflow
- Evita condiciones de carrera y uso excesivo de recursos

## Datos binarios y almacenamiento
- Usa almacenamiento externo para grandes binarios (S3, etc.)

## Métricas y benchmarking
- Mide tiempos de ejecución, colas y consumo de recursos
- Ajusta workers/concurrencia en base a métricas
