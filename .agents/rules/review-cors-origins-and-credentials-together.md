# Nombre
`review-cors-origins-and-credentials-together`

## Alcance
Cambios de la configuración CORS en `backend/app/main.py`.

## Justificación
Las reglas propuestas identifican `allow_origins=["*"]` junto con `allow_credentials=True`. Los orígenes permitidos y el uso de credenciales condicionan conjuntamente qué solicitudes acepta el navegador.

## Guía específica del proyecto
- Al modificar CORS, revisa `allow_origins` y `allow_credentials` como una sola configuración en `backend/app/main.py`.
- Antes de cambiar la configuración, anota el origen frontend esperado para el entorno y si las solicitudes necesitan credenciales; no deduzcas que `*` es la política deseada.
- Envía una petición `OPTIONS` a `/api/metrics` con ese `Origin` y `Access-Control-Request-Method: GET`.
- El preflight debe responder HTTP 200 y `Access-Control-Allow-Origin` debe coincidir con el origen esperado. Exige `Access-Control-Allow-Credentials: true` solo si el entorno necesita credenciales; registra el resultado y cualquier diferencia de política.
- Repite el preflight con un origen de prueba que no esté en la lista permitida. La respuesta no debe autorizar ese origen; si devuelve ese mismo origen en `Access-Control-Allow-Origin`, la política permite orígenes no confiables y el cambio no pasa la revisión.