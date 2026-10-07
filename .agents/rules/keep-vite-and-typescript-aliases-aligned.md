# Nombre
`keep-vite-and-typescript-aliases-aligned`

## Alcance
Cambios del alias `@` en la configuración de TypeScript o Vite del frontend.

## Justificación
`frontend/tsconfig.app.json` y `frontend/vite.config.ts` configuran `@` para apuntar a `src`. Si sus destinos divergen, la resolución de imports del compilador y del bundler puede dejar de coincidir.

## Guía específica del proyecto
- Cuando cambies el alias `@`, actualiza tanto `frontend/tsconfig.app.json` como `frontend/vite.config.ts`.
- Mantén ambos destinos apuntando a la misma ubicación y revisa los imports afectados.
- Verifica el cambio con `cd frontend && npm run build`.