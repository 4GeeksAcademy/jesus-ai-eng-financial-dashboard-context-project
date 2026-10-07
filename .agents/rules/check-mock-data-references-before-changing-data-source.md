# Nombre
`check-mock-data-references-before-changing-data-source`

## Alcance
Cambios en la fuente de datos del dashboard o en `frontend/src/lib/mock-data.ts`.

## Justificación
`mockMovements` está declarado en `mock-data.ts`, pero `frontend/src/App.tsx` carga datos desde `/api/metrics`. Modificar los mocks no implica que cambien los datos consumidos por la aplicación.

## Guía específica del proyecto
- Antes de cambiar la fuente o los mocks, ejecuta `rg -n "mockMovements" frontend/src` desde la raíz. Si `rg` no está instalado, usa `grep -R -n --exclude-dir=node_modules "mockMovements" frontend/src`.
- Revisa los resultados junto al `fetch` de `/api/metrics` en `frontend/src/App.tsx`.
- Confirma qué fuente consume realmente la aplicación y limita el cambio a la fuente y los consumidores pertinentes.
- No asumas que `mockMovements` alimenta el dashboard solo porque existe en el repositorio.