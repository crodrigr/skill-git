# Taller 01 — Conflictos y archivos ignorados en tu diario

## Objetivo

Aplicar en un proyecto real los dos resultados de la clase: resolver conflictos con
repositorios locales y remotos (RA-1) y controlar qué archivos se rastrean (RA-2).

## Contexto

Seguirás trabajando sobre el repositorio `diario` de la Clase 03 y sus dos copias locales
en carpetas distintas (copia A y copia B), que simulan a dos personas colaborando.

## Pasos

1. Verificá que `pull.rebase` esté en `false` y que tu token de acceso a GitHub siga
   vigente.
2. En la copia A, creá un `.gitignore` que ignore los archivos `.log` y una carpeta de
   salida de tu elección, confirmalo y publicalo.
3. En la copia B, traé el `.gitignore` con pull.
4. Editá **la misma línea** de un archivo en ambas copias, publicá primero desde una y
   provocá el conflicto en la otra al hacer pull.
5. Resolvé el conflicto, comprobá que no quedan marcadores, completá la fusión y
   publicala.
6. Repetí el conflicto con otra línea, pero esta vez abortá la fusión y verificá que
   volviste al estado previo.
7. Confirmá por descuido un archivo `credenciales.txt` con contenido ficticio (escribí en
   él "NO REAL"), dejá de rastrearlo sin borrarlo del disco, agregalo al `.gitignore`,
   confirmá y publicá.
8. Con `git check-ignore -v`, comprobá qué regla ignora ese archivo.

## Entregable

Tu repositorio `diario` en GitHub con un `.gitignore` versionado, una fusión con conflicto
publicada y un archivo que dejaste de rastrear; `git status` limpio en ambas copias.

## Criterios de evaluación

- El conflicto se resolvió sin dejar marcadores y la fusión se publicó.
- El aborto devolvió el repositorio al estado previo sin perder trabajo.
- `git status` queda limpio y el archivo sensible se conserva en disco sin rastrearse.
- Se identificó la regla que ignora el archivo.
- Nunca se usaron credenciales reales ni se forzó un envío.
