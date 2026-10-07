# Stack tecnológico

## Lenguajes y frameworks

- ✅ Frontend: TypeScript y React; backend: Python y FastAPI. Fuentes: [README.es.md](../README.es.md#L18), [package.json](../frontend/package.json#L19), [backend/Dockerfile](../backend/Dockerfile#L1), [requirements.txt](../backend/requirements.txt#L1).

## Infraestructura y herramientas

- ✅ Docker Compose define frontend y backend; configura la publicación de los puertos 5173, 8000 y 5678. Fuente: [docker-compose.yml](../docker-compose.yml#L1), [docker-compose.yml](../docker-compose.yml#L7), [docker-compose.yml](../docker-compose.yml#L19), [docker-compose.yml](../docker-compose.yml#L20).
- ✅ Imágenes: Node 24 Alpine y Python 3.13 slim. Fuentes: [frontend/Dockerfile](../frontend/Dockerfile#L1), [backend/Dockerfile](../backend/Dockerfile#L1).
- ✅ Vite configura el host del frontend, reenvía `/api` a `backend:8000` y dirige `@` a `src`; TypeScript declara el mismo alias. Fuentes: [vite.config.ts](../frontend/vite.config.ts#L7), [vite.config.ts](../frontend/vite.config.ts#L13), [vite.config.ts](../frontend/vite.config.ts#L20), [tsconfig.app.json](../frontend/tsconfig.app.json#L13).

## Dependencias clave

- ✅ Frontend: React/React DOM `^19.2.4`, Recharts `^3.8.1`, TypeScript `~6.0.2`, Vite `^8.0.4` y Vitest `^4.1.4`. Fuente: [package.json](../frontend/package.json#L19), [package.json](../frontend/package.json#L21), [package.json](../frontend/package.json#L39), [package.json](../frontend/package.json#L41), [package.json](../frontend/package.json#L42).
- ✅ Backend: FastAPI, Uvicorn, debugpy, pytest y httpx. Fuente: [requirements.txt](../backend/requirements.txt#L1), [requirements.txt](../backend/requirements.txt#L2), [requirements.txt](../backend/requirements.txt#L3), [requirements.txt](../backend/requirements.txt#L4), [requirements.txt](../backend/requirements.txt#L6).
