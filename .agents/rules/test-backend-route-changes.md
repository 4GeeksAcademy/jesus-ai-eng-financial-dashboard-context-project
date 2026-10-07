# Nombre
`test-backend-route-changes`

## Alcance
Cambios en el comportamiento de endpoints definidos en `backend/app/routes.py`.

## Justificación
`backend/tests/test_routes.py` prueba las rutas con `TestClient` y funciones con prefijo `test_`. Mantener estas pruebas junto al comportamiento de la API ayuda a detectar cambios de contrato y respuestas inesperadas.

## Guía específica del proyecto
- Al cambiar el comportamiento de un endpoint, añade o actualiza una prueba en `backend/tests/test_routes.py`.
- Usa `TestClient`, los helpers existentes y nombres de función con prefijo `test_`.
- Comprueba el código de estado y los datos de respuesta relevantes al cambio.
- Desde la raíz del proyecto, ejecuta `python -m pytest backend/tests`.