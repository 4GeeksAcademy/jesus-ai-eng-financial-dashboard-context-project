# Nombre
`place-frontend-code-by-responsibility`

## Alcance
Creación o reorganización de componentes, tipos y cálculos en `frontend/src`.

## Justificación
`frontend/src/App.tsx` compone componentes de `components/dashboard` y lógica de `lib`. `kpi-row.tsx` reutiliza otros componentes del dashboard. Mantener esta separación evita mezclar presentación específica, elementos reutilizables y cálculos financieros.

## Guía específica del proyecto
- Coloca las piezas específicas del dashboard en `frontend/src/components/dashboard`.
- Coloca los componentes visuales reutilizables en `frontend/src/components/ui`.
- Coloca los tipos y cálculos compartidos en `frontend/src/lib`.
- Comprueba que los archivos nuevos estén en la carpeta correspondiente y que sus imports respeten estas responsabilidades.