# Nombre
`keep-period-label-aligned-with-generated-data`

## Alcance
Cambios en el período mostrado por el dashboard o en la generación de fechas de la API.

## Justificación
Las reglas propuestas identifican una etiqueta fija `2024 - Full Year` en `frontend/src/App.tsx`, mientras `backend/app/routes.py` genera fechas con `date.today()`. Esta diferencia puede hacer que el encabezado describa un período distinto al de los datos.

## Guía específica del proyecto
- Al modificar el período mostrado o la generación de fechas, revisa conjuntamente `frontend/src/App.tsx` y `backend/app/routes.py`.
- Actualiza la etiqueta y la generación de datos según sea necesario para que describan el mismo período.
- Consulta `GET /api/metrics`, identifica las fechas mínima y máxima devueltas y compáralas con el período del encabezado.