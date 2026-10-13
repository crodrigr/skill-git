# Desafío 01 — Conflictos y archivos ignorados en un mismo proyecto

## Estado inicial

Dos copias de tu repositorio `diario` (A tuya, B de otra persona), sincronizadas con el
remoto en `master`. `pull.rebase` es `false`. En el repositorio ya se confirmó y se
publicó por descuido un archivo `credenciales.txt` con contenido ficticio (**NO REAL**),
y además hay archivos `.log` sin confirmar en cada copia.

## Proceso esperado

1. **Conflicto en pull.** Cambiar la misma línea de `diario.txt` en las dos copias,
   publicar primero desde B, y traer los cambios en A. Resolver el conflicto combinando
   ambos lados, comprobar que no quedan marcadores y publicar la fusión.
2. **Conflicto al revertir.** En A, confirmar un cambio con un dato incorrecto y, a
   continuación, un segundo cambio sobre **la misma línea**. Publicar ambos. Revertir el
   primero, resolver el conflicto que aparezca y completar la reversión.
3. **Ignorar archivos.** Crear un `.gitignore` que ignore los archivos `.log` y
   `credenciales.txt`. Dejar de rastrear `credenciales.txt` **sin borrarlo del disco**,
   confirmar y publicar.
4. En la copia B, traer los cambios y comprobar que `.gitignore` llegó y que
   `git status` queda limpio.
5. Averiguar qué regla ignora un archivo `.log` y explicar por escrito por qué
   `credenciales.txt` sigue estando en el historial del remoto y qué habría que hacer con
   esa credencial si fuera real.

## Resultado esperado

- El historial remoto contiene los commits de ambas copias, el commit de fusión del
  paso 1, los dos commits del paso 2, el commit de reversión y el commit que dejó de
  rastrear el archivo.
- `diario.txt` no contiene la línea incorrecta del paso 2 ni su ampliación.
- `credenciales.txt` existe en el disco de A, pero ya no está rastreado, y `.gitignore`
  está versionado en el remoto.
- La explicación del paso 5 es correcta y no propone reescribir el historial.

## Restricciones

- Prohibido forzar el envío, borrar commits y reescribir el historial.
- Los mensajes de commit deben ser descriptivos.
- Antes de cada confirmación posterior a un conflicto, comprobar que no quedan marcadores.
- No usar credenciales reales.

## Resultados de aprendizaje

- RA-1: Resolver los conflictos que surgen al trabajar con repositorios locales y remotos.
- RA-2: Evitar que ciertos archivos sean rastreados por Git, y corregir el caso de los ya rastreados.

## Casos de prueba

| Acción | Resultado esperado |
|---|---|
| `git pull` en A en el paso 1 | Conflicto de contenido en `diario.txt` |
| `git diff --check` antes de completar la fusión | Sin avisos de marcadores |
| `git revert` del primer commit del paso 2 | Conflicto, porque el segundo commit modificó la misma línea |
| `git status -sb` durante el conflicto de reversión | `diario.txt` aparece con `UU` |
| `git ls-files` en A tras el paso 3 | No incluye `credenciales.txt` pero sí `.gitignore` |
| `ls` en A tras el paso 3 | `credenciales.txt` sigue existiendo |
| `git status` en B tras el paso 4 | Limpio, con `.gitignore` presente |
| `git check-ignore -v` sobre un `.log` | Muestra el archivo `.gitignore`, la línea y el patrón |
