# Intermedio 01 — Conflicto al hacer pull: resolver, publicar y abortar

## Estado inicial

Dos copias de tu repositorio `diario` en carpetas distintas: la copia A (tuya) y la copia
B (simula a otra persona). Ambas están sincronizadas con el remoto en `master` y
`pull.rebase` es `false`.

## Proceso esperado

1. **Provocar el conflicto.** Sin sincronizar entre sí, cambiar **la misma línea** de
   `diario.txt` en las dos copias, confirmar en ambas y publicar primero desde B.
2. En la copia A, intentar publicar y anotar qué dice Git.
3. Traer los cambios del remoto y comprobar qué archivo quedó en conflicto y de qué lado
   viene cada versión.
4. Resolver el conflicto dejando una versión que combine ambos lados, comprobar que no
   quedan marcadores, completar la fusión y publicarla.
5. **Repetir y abortar.** Provocar un segundo conflicto con otra línea y, esta vez, en
   lugar de resolverlo, abortar la fusión.
6. Verificar que, después de abortar, tu commit sigue en el historial y el archivo
   conserva tu versión.

## Resultado esperado

- Después del paso 4, el remoto contiene los commits de A y de B más el commit de fusión.
- Después del paso 6, el repositorio está como antes del segundo `pull`: sin marcadores,
  sin fusión en curso y con tu commit intacto.
- Podés explicar por qué el primer envío fue rechazado y qué indican `ahead` y `behind`.

## Restricciones

- Prohibido forzar el envío.
- Antes de completar la fusión, comprobar que no quedan marcadores en el archivo.
- Resolver editando el archivo, no copiando un archivo entero.

## Resultados de aprendizaje

- RA-1: Resolver los conflictos que surgen al trabajar con repositorios locales y remotos.
