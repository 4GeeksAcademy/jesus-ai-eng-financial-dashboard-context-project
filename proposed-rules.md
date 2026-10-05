# Reglas propuestas

Estas reglas se derivan de los hallazgos de [verification.md](verification.md). Son propuestas para revisión; todavía no se han incorporado a `.agents/rules`.

## Arquitectura

### `place-frontend-code-by-responsibility`
- **Categoría:** Arquitectura
- **Análisis:** `App.tsx` compone componentes de `components/dashboard` y lógica de `lib`; `kpi-row.tsx` importa otro componente del dashboard ([frontend/src/App.tsx](frontend/src/App.tsx#L2), [frontend/src/components/dashboard/kpi-row.tsx](frontend/src/components/dashboard/kpi-row.tsx#L1)).
- **Notas / regla:** Al añadir una pieza del dashboard, colócala en `components/dashboard`; si es un componente visual reutilizable, en `components/ui`; los tipos y cálculos compartidos van en `lib`.
- **Comprobación:** Revisar que el archivo nuevo esté en la carpeta correspondiente y que sus imports sigan esos límites.

### `keep-vite-and-typescript-aliases-aligned`
- **Categoría:** Arquitectura / DX
- **Análisis:** TypeScript y Vite configuran el alias `@` para apuntar a `src` ([frontend/tsconfig.app.json](frontend/tsconfig.app.json#L12), [frontend/vite.config.ts](frontend/vite.config.ts#L18)).
- **Notas / regla:** Al cambiar el alias `@`, actualízalo tanto en `tsconfig.app.json` como en `vite.config.ts`.
- **Comprobación:** Ejecutar `cd frontend && npm run build`.

## Testing

### `test-frontend-utility-changes`
- **Categoría:** Testing
- **Análisis:** `financial-utils.test.ts` usa Vitest para probar `computeKPIs`, `computeMonthlyData` y los formatters ([frontend/package.json](frontend/package.json#L11), [frontend/src/lib/financial-utils.test.ts](frontend/src/lib/financial-utils.test.ts#L35)).
- **Notas / regla:** Al cambiar una utilidad financiera, actualiza sus pruebas Vitest en el archivo `.test.ts` correspondiente.
- **Comprobación:** Ejecutar `cd frontend && npm test`.

### `test-backend-route-changes`
- **Categoría:** Testing
- **Análisis:** Las pruebas backend usan `TestClient` y funciones con prefijo `test_` en `backend/tests/test_routes.py` ([backend/tests/test_routes.py](backend/tests/test_routes.py#L3), [backend/tests/test_routes.py](backend/tests/test_routes.py#L29)).
- **Notas / regla:** Al cambiar el comportamiento de un endpoint, actualiza una prueba de ruta con `TestClient` en `backend/tests/test_routes.py`.
- **Comprobación:** Ejecutar `python -m pytest backend/tests`.

### `verify-dashboard-api-loading-states`
- **Categoría:** Testing
- **Análisis:** `App` carga `/api/metrics` y muestra un mensaje en caso de error; la suite frontend existente cubre utilidades ([frontend/src/App.tsx](frontend/src/App.tsx#L16), [frontend/src/App.tsx](frontend/src/App.tsx#L34), [frontend/src/lib/financial-utils.test.ts](frontend/src/lib/financial-utils.test.ts#L35)).
- **Notas / regla:** Al cambiar la carga de datos o el manejo de errores de `App`, verifica el render con una respuesta correcta y con un fallo de red o una respuesta no exitosa.
- **Comprobación:** Probar en el navegador con la API disponible y luego inaccesible, comprobando el mensaje de error.

## Datos e interfaz

### `keep-period-label-aligned-with-generated-data`
- **Categoría:** Datos / Interfaz
- **Análisis:** La interfaz fija el período como `2024 - Full Year`, mientras el backend usa `date.today()` para generar fechas ([frontend/src/App.tsx](frontend/src/App.tsx#L49), [backend/app/routes.py](backend/app/routes.py#L97)).
- **Notas / regla:** Al cambiar el período mostrado o la generación de fechas, actualiza ambos para que la etiqueta corresponda a las fechas devueltas por la API.
- **Comprobación:** Comparar el período del encabezado con las fechas mínima y máxima de `GET /api/metrics`.

### `check-mock-data-references-before-changing-data-source`
- **Categoría:** Datos / Mantenimiento
- **Análisis:** `mockMovements` está declarado en `mock-data.ts`, mientras `App` carga datos desde `/api/metrics` ([frontend/src/lib/mock-data.ts](frontend/src/lib/mock-data.ts#L3), [frontend/src/App.tsx](frontend/src/App.tsx#L16)).
- **Notas / regla:** Al cambiar la fuente de datos del dashboard o los datos mock, busca referencias a `mockMovements` y comprueba qué fuente consume realmente la app.
- **Comprobación:** Ejecutar `rg -n "mockMovements" frontend/src` y revisar los resultados junto al `fetch` de `App`.

## DX y documentación

### `keep-compose-api-proxy-target-valid`
- **Categoría:** DX / Configuración
- **Análisis:** El proxy de Vite apunta a `http://backend:8000`, el hostname del servicio de Compose ([frontend/vite.config.ts](frontend/vite.config.ts#L13)).
- **Notas / regla:** Al cambiar el proxy de `/api` o el servicio backend de Compose, mantén alineados el destino de Vite y el nombre del servicio.
- **Comprobación:** Ejecutar `docker compose config` y, con Compose iniciado, solicitar `http://localhost:5173/api/metrics`.

### `verify-clean-frontend-dependency-build`
- **Categoría:** DX / Dependencias
- **Análisis:** La imagen instala dependencias con `npm install`, el manifiesto usa rangos de versión y no se encontró un lockfile ([frontend/Dockerfile](frontend/Dockerfile#L5), [frontend/package.json](frontend/package.json#L19), [frontend/package.json](frontend/package.json#L39)).
- **Notas / regla:** Al modificar dependencias frontend, comprueba que la imagen se construya desde cero.
- **Comprobación:** Ejecutar `docker compose build --no-cache frontend`.

### `keep-agent-directory-docs-in-sync`
- **Categoría:** Documentación
- **Análisis:** El README describe `.agents/rules` y `.agents/skills`, pero esas carpetas no estaban presentes en el checkout revisado ([README.es.md](README.es.md#L28)).
- **Notas / regla:** Al añadir o retirar carpetas bajo `.agents`, actualiza la estructura que documenta el README.
- **Comprobación:** Comparar el árbol documentado con `find .agents -maxdepth 3 -type f`.

## Seguridad

### `review-debugger-listener-and-port-publication-together`
- **Categoría:** Seguridad / Configuración
- **Análisis:** `debugpy` escucha en `0.0.0.0:5678` y Compose publica `5678:5678` ([backend/Dockerfile](backend/Dockerfile#L12), [docker-compose.yml](docker-compose.yml#L20)).
- **Notas / regla:** Al modificar el arranque o los puertos publicados del backend, revisa conjuntamente el listener de `debugpy` y su mapeo en Compose.
- **Comprobación:** Inspeccionar esas dos configuraciones y la salida de `docker compose config`.

### `review-cors-origins-and-credentials-together`
- **Categoría:** Seguridad
- **Análisis:** CORS se configura con `allow_origins=["*"]` y `allow_credentials=True` ([backend/app/main.py](backend/app/main.py#L9), [backend/app/main.py](backend/app/main.py#L10)).
- **Notas / regla:** Al cambiar CORS, revisa `allow_origins` y `allow_credentials` como una sola configuración y verifica la respuesta preflight.
- **Comprobación:** Inspeccionar ambos valores y comprobar la respuesta `OPTIONS` de `/api/metrics` con un encabezado `Origin`.

## Observaciones sin regla propuesta

No se fija una convención para el formato de commits, las comillas o el idioma de la interfaz: los ejemplos observados son inconsistentes y no permiten derivar una regla única sin añadir una decisión nueva.
