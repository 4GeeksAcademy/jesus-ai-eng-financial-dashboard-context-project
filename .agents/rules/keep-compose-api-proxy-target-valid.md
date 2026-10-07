# Nombre
`keep-compose-api-proxy-target-valid`

## Alcance
Cambios del proxy de `/api` en Vite o del servicio backend en `docker-compose.yml`.

## Justificación
El proxy de `frontend/vite.config.ts` apunta a `http://backend:8000`, cuyo hostname corresponde al servicio de Compose. Un cambio de nombre o puerto sin actualizar el proxy rompe las solicitudes del frontend a la API.

## Guía específica del proyecto
- Revisa conjuntamente el destino del proxy en `frontend/vite.config.ts` y el nombre y puerto del servicio en `docker-compose.yml`.
- Mantén el destino del proxy resoluble desde el contenedor frontend y alineado con el puerto del backend.
- Ejecuta `docker compose config` desde la raíz y confirma que el destino del proxy coincide con el nombre del servicio y el puerto interno del backend.
- Inicia el stack con `docker compose up --build` y solicita `http://localhost:5173/api/metrics`; el criterio de éxito es HTTP 200 con JSON, no basta con que la API responda directamente en el host.
- Si falla el proxy, compara la respuesta directa de `http://localhost:8000/api/metrics` y comprueba desde el contenedor frontend que el nombre del servicio resuelve y que se puede conectar al puerto interno. Registra si el fallo es de configuración o de conectividad del entorno; no declares el proxy verificado mientras la petición proxificada falle.