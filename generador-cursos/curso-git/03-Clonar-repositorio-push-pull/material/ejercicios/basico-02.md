# Básico 02 — Descargar cambios con pull

## Estado inicial

Dos copias de tu repositorio `diario` (el de la Clase 02), clonadas en carpetas
distintas: la copia A (tuya) y la copia B (simula a otra persona). Ambas están
sincronizadas con el remoto y `pull.rebase` es `false`.

## Proceso esperado

1. En la copia B, agregar una entrada nueva al diario, confirmarla y publicarla.
2. En la copia A, comprobar que el archivo todavía no tiene la entrada nueva.
3. En la copia A, descargar los cambios del remoto y verificar que la entrada y su
   commit aparecen.
4. Descargar los cambios una segunda vez y anotar qué responde Git.

## Resultado esperado

- Tras el paso 3, el archivo de la copia A contiene la entrada nueva y `git log
  --oneline` muestra el commit de la copia B.
- Tras el paso 4, Git informa que no hay nada nuevo por descargar.
- Podés explicar por qué en el paso 2 la copia A aún no veía la entrada.

## Restricciones

- No editar el archivo de la copia A para "copiar" la entrada a mano.
- Usar mensajes de commit descriptivos (no "cambios" ni "update").

## Resultados de aprendizaje

- RA-2: Descargar actualizaciones del repositorio remoto al repositorio local.
