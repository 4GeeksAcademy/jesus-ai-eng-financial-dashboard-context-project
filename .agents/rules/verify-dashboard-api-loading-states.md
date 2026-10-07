# Nombre
`verify-dashboard-api-loading-states`

## Alcance
Cambios en la carga de datos, el estado de carga o el manejo de errores de `frontend/src/App.tsx`.

## Justificación
`App` obtiene datos de `/api/metrics` y muestra un mensaje cuando la petición falla. Las pruebas frontend existentes cubren utilidades, por lo que no bastan para verificar el render ante distintos resultados de la API.

## Guía específica del proyecto
- Al cambiar esta lógica, inicia la aplicación con `docker compose up --build` y abre `http://localhost:5173` en un navegador. Con `/api/metrics` respondiendo HTTP 200, confirma que los skeletons desaparecen, aparecen los KPI/gráficos y no se muestra el mensaje de error.
- Para el caso de fallo, usa el bloqueo de solicitudes del navegador para bloquear `/api/metrics` y recarga. Confirma que aparece `No se pudo cargar la informacion financiera. Revisa la API de backend.` y que la carga termina; no aceptes skeletons permanentes ni un dashboard vacío como éxito.
- Desbloquea la solicitud, recarga y confirma que vuelve el dashboard sin el mensaje de error. Registra por separado los resultados de éxito y fallo; una respuesta HTTP inspeccionada con `curl` no sustituye la comprobación del render en navegador.