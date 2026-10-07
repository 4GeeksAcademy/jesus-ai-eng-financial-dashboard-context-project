# Nombre
`verify-clean-frontend-dependency-build`

## Alcance
Modificaciones de dependencias del frontend o de su instalación en la imagen Docker.

## Justificación
`frontend/package-lock.json` existe junto a `frontend/package.json`, y `frontend/Dockerfile` copia ambos con `COPY package*.json ./` antes de ejecutar `npm install`. La compilación sin caché comprueba que la instalación limpia funciona con los manifiestos presentes, incluido el lockfile.

## Guía específica del proyecto
- Cuando modifiques dependencias frontend, revisa `frontend/package.json`, `frontend/package-lock.json` y su instalación en `frontend/Dockerfile`; actualiza el manifiesto y el lockfile de forma coherente.
- Desde la raíz, ejecuta `docker compose build --no-cache frontend` para comprobar una instalación y construcción limpias.
- Comprueba que el proceso termine correctamente. La instrucción `COPY package*.json ./` debe incluir el lockfile; una instalación local previa no sustituye esta verificación.