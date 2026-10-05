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