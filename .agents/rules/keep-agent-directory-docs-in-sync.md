# Nombre
`keep-agent-directory-docs-in-sync`

## Alcance
Adición o eliminación de carpetas bajo `.agents` y documentación de su estructura.

## Justificación
`README.es.md` documenta una estructura esperada con `.agents/rules` y `.agents/skills`. En el checkout actual existe `.agents/rules` con 12 archivos y no existe `.agents/skills`; `AGENTS.md` indica que se revisen las reglas y las skills disponibles. La estructura esperada del README no debe confundirse con un inventario de carpetas presentes.

## Guía específica del proyecto
- Cuando añadas o retires carpetas bajo `.agents`, actualiza el árbol documentado en `README.es.md` y cualquier estructura equivalente en `README.md`.
- Compara el árbol documentado con `find .agents -maxdepth 3 -type f` desde la raíz y aclara si el README describe la estructura esperada o la que existe actualmente.
- Si el README dice "esperada", una carpeta opcional ausente no es una contradicción; actualiza el árbol solo cuando cambie la estructura objetivo. Si afirma describir el checkout actual, refleja exactamente las carpetas y archivos presentes.
- Mantén los archivos de reglas en `.agents/rules`, la ruta que `AGENTS.md` indica revisar.
- No describas una ruta como ubicación de descubrimiento automático si las instrucciones del proyecto apuntan a otra.