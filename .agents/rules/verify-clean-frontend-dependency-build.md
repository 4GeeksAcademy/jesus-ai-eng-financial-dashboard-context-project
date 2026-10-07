# Nombre
`verify-clean-frontend-dependency-build`

## Alcance
Modificaciones de dependencias del frontend o de su instalación en la imagen Docker.

## Justificación
Las reglas propuestas señalan que `frontend/Dockerfile` usa `npm install`, `frontend/package.json` declara rangos de versión y no había un lockfile en el checkout revisado. Una instalación sin caché puede resolver dependencias distintas a las disponibles localmente.

## Guía específica del proyecto
- Cuando modifiques dependencias frontend, revisa `frontend/package.json` y su instalación en `frontend/Dockerfile`.
- Desde la raíz, ejecuta `docker compose build --no-cache frontend` para comprobar una instalación y construcción limpias.
- Comprueba que el proceso termine correctamente; una instalación local previa no sustituye esta verificación.