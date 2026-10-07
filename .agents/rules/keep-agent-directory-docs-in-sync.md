# Nombre
`keep-agent-directory-docs-in-sync`

## Alcance
Adición o eliminación de carpetas bajo `.agents` y documentación de su estructura.

## Justificación
`README.es.md` describe `.agents/rules` y `.agents/skills`, aunque las reglas propuestas señalan que esas carpetas no estaban presentes en el checkout revisado. Una estructura documentada distinta de la real dificulta encontrar las instrucciones.

## Guía específica del proyecto
- Cuando añadas o retires carpetas bajo `.agents`, actualiza el árbol documentado en `README.es.md` y cualquier estructura equivalente en `README.md`.
- Compara el árbol documentado con `find .agents -maxdepth 3 -type f` desde la raíz y aclara si el README describe la estructura esperada o la que existe actualmente.
- Si el README dice "esperada", una carpeta opcional ausente no es una contradicción; actualiza el árbol solo cuando cambie la estructura objetivo. Si afirma describir el checkout actual, refleja exactamente las carpetas y archivos presentes.
- Mantén los archivos de reglas en `.agents/rules`, la ruta que `AGENTS.md` indica revisar.
- No describas una ruta como ubicación de descubrimiento automático si las instrucciones del proyecto apuntan a otra.