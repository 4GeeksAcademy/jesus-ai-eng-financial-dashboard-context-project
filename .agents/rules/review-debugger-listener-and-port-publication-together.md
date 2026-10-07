# Nombre
`review-debugger-listener-and-port-publication-together`

## Alcance
Cambios del arranque del backend, del listener de depuración o de los puertos publicados en Compose.

## Justificación
`backend/Dockerfile` configura `debugpy` para escuchar en `0.0.0.0:5678` y `docker-compose.yml` publica `5678:5678`. La exposición efectiva del depurador depende de ambas configuraciones, no de una sola.

## Guía específica del proyecto
- Al modificar el arranque o los puertos del backend, revisa conjuntamente el listener de `debugpy` en `backend/Dockerfile` y su publicación en `docker-compose.yml`.
- Comprueba en `backend/Dockerfile` que listener e interfaz usan el mismo puerto interno; ejecuta `docker compose config` y confirma el puerto publicado y si se limita a una interfaz del host.
- Registra el entorno objetivo y si necesita depuración remota. Para desarrollo local, acepta la publicación solo si es intencional; para producción, no publiques `5678` salvo que exista una decisión explícita y controles de red documentados.
- Si el entorno o la necesidad no están definidos, informa que la exposición está pendiente de decisión y no la marques como aprobada por el mero hecho de que la configuración sea coherente.