# Estado actual

Estado: ✅ indica una afirmación contrastada con su fuente; los resultados de ejecución son los que `verification.md` registra y no se han vuelto a ejecutar en esta revisión.

## Qué funciona

- ✅ `docker compose config` y la build limpia de la imagen frontend terminaron correctamente, según el registro de verificación. Fuentes: [verification.md](../verification.md#L42), [verification.md](../verification.md#L50).
- ✅ Según el registro, en contenedores pasaron 15 pruebas backend y 5 frontend. Fuentes: [verification.md](../verification.md#L48), [verification.md](../verification.md#L49).
- ✅ Según el registro, el build TypeScript/Vite terminó con 2290 módulos y avisó de un chunk mayor de 500 kB. Fuente: [verification.md](../verification.md#L44).
- ✅ Según el registro, la API directa respondió HTTP 200 y devolvió 360 registros entre `2025-10-02` y `2026-09-28`. Fuentes: [verification.md](../verification.md#L42), [verification.md](../verification.md#L43).

## Problemas conocidos

- ✅ El proxy de Vite dio HTTP 502 aunque la API directa respondió 200; desde frontend la conexión a `backend:8000` agotó el tiempo. Fuente: [verification.md](../verification.md#L42).
- ✅ CORS autorizó `https://example.invalid` con credenciales. Fuentes: [verification.md](../verification.md#L46), [main.py](../backend/app/main.py#L9), [main.py](../backend/app/main.py#L10).
- ✅ El encabezado fija 2024, pero los datos observados corresponden a octubre de 2025-septiembre de 2026. Fuentes: [App.tsx](../frontend/src/App.tsx#L49), [verification.md](../verification.md#L43).
- ✅ `mockMovements` solo aparece en su declaración; la pantalla usa la API. Fuentes: [verification.md](../verification.md#L40), [mock-data.ts](../frontend/src/lib/mock-data.ts#L3), [App.tsx](../frontend/src/App.tsx#L16).
- ✅ `debugpy` se publica en todas las interfaces del host en el puerto 5678. Fuentes: [verification.md](../verification.md#L47), [docker-compose.yml](../docker-compose.yml#L20).
- ✅ El render de carga/error no se verificó visualmente; el build avisó de un chunk >500 kB. Fuentes: [verification.md](../verification.md#L51), [verification.md](../verification.md#L44).
- ✅ Los tests no arrancaron en el host por falta de pytest/Vitest, pero pasaron en contenedores. Fuentes: [verification.md](../verification.md#L48), [verification.md](../verification.md#L49).

## Siguientes prioridades

- ✅ Diagnosticar la conectividad del proxy antes de cambiar su destino. Fuente: [verification.md](../verification.md#L57).
- ✅ Definir orígenes confiables y necesidad de credenciales; repetir preflight positivo y negativo. Fuente: [verification.md](../verification.md#L55).
- ✅ Decidir si el dashboard debe representar 2024 o el período generado y alinear ambos. Fuente: [verification.md](../verification.md#L56).
- ✅ Antes de usar fuera de desarrollo local, retirar la publicación del debugger o documentar necesidad y controles de red. Fuente: [verification.md](../verification.md#L58).
- ✅ Completar la verificación visual de carga y error cuando haya navegador disponible. Fuentes: [verification.md](../verification.md#L51), [verify-dashboard-api-loading-states.md](../.agents/rules/verify-dashboard-api-loading-states.md#L11).
