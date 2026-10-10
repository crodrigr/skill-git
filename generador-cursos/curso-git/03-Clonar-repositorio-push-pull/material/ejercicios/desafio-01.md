# Desafío 01 — Colaborar con dos copias: divergencia, conflicto y reversión

## Estado inicial

Las dos copias de `diario` (A tuya, B de otra persona), sincronizadas con el remoto en
`master`. El archivo `diario.txt` tiene un título y al menos dos entradas.
`pull.rebase` es `false`.

## Proceso esperado

1. **Divergencia con conflicto.** Sin sincronizar entre sí, editar **la misma línea**
   del archivo en la copia A y en la copia B, y confirmar en ambas. Publicar primero
   desde B. Intentar publicar desde A, descargar los cambios, resolver el conflicto
   dejando una versión conjunta de la línea y completar el envío.
2. **Error ya publicado.** En la copia A, agregar una entrada con un dato incorrecto,
   confirmarla, y a continuación confirmar un segundo cambio que **modifique esa misma
   línea** (por ejemplo, ampliarla). Enviar ambos commits.
3. **Reversión con conflicto.** Revertir el primero de esos dos commits (el erróneo).
   Resolver el conflicto que aparezca de modo que la línea incorrecta desaparezca y
   completar la reversión.
4. Enviar la reversión y comprobar, en la copia B, que se recibe con una descarga.
5. Mostrar el historial final de la copia B.

## Resultado esperado

- El historial remoto contiene los commits de ambas copias, el commit de fusión del
  paso 1, los dos commits del paso 2 y el commit de reversión del paso 3.
- El archivo final, en ambas copias, no contiene ni la línea incorrecta ni su
  ampliación, y conserva la versión conjunta del paso 1.
- Ningún commit se perdió y no se reescribió historial.

## Restricciones

- Prohibido forzar el envío y borrar commits del historial.
- Los mensajes de commit deben ser descriptivos.
- Antes de continuar tras cada conflicto, comprobar el estado del repositorio.

## Resultados de aprendizaje

- RA-1: Clonar un proyecto desde un repositorio remoto en la máquina local.
- RA-2: Descargar actualizaciones del repositorio remoto al repositorio local.
- RA-3: Enviar cambios del repositorio local al repositorio remoto.
- RA-4: Revertir cambios en el repositorio local y en el remoto.

## Casos de prueba

| Acción | Resultado esperado |
|---|---|
| Intento de envío desde A en el paso 1 | Rechazado: el remoto tiene un commit que A no tiene |
| `git pull` en A en el paso 1 | Conflicto de contenido en `diario.txt` |
| `git log --oneline` en el remoto tras el paso 1 | Incluye un commit de fusión y los commits de A y B |
| Revertir el primer commit del paso 2 | Conflicto, porque el segundo commit modificó la misma línea |
| `git status` durante el conflicto de reversión | Muestra `diario.txt` con cambios sin resolver |
| `git log --oneline` en B tras el paso 4 | El commit de reversión aparece encima de los dos commits del paso 2 |
| `cat diario.txt` en B | No contiene la línea incorrecta ni su ampliación |
