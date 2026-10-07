# Verificación — Fase 1

## Cómo se ejecuta
- Arranque documentado: `docker compose up --build` ([README.es.md](README.es.md#L42)). `docker compose config` validó la configuración; no se ejecutó `docker compose up`, por lo que ninguna URL respondió durante esta verificación.
- Frontend: `http://localhost:5173`; Compose publica `5173:5173` ([docker-compose.yml](docker-compose.yml#L7)) y el Dockerfile pasa `--port 5173` a Vite ([frontend/Dockerfile](frontend/Dockerfile#L12)). El README también lista esa URL ([README.es.md](README.es.md#L48)).
- Backend: `http://localhost:8000`; Compose publica `8000:8000` ([docker-compose.yml](docker-compose.yml#L19)), Uvicorn escucha en 8000 ([backend/Dockerfile](backend/Dockerfile#L12)) y el README lista la URL ([README.es.md](README.es.md#L49)). `debugpy` escucha en 5678 ([backend/Dockerfile](backend/Dockerfile#L12)); el README no lo lista.
- Documentación API: README indica `http://localhost:8000/docs` ([README.es.md](README.es.md#L50)); no se comprobó respondiendo desde un backend iniciado.

## Afirmaciones verificadas
| Afirmación | Estado | Evidencia |
|---|---|---|
| Dashboard financiero con frontend React/TypeScript y backend FastAPI. | ✅ verificada | [README.es.md](README.es.md#L18): “Dashboard de métricas financieras con frontend en React + TypeScript y backend en FastAPI.” |
| Versiones declaradas: React `^19.2.4`, React DOM `^19.2.4`, TypeScript `~6.0.2`, Vite `^8.0.4` y Vitest `^4.1.4`; imágenes Node 24 y Python 3.13. | ✅ verificada | [frontend/package.json](frontend/package.json#L19), [frontend/package.json](frontend/package.json#L39), [frontend/package.json](frontend/package.json#L41), [frontend/package.json](frontend/package.json#L42), [frontend/Dockerfile](frontend/Dockerfile#L1), [backend/Dockerfile](backend/Dockerfile#L1). El backend declara FastAPI/Uvicorn sin fijar versiones ([backend/requirements.txt](backend/requirements.txt#L1)). |
| `App` llama `GET /api/metrics`; Vite reenvía `/api` a `http://backend:8000`. | ✅ verificada | [frontend/src/App.tsx](frontend/src/App.tsx#L16), [frontend/vite.config.ts](frontend/vite.config.ts#L12), [frontend/vite.config.ts](frontend/vite.config.ts#L13). |
| La ruta usada genera movimientos simulados con semilla 42; las fechas dependen de `date.today()`. | ✅ verificada | [backend/app/routes.py](backend/app/routes.py#L255), [backend/app/routes.py](backend/app/routes.py#L94), [backend/app/routes.py](backend/app/routes.py#L97). |
| No hay integración de base de datos en el flujo de datos comprobado. | ✅ verificada | La ruta llama al generador en memoria: [backend/app/routes.py](backend/app/routes.py#L255). La búsqueda en el repo de nombres de drivers/URLs de BD no encontró coincidencias. |
| Endpoints backend: `/health`, `/api/metrics`, `/api/metrics/facets`, `/api/metrics/summary`, `/api/metrics/categories/top`, `/api/metrics/comparison`, `/api/metrics/alerts`, `/api/metrics/b2b`, `/api/metrics/b2c`. | ✅ verificada | Declaraciones en [backend/app/routes.py](backend/app/routes.py#L243), [backend/app/routes.py](backend/app/routes.py#L248), [backend/app/routes.py](backend/app/routes.py#L262), [backend/app/routes.py](backend/app/routes.py#L268), [backend/app/routes.py](backend/app/routes.py#L287), [backend/app/routes.py](backend/app/routes.py#L305), [backend/app/routes.py](backend/app/routes.py#L342), [backend/app/routes.py](backend/app/routes.py#L362), [backend/app/routes.py](backend/app/routes.py#L378). La única llamada encontrada en código del frontend es `/api/metrics` ([frontend/src/App.tsx](frontend/src/App.tsx#L16)). |
| Frontend tests: `npm test` ejecuta `vitest run`; hay pruebas de utilidades. Backend: hay pruebas con `TestClient` y `pytest` en requirements. | ✅ verificada | [frontend/package.json](frontend/package.json#L11), [frontend/src/lib/financial-utils.test.ts](frontend/src/lib/financial-utils.test.ts#L35), [backend/tests/test_routes.py](backend/tests/test_routes.py#L3), [backend/requirements.txt](backend/requirements.txt#L4). |
| La aplicación lee `VITE_API_BASE_URL`; la plantilla `.env.example` existe. | ✅ verificada | [frontend/src/App.tsx](frontend/src/App.tsx#L13), [frontend/.env.example](frontend/.env.example#L4). |

## Correcciones
- ❌ “No encontré `frontend/.env.example`; disponibilidad sin verificar” → el archivo existe y declara `VITE_API_BASE_URL` ([frontend/.env.example](frontend/.env.example#L4)).

## Problemas detectados en el repo
- El encabezado fija el período a `2024 - Full Year`, pero el backend deriva las fechas de `date.today()` ([frontend/src/App.tsx](frontend/src/App.tsx#L49), [backend/app/routes.py](backend/app/routes.py#L65), [backend/app/routes.py](backend/app/routes.py#L97)).
- Hay datos estáticos exportados en `mock-data.ts`, pero la app obtiene datos por API ([frontend/src/lib/mock-data.ts](frontend/src/lib/mock-data.ts#L3), [frontend/src/App.tsx](frontend/src/App.tsx#L16)); la búsqueda de referencias encontró solo la declaración de `mockMovements`.
- Los tests no pudieron ejecutarse en este entorno: `cd frontend && npm test` terminó con `vitest: not found`; `python -m pytest backend` terminó con `No module named pytest`. No se modificaron archivos de código ni configuración.

## Pendiente de verificar
- Si las URLs responden al iniciar servicios; se validó `docker compose config`, pero no se arrancó Compose.
- Si `http://localhost:8000/docs` responde; la URL está en README, pero no se probó contra un backend activo ([README.es.md](README.es.md#L50)).
- Ejecución efectiva de los tests, una vez disponibles Vitest y Pytest. El repo no declara un script de test para backend.

## Validación operativa de las 12 reglas

Se eligió para cada regla una revisión pequeña de su área y se ejecutó su comprobación prescrita o, cuando el host no tenía dependencias, la misma suite dentro de los contenedores del proyecto. No se modificó código de la aplicación. Las seis reglas que necesitaron instrucciones más precisas se editaron y se volvió a ejecutar su check.

| Regla | Qué pide y qué parte afecta | Tarea, comprobación y resultado real | ¿Ayudó? |
|---|---|---|---|
| `check-mock-data-references-before-changing-data-source` | Seguir referencias a mocks y confirmar la fuente real del dashboard (`frontend/src`). | `rg` no está instalado. El fallback nuevo `grep -R -n --exclude-dir=node_modules "mockMovements" frontend/src` halló solo `frontend/src/lib/mock-data.ts:3`; `App.tsx` usa `fetch(.../api/metrics)`. | ✅ Tras añadir fallback para `grep`. |
| `keep-agent-directory-docs-in-sync` | Mantener coherente el árbol de `.agents` con los README. | `find .agents -maxdepth 3 -type f` listó 12 reglas y ningún skill. Los README dicen “estructura esperada”; aclarada esa distinción, la ausencia de `.agents/skills` no contradice lo documentado. | ✅ |
| `keep-compose-api-proxy-target-valid` | Alinear proxy Vite con servicio backend y probar el flujo API (Vite, Compose). | `docker compose config`: válido; host `http://localhost:8000/api/metrics`: HTTP 200; proxy `http://localhost:5173/api/metrics`: HTTP 502. Desde frontend, `backend` resolvió a `172.18.0.2`, pero la conexión a `backend:8000` agotó el tiempo. Se aclaró el criterio y el diagnóstico en la regla; el fallo de runtime persiste. | ✅ Detectó una discrepancia real entre configuración y conectividad. |
| `keep-period-label-aligned-with-generated-data` | Comparar etiqueta y fechas generadas (dashboard/API). | `GET /api/metrics`: 360 filas; fechas `2025-10-02` a `2026-09-28`; `App.tsx` aún muestra `2024 - Full Year`. La comparación detectó el desfase. | ✅ |
| `keep-vite-and-typescript-aliases-aligned` | Mantener igual el alias `@` (Vite/TypeScript). | `docker compose exec frontend npm run build`: TypeScript y Vite completaron; `2290 modules transformed`, build correcto. Hubo aviso de chunk >500 kB. En host faltaba `tsc`. | ✅ |
| `place-frontend-code-by-responsibility` | Separar dashboard, UI reutilizable y lógica/tipos (`frontend/src`). | `find frontend/src/components -maxdepth 2 -type f` mostró componentes dashboard y UI en sus carpetas; revisión de imports confirmó que dashboard consume UI y `lib`. | ✅ |
| `review-cors-origins-and-credentials-together` | Revisar orígenes permitidos junto con credenciales (FastAPI CORS). | Preflight para `http://localhost:5173`: HTTP 200, `Access-Control-Allow-Origin` exacto y `Access-Control-Allow-Credentials: true`. El preflight negativo para `https://example.invalid` también lo autorizó con credenciales. Se añadió esta prueba negativa a la regla y se repitió: confirmó el mismo permiso. | ✅ Detectó política demasiado amplia; no se cambió el backend. |
| `review-debugger-listener-and-port-publication-together` | Revisar listener, publicación y necesidad según entorno (debugpy/Compose). | `backend/Dockerfile` escucha en `0.0.0.0:5678`; `docker compose config` publica `5678:5678`; `docker compose port backend 5678` devolvió `0.0.0.0:5678`. La regla ahora diferencia desarrollo local de producción. | ✅ |
| `test-backend-route-changes` | Acompañar cambios de rutas con pruebas `TestClient` (backend). | En host, `python -m pytest backend/tests` no pudo iniciar: `No module named pytest`. En la imagen: `docker compose exec backend python -m pytest tests` dio `15 passed, 1 warning`. | ✅ |
| `test-frontend-utility-changes` | Probar cambios de cálculos financieros con Vitest (`frontend/src/lib`). | En host, `cd frontend && npm test` dio `vitest: not found`. En el contenedor: `docker compose exec frontend npm test` dio `1` archivo y `5` tests pasados. | ✅ |
| `verify-clean-frontend-dependency-build` | Construir frontend con dependencias instaladas desde cero (Docker), teniendo en cuenta el lockfile. | `frontend/package-lock.json` existe; `frontend/Dockerfile` lo incluye mediante `COPY package*.json ./`. `docker compose build --no-cache frontend` terminó correctamente y ejecutó `npm install`. | ✅ |
| `verify-dashboard-api-loading-states` | Verificar visualmente carga, éxito y error de API (`frontend/src/App.tsx`). | La regla ahora enumera qué observar y cómo bloquear `/api/metrics`. La página frontend respondió HTTP 200, pero no hay navegador ni automatización browser instalados en esta sesión; no pude inspeccionar los estados renderizados. La comprobación visual queda pendiente. | ✅ Guía clara; ejecución visual no disponible aquí. |

### Hallazgos y comprobaciones pendientes

- CORS autoriza cualquier origen observado, incluso `https://example.invalid`, con credenciales. Para corregirlo, primero listar los orígenes confiables del entorno y si la app necesita credenciales; después sustituir el comodín por esa lista explícita y desactivar credenciales si no hacen falta; por último repetir preflight positivo y negativo. No se modificó `backend/app/main.py`.
- El período mostrado no coincide con los datos actuales. Elegir si el dashboard debe representar 2024 o el período móvil generado por API; alinear la etiqueta o la generación de datos con esa decisión y repetir la comparación de fechas. No se modificó código.
- El target del proxy y el servicio coinciden en configuración, pero desde frontend no se alcanza al backend, mientras el acceso directo publicado funciona. Repetir/diagnosticar esta comunicación en un entorno Docker operativo antes de cambiar el target; no se modificó Vite ni Compose.
- `debugpy` queda publicado en todas las interfaces del host. La configuración es para desarrollo local; antes de usarla en producción, retirar la publicación o documentar una necesidad y controles de red explícitos. No se modificó Docker.
- El resultado visual de carga y error no se pudo confirmar sin navegador. Los tests frontend cubren utilidades, no el render de `App`.

### Archivos y entorno

Se editaron seis reglas para aclarar fallback, criterios o pasos: `check-mock-data-references-before-changing-data-source.md`, `keep-agent-directory-docs-in-sync.md`, `keep-compose-api-proxy-target-valid.md`, `review-cors-origins-and-credentials-together.md`, `review-debugger-listener-and-port-publication-together.md` y `verify-dashboard-api-loading-states.md`. No se cambió ningún archivo funcional de frontend o backend. Este apartado registra la validación.

El directorio `.agents/` ya aparecía como no versionado en el `git status` inicial; sus reglas se conservaron. Los servicios iniciados para las pruebas se detuvieron con `docker compose down`, sin borrar volúmenes. No se hizo commit.

### Corrección documental posterior

La validación inicial afirmó incorrectamente que no había `frontend/package-lock.json`. El archivo sí existe (su fecha de modificación observada fue el 4 de octubre de 2026); se corrigieron esa afirmación en esta regla y en `proposed-rules.md`. El Dockerfile copia el lockfile antes de instalar dependencias.

### Auditoría documental del checkout

Se contrastaron las afirmaciones de las 12 reglas con cada archivo fuente citado, además de `AGENTS.md`, ambos README, `proposed-rules.md` y la estructura actual. Las afirmaciones técnicas de las reglas coinciden con el código/configuración revisados; los resultados dinámicos previos (proxy HTTP 502, CORS que refleja orígenes y desfase del período) siguen registrados como hallazgos, no como discrepancias documentales.

Se corrigieron dos afirmaciones de estado que ya no correspondían al checkout: `proposed-rules.md` decía que las reglas aún no estaban incorporadas, aunque las 12 existen en `.agents/rules`; y la justificación de `keep-agent-directory-docs-in-sync` describía esas carpetas como ausentes. El README etiqueta su árbol como estructura esperada. Actualmente `.agents/rules` contiene 12 archivos, `.agents/skills` no existe y tampoco existe `memory-bank`; `AGENTS.md` menciona skills disponibles y condiciona la memoria a que exista.