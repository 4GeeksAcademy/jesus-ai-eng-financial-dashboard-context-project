# Nombre
`test-frontend-utility-changes`

## Alcance
Cambios de comportamiento en las utilidades financieras de `frontend/src/lib`.

## Justificación
`frontend/src/lib/financial-utils.test.ts` utiliza Vitest para probar `computeKPIs`, `computeMonthlyData` y los formateadores. Estas pruebas permiten detectar regresiones en los cálculos que alimentan el dashboard.

## Guía específica del proyecto
- Al modificar una utilidad financiera, actualiza sus pruebas en el archivo `.test.ts` correspondiente.
- Para las utilidades existentes de `financial-utils.ts`, reutiliza `frontend/src/lib/financial-utils.test.ts`.
- Cubre el comportamiento modificado y los casos límite relevantes al cambio.
- Ejecuta `cd frontend && npm test`.